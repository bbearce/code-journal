# Docker Compose

This page is for using docker-compose which allows you to deploy multiple containers in a config file. 

The details of using the ```docker-compose``` command are below as well as a lengthy example using [codalab's](https://github.com/codalab/codalab-competitions/blob/develop/.env_sample) .

```
Define and run multi-container applications with Docker.

Usage:
  docker-compose [-f <arg>...] [options] [COMMAND] [ARGS...]
  docker-compose -h|--help

Options:
  -f, --file FILE             Specify an alternate compose file
                              (default: docker-compose.yml)
  -p, --project-name NAME     Specify an alternate project name
                              (default: directory name)
  --verbose                   Show more output
  --log-level LEVEL           Set log level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
  --no-ansi                   Do not print ANSI control characters
  -v, --version               Print version and exit
  -H, --host HOST             Daemon socket to connect to

  --tls                       Use TLS; implied by --tlsverify
  --tlscacert CA_PATH         Trust certs signed only by this CA
  --tlscert CLIENT_CERT_PATH  Path to TLS certificate file
  --tlskey TLS_KEY_PATH       Path to TLS key file
  --tlsverify                 Use TLS and verify the remote
  --skip-hostname-check       Don't check the daemon's hostname against the
                              name specified in the client certificate
  --project-directory PATH    Specify an alternate working directory
                              (default: the path of the Compose file)
  --compatibility             If set, Compose will attempt to convert keys
                              in v3 files to their non-Swarm equivalent
  --env-file PATH             Specify an alternate environment file

Commands:
  build              Build or rebuild services
  config             Validate and view the Compose file
  create             Create services
  down               Stop and remove containers, networks, images, and volumes
  events             Receive real time events from containers
  exec               Execute a command in a running container
  help               Get help on a command
  images             List images
  kill               Kill containers
  logs               View output from containers
  pause              Pause services
  port               Print the public port for a port binding
  ps                 List containers
  pull               Pull service images
  push               Push service images
  restart            Restart services
  rm                 Remove stopped containers
  run                Run a one-off command
  scale              Set number of containers for a service
  start              Start services
  stop               Stop services
  top                Display the running processes
  unpause            Unpause services
  up                 Create and start containers
  version            Show the Docker-Compose version information

```

## Useful Tid Bits

### Restarting\Rebuilding One Container

By default ```docker-compose up``` will build containers based upon the ```*.yml``` definition. Usually if you build a new version of an image you want to be one of the composed containers you'd have to run ```docker-compose down```, which kills and deletes all containers defined in the ```*.yml```. 

It can annoying and time consuming to do this for all containers when only one container needs updating. 

Use this code to refresh and rebuild only the container you need:

```bash
$ docker-compose up -d --no-deps --build <service_name>
```
> ```--no-deps``` - Don't start linked services.

> ```--build``` - Build images before starting containers.




## Example docker-compose.yml
Here is an example:

```bash
version: '2'
services:
  # --------------------------------------------------------------------------
  # HTTP Server
  # --------------------------------------------------------------------------
  nginx:
    image: nginx
    ports:
      - ${NGINX_PORT}:${NGINX_PORT}
      - ${SSL_PORT}:${SSL_PORT}
    command: bash -x /app/docker/run_nginx.sh
    volumes:
      - ./certs:/app/certs
      - ./docker:/app/docker
      - ./codalab:/app/codalab
      - ${LOGGING_DIR}/nginx:/var/log/nginx/
    env_file: .env
    links:
      - django:django
    logging:
      options:
        max-size: "200k"
    container_name: nginx


  # --------------------------------------------------------------------------
  # Database
  # --------------------------------------------------------------------------
  postgres:
    image: postgres:9.6.3
    environment:
      - POSTGRES_DB=${DB_NAME}
      - POSTGRES_USER=${DB_USER}
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - ./docker:/app/docker
      - ${LOGGING_DIR}/psql:/var/log/psql
      - ${DB_DATA_PATH}:/var/lib/postgresql/data
      - ./backups:/app/backups
    env_file: .env
    logging:
      options:
        max-size: "200k"
    container_name: postgres


  # --------------------------------------------------------------------------
  # Message queue
  # --------------------------------------------------------------------------
  rabbit:
    build:
      # Custom Dockerfile for rabbit mostly to make building faster because
      # of envsubst dependency
      context: .
      dockerfile: docker/rabbitmq/Dockerfile
    hostname: rabbit
    command: bash -x /app/docker/run_rabbitmq.sh
    environment:
      - RABBITMQ_LOGS=/var/log/rabbitmq/output.log
      - RABBITMQ_SASL_LOGS=/var/log/rabbitmq/output_sasl.log
    env_file: .env
    volumes:
      - ./docker:/app/docker
      - ./certs:/app/certs
      - ${LOGGING_DIR}/rabbitmq:/var/log/rabbitmq
      - ./var/data/rabbitmq/:/var/lib/rabbitmq/mnesia
    ports:
      - ${RABBITMQ_PORT}:${RABBITMQ_PORT}
      - ${RABBITMQ_MANAGEMENT_PORT}:${RABBITMQ_MANAGEMENT_PORT}
    logging:
      options:
        max-size: "200k"
    container_name: rabbit

  flower:
    build:
      context: .
      dockerfile: docker/flower/Dockerfile
    hostname: flower
    ports:
      - ${FLOWER_PORT}:${FLOWER_PORT}
    environment:
      # These aren't set in .env
      - AMQP_USERNAME=${RABBITMQ_DEFAULT_USER}
      - AMQP_PASSWORD=${RABBITMQ_DEFAULT_PASS}
      - AMQP_HOST=rabbit
      - AMQP_PORT=${RABBITMQ_PORT}
      - FLOWER_CERTFILE=${SSL_CERTIFICATE}
      - FLOWER_KEYFILE=${SSL_CERTIFICATE_KEY}
    volumes:
      - ./certs:/app/certs
    env_file: .env
    links:
      - rabbit
    logging:
      options:
        max-size: "200k"
    container_name: flower


  # --------------------------------------------------------------------------
  # Cache
  # --------------------------------------------------------------------------
  memcached:
    image: memcached
    hostname: memcached
    command: "/usr/local/bin/memcached -u memcache"
    logging:
      options:
        max-size: "200k"
    container_name: memcached


  # --------------------------------------------------------------------------
  # Django
  # --------------------------------------------------------------------------
  django:
    build:
      context: .
      dockerfile: Dockerfile
    hostname: django
    ports:
      - ${DJANGO_PORT}:${DJANGO_PORT}
    command: bash /app/docker/run_django.sh
    volumes:
      - ./certs:/app/certs
      - ./codalab:/app/codalab
      - ./docker:/app/docker
      - ${LOGGING_DIR}/django:/var/log/django/
      - ./backups:/app/backups
    env_file: .env
    environment:
      - CONFIG_SERVER_NAME=${CODALAB_SITE_DOMAIN}
      - PYTHONUNBUFFERED=1
    links:
      - postgres
      - rabbit
      - memcached
    logging:
      options:
        max-size: "200k"
    container_name: django
    

  # --------------------------------------------------------------------------
  # Celery Workers
  # --------------------------------------------------------------------------
  worker_site:
    build:
      context: .
      dockerfile: Dockerfile
    command: sh /app/docker/run_site.sh
    volumes:
      - ./codalab:/app/codalab
      - ./docker:/app/docker
      - ${LOGGING_DIR}/worker_site:/var/log/
    environment:
      # Stop memory leaks
      - DEBUG=False
      - REQUESTS_CA_BUNDLE=/usr/local/lib/python2.7/site-packages/certifi/cacert.pem
    env_file: .env
    links:
      - postgres
      - rabbit
    logging:
      options:
        max-size: "200k"
    container_name: worker_site
      
  worker_compute:
    image: codalab/competitions-v1-compute-worker:1.1.7
    privileged: true
    volumes:
      - ${LOGGING_DIR}/worker_compute:/var/log/
      - ${SUBMISSION_TEMP_DIR}:${SUBMISSION_TEMP_DIR}
      - /var/run/docker.sock:/var/run/docker.sock
    env_file: .env
    links:
      - rabbit
    logging:
      options:
        max-size: "200k"
    mem_limit: 1g
    memswap_limit: 1g
    container_name: worker_compute
```