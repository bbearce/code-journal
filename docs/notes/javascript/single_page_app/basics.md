# Vue and Flask Single Page App

>Source [testdriven](https://testdriven.io/blog/developing-a-single-page-app-with-flask-and-vuejs/)

This is an amazing tutorial. Worked first time. No bugs.

Also learned about an amazing python package for light json databases. There is an equivalent nodejs one too.

## python - jsondb
[tinydb](https://tinydb.readthedocs.io/en/latest/getting-started.html)

### Installing TinyDB

To install TinyDB from PyPI, run:
```bash
$ pip install tinydb
```

You can also grab the latest development version from [GitHub](http://github.com/msiemens/tinydb/). After downloading and unpacking it, you can install it using:
```
$ python setup.py install
```

#### Basic Usage

Let’s cover the basics before going more into detail. We’ll start by setting up a TinyDB database:
```python
>>> from tinydb import TinyDB, Query
>>> db = TinyDB('db.json')
```
You now have a TinyDB database that stores its data in db.json. What about inserting some data? TinyDB expects the data to be Python dicts:
```python
>>> db.insert({'type': 'apple', 'count': 7})
>>> db.insert({'type': 'peach', 'count': 3})
```

>Note
The insert method returns the inserted document’s ID. Read more about it here: [Using Document IDs](https://tinydb.readthedocs.io/en/latest/usage.html#document-ids).

Now you can get all documents stored in the database by running:
```python
>>> db.all()
[{'count': 7, 'type': 'apple'}, {'count': 3, 'type': 'peach'}]
```

You can also iter over stored documents:
```python
>>> for item in db:
>>>     print(item)
{'count': 7, 'type': 'apple'}
{'count': 3, 'type': 'peach'}
```

Of course you’ll also want to search for specific documents. Let’s try:
```python
>>> Fruit = Query()
>>> db.search(Fruit.type == 'peach')
[{'count': 3, 'type': 'peach'}]
>>> db.search(Fruit.count > 5)
[{'count': 7, 'type': 'apple'}]
```

Next we’ll update the count field of the apples:
```python
>>> db.update({'count': 10}, Fruit.type == 'apple')
>>> db.all()
[{'count': 10, 'type': 'apple'}, {'count': 3, 'type': 'peach'}]
```

In the same manner you can also remove documents:
```python
>>> db.remove(Fruit.count < 5)
>>> db.all()
[{'count': 10, 'type': 'apple'}]
```
And of course you can throw away all data to start with an empty database:
```python
>>> db.purge()
```
#### Advanced Usage

>Source [readthedocs - advanced tinydb](https://tinydb.readthedocs.io/en/latest/usage.html)


## nodejs - jsondb
[simple-json-db](https://www.npmjs.com/package/simple-json-db)

A simple, no-frills, JSON storage engine for Node.JS with full test coverage.

### Installation (bash)
```bash
npm install --save simple-json-db
```

### Usage (javascript)
**Instantiation**
```js
const JSONdb = require('simple-json-db');
const db = new JSONdb('/path/to/your/database.json');
```

The prototype of the constructor is ```new JSONdb(string, [object])```, and you can supply the optional options object by giving it as second parameter:
```js
const db = new JSONdb('/path/to/your/database.json', { ... });
```
See the [Options](https://www.npmjs.com/package/simple-json-db#options) section for more details.

**Options**

| **Key**         | **Value**   | **Description**                                                    | **Default Value**                 |
|-----------------|-------------|--------------------------------------------------------------------|-----------------------------------|
| asyncWrite      | Boolean     | Enables the storage to be asynchronously written to disk.          | false (synchronous behaviour)     |
| syncOnWrite     | Boolean     | Makes the storage be written to disk after every modification.     | true                              |

**Set a key**
```js
db.set('key', 'value');
```

The key parameter must be a string, value can be whatever kind of object can be stored in JSON format. ```JSON.stringify()``` is your friend!

**Get a key**
```js
db.get('key');
```

The key parameter must be a string. If the key exists its value is returned, if it doesn't the function returns ```undefined```.

**Check a key**
```js
db.has('key');
```

The key parameter must be a string. If the key exhists ```true``` is returned, if it doesn't the function returns ```false```.

**Delete a key**
```js
db.delete('key');
```

The key parameter must be a string. The function returns [as per the delete operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/delete#Return_value) if the key exhists, else it returns ```undefined```.

**Sync to disk**
```js
db.sync();
```

This function writes the JSON storage object to the file path specified as the parameter of the main constructor. Consult the [Options](https://www.npmjs.com/package/simple-json-db#options) section for usage details; on default options there is no need to manually invoke it.

**Access JSON storage**
```js
db.JSON();
```

This will return a copy of the internal ```JSON``` storage object, for you to tinker with and loop over.

**Replace JSON storage**
```js
db.JSON({ data });
```

Giving a parameter to the ```JSON``` function makes the object passed replace the internal one. *Be careful, as there's no way to recover the old object if the changes have already been written to disk*.