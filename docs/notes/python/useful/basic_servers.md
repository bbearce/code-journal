# Some Quick Python Based Servers

> Update [Anvileight has a better article covering more topics](https://blog.anvileight.com/posts/simple-python-http-server/)

> Courtesy of [2ality](https://2ality.com/2014/06/simple-http-server.html)

*Below notes are from second link*

## Python2

**SimpleHTTPServer: a quick way to serve a directory**

### Using SimpleHTTPServer  
SimpleHTTPServer is invoked like this (the parameter <port> is optional):

```bash
python -m SimpleHTTPServer <port>
```

(On OS X, Python is pre-installed and this command works out of the box.)

Let’s look at an example of using SimpleHTTPServer: During the following Unix shell interaction, I first list the files in the current directory and then start SimpleHTTPServer to serve it.

```bash
$ ls .
foo.html
$ python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 ...
```

Afterwards, I can access the following URLs:

* http://localhost:8000/ lists the files in the current directory (namely, just foo.html). If there were a file index.html, it would be displayed, instead.  
* http://localhost:8000/foo.html displays the file foo.html in the current directory.  


### Customizing SimpleHTTPServer  

The following Unix shell script demonstrates how to customize SimpleHTTPServer so that it serves files that have a given file name extension with a given media type. One case where that matters is Firefox being picky about the media type of the webapp.manifest.

```python
#!/usr/bin/python

import SimpleHTTPServer
import SocketServer

PORT = 8000

Handler = SimpleHTTPServer.SimpleHTTPRequestHandler
Handler.extensions_map.update({
    '.webapp': 'application/x-web-app-manifest+json',
});

httpd = SocketServer.TCPServer(("", PORT), Handler)

print "Serving at port", PORT
httpd.serve_forever()
```

## Python3

In python 3 you can run:
```python
python3 -m http.server 8080
```

to create a server that will serve to the folder you are currntly in.