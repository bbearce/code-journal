# Chapter 1

> Source [github](https://github.com/PacktPublishing/Learn-D3.js/)

## What is D3?
D3 Stands for Data Driven Documents. It is a javascript library but not a charting library. It focuses on the data. It will replace the DOM API and libraries such as jQuery. It uses data to drive everything. Data is first. "This API is also used to bind and dispatch events, and to generate animated transitions. It can also parse different data formats, such as JSON and CSV, perform general data manipulation on objects and arrays.

A typical D3.js script uses CSS selectors to select HTML or SVG elements and binds them to individual data items, removing, updating, or appending graphical elements automatically, when necessary. From the intro it seems you use CSS to create a selection object on a group of HTML tags and then bind data to them. You can also make a selection bound to data when the HTML doen't exist yet and have the HTML created automatically.

Use [https://d3js.org/](https://d3js.org/) and click on any of the hexagon example to be taken to a tutorial. Two other platforms are [https://www.bl.ocks.org](https://www.bl.ocks.org) and [https://www.observablehq.com](https://www.observablehq.com) which contain interactive tutorials.

## Environment Setup

### npm
```bash
$ npm install d3
```

From there in my home directory is:
```bash
$ ls node_modules/d3/dist/
d3.js  d3.min.js  d3.node.js  package.js
```

Just source the *min* after copying it to your development directory as follows:
```html
<script src="js/d3/d3.v5.min.js"></script>
```

### CDN link

You can also just source it from the internet if you always have internet:
```html
<script src="https://d3js.org/d3.v5.min.js"></script>
```

### Simple Web Server with node
For simple examples using the file system to open the file is enough, but for larger ones that load external files, it is better to use a web server.

```bash
$ npm install http-server -g
```

Once installed move to the working directory and execute:
```bash
$ http-server
```

> I had an issue running ```http-server``` out of the box and this [thread](https://github.com/nodejs/node-v0.x-archive/issues/3911) fixed it.

Ex: Create a sim link called *node* that points to *nodejs*
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop/D3_tutorials$ sudo ln -s /usr/bin/nodejs /usr/bin/node
bbearce@bbearce-XPS-15-9560:~/Desktop/D3_tutorials$ http-server
Starting up http-server, serving ./
Available on:
  http://127.0.0.1:8080
  http://192.168.0.46:8080
Hit CTRL-C to stop the server
``` 

Notice:
```bash
bbearce@bbearce-XPS-15-9560:~/Desktop/D3_tutorials$ ls -la /usr/bin/node
lrwxrwxrwx 1 root root 15 Feb 27 22:53 /usr/bin/node -> /usr/bin/nodejs
bbearce@bbearce-XPS-15-9560:~/Desktop/D3_tutorials$ ls -la /usr/bin/nodejs
-rwxr-xr-x 1 root root 11187096 Aug  9  2018 /usr/bin/nodejs
```

>Don't forget about codepen and jsfiddle as possible development environments

### Hello World

