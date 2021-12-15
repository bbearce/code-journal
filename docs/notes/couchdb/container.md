# From Container

> Source [docker](https://hub.docker.com/_/couchdb?tab=description)

```bash
$ docker pull couchdb
```

This will get you latest. Then as per their instructoins run this command:


```bash
$ docker run -d --name my-couchdb couchdb:tag
```

If you want to expose the port to the outside world, run

```bash
$ docker run -p 5984:5984 -d couchdb
```

Start not in *Admin Party* mode:

```bash
$ docker run -e COUCHDB_USER=admin -e COUCHDB_PASSWORD=password -d couchdb
```

Example I used for a simple project:

```bash
$ docker run -d --name ben-couchdb2 -e COUCHDB_USER=admin -e COUCHDB_PASSWORD=password -p 5985:5984 couchdb:latest
```

