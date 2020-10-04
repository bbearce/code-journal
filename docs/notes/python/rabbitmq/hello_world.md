# Hello World

> Source: [rabbitmq.com](https://www.rabbitmq.com/tutorials/tutorial-one-python.html)

**RabbitMQ libraries**

RabbitMQ speaks multiple protocols. This tutorial uses AMQP 0-9-1, which is an open, general-purpose protocol for messaging. There are a number of clients for RabbitMQ in many different languages. In this tutorial series we're going to use Pika 1.0.0, which is the Python client recommended by the RabbitMQ team. To install it you can use the pip package management tool:

```bash
python -m pip install pika --upgrade
```

Our first program *send.py* will send a single message to the queue. The first thing we need to do is to establish a connection with RabbitMQ server.


send.py:
```bash
#!/usr/bin/env python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Next, before sending we need to make sure the recipient queue exists
channel.queue_declare(queue='hello')

# In RabbitMQ a message can never be sent directly to the queue, it always needs to go through an exchange.
# The queue name needs to be specified in the routing_key parameter:
channel.basic_publish(exchange='',
                      routing_key='hello',
                      body='Hello World!')
print(" [x] Sent 'Hello World!'")

# Before exiting the program we need to make sure the 
# network buffers were flushed and our message was actually delivered to RabbitMQ. 
# We can do it by gently closing the connection.
connection.close()
```

receive.py:
```bash
#!/usr/bin/env python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# The next step, just like before, is to make sure that the queue exists. 
# Creating a queue using queue_declare is idempotent ‒ 
# - we can run the command as many times as we like, and only one will be created.
channel.queue_declare(queue='hello')

# Receiving messages from the queue is more complex. 
# It works by subscribing a callback function to a queue. 
# Whenever we receive a message, this callback function is called by
# the Pika library. In our case this function will print on the screen the contents of the message.

def callback(ch, method, properties, body):
    print(" [x] Received %r" % body)

channel.basic_consume(queue='hello',
                      auto_ack=True,
                      on_message_callback=callback)

# And finally, we enter a never-ending loop that waits for data and runs callbacks whenever necessary.
print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()
```