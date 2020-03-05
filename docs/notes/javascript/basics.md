# Basics

All JavaScript examples use ES2015 syntax, which include:

* ```const``` and ```let``` instead of ```var```
* Arrow functions (```d => d``` instead of ```function(d) { return d; }```) where appropriate
* Spread operators ```[... iterable]```, chained expressions, maps, sets and promises
* Template strings literals, defined using backticks
* Iterable collections, such as maps and sets

## Arrays

```js
const colors = ["red", "blue", "green"];
const geocoords = [27.2345, 34.9937]; 
const numbers = [1,2,3,4,5,6]; 
const empty = [];
```

Each array has a ```length``` property that returns the number of elements. It's very useful to iterate using the array index:

```js
for(let i = 0; i < colors.length; i++) {
    console.log(colors[i]); 
}
```

You can also loop over the elements of an array using the of operator (introduced in ES2015), when you don't need the index:

```js
for(let color of colors) {
    console.log(color); 
}
```

And you can use the ```forEach()``` method, which runs a function for each element and also allows access to the **index**, **item** and **array** inside the function:

```js
colors.forEach(function(i, color, colors) {
    console.log((i+1) + ": " + color);
})
```
> You need to pass something into the funtion and most by default use index, item, array, but as you can see other more descriptive placeholders can be used.

Multi-dimensional arrays are created in JavaScript as arrays of arrays:

```js
const points = [[200,300], [150,100], [100,300]];
```

You can retrieve individual items like this:

```js
const firstPoint = points[0];
const middleX = points[1][0];
```

JavaScript provides many ways to extract and insert data into an array. It's usually recommended to use methods whenever possible. The following table lists useful methods you can use on arrays. Some modify the array; others return new arrays and other types. The examples provided use the ```colors``` and ```numbers``` arrays as declared above. Try them out using your browser's JavaScript console:

| Method            | Description                                                                                       | Example                                                                                          |
|-------------------|---------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| push(item)        | Modifies the array adding an item to the end.                                                     | ```colors.push("yellow"); // ["red", "blue", "green", "yellow"];                             ``` |
| pop()             | Modifies the array, removing and returning the last item.                                         | ```const green = colors.pop(); // ["red", "blue"];                                           ``` |
| unshift(item)     | Modifies the array inserting an item at the beginning.                                            | ```colors.unshift("yellow"); // ["yellow", "red", "blue", "green"];                          ``` |
| shift()           | Modifies the array, removing and returning the first item.                                        | ```const red = colors.shift(); // ["blue", "green"];                                         ``` |
| splice(p, n, i)   | Modifies the array, starting at position p. Can be used to delete items, insert or replace.       | ```const s = numbers.splice(2,3);  // s = [3,4,5]  // numbers = [1,2,6]                      ``` |
| reverse()         | Modifies the array, reversing its order.                                                          | ```numbers.reverse(); // [6,5,4,3,2,1]                                                       ``` |
| sort()            | Modifies the array sorting by string order (if no args) or by a comparator function.              | ```numbers.sort((a,b) => b – a); // numbers = [6,5,4,3,2,1]                                  ``` |
| slice(b,e)        | Returns a shallow copy of the array between b and e.                                              | ```const mid = numbers.slice(2,4)  // mid = [3,4]                                            ``` |
| filter(function)  | Returns  new array where the elements pass the test implemented by the function.                  | ```const even = numbers.filter(n => n%2==0); // [2,4,6]                                      ``` |
| find(function)    | Returns the first element that satisfies the test function                                        | ```const two = numbers.find(n => n%2==0); // 2                                               ``` |
| indexOf(item)     | Returns the index of the first occurrence of item in the array.                                   | ```const n = numbers.indexOf(3); // 4                                                        ``` |
| includes(item)    | Returns true if an array contains item among its entries.                                         | ```const n = numbers.includes(3); // true                                                    ``` |
| lastIndexOf(item) | Returns the index of the last occurrence of the item in the array.                                | ```const n = colors.lastIndexOf("blue"); // 1                                                ``` |
| concat(other)     | Returns a new array that merges the current array with another.                                   | ```const eight = numbers.concat([7,8]); // [1,2,3,4,5,6,7,8]                                 ``` |
| join()join(delim) | Returns a comma-separated string of the elements in the array (an optional delimiter may be used) | ```const csv = numbers.join(); // "1,2,3,4,5,6"Copyconst conc = numbers.join(""); // "123456"``` |
| map(function)     | Returns new array with each element modified by function.                                         | ```const squares = numbers.map(n => n*n); // [1,4,9,16,25,36]                                ``` |
| reduce(function)  | Returns the result of an accumulation operation using the values in the array.                    | ```const sum =    numbers.reduce((a, n) => a + n);                                           ``` |
| forEach(function) | Executes the provided function once for each element in the array.                                | ```const squares = [];  numbers.forEach(n => squares.push(n*n) // squares = [1,4,9,16,26,36] ``` |


## Strings

Strings are primitive types in JavaScript that can be created with single quotes or double quotes. There is no difference. It's only a matter of style. ES2015 introduced two new string features: **template literals** and **multiline strings**.

Multiline strings can be created adding a backslash at the end of each line:

```js
const line = "Multiline strings can be \
reated adding a backslash \
at the end of each line";
```

Template literals are strings created with **backticks**. They allow the inclusion of JavaScript expressions inside ```${}``` placeholders. The result is concatenated as a single string.

```js
const template = `The square root of 2 is ${Math.sqrt(2)}`;
```

If you need to use a special character in a string, such as a double quote in a double quoted string, or a backslash, you need to precede it with a backslash:

```js
const s = "This is a backslash \\ and this is a double quote \"";
```

There are several methods for string manipulation. They all return new strings or other types. No methods modify the original strings.

| Method                      | Description                                                                                                                                    | Example                                                                                                                                     |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| startsWith(s)               | Returns true if string starts with the string passed as a parameter                                                                            | ```const s = "This is a test string" const r = s.startsWith("This"); // true                                                             ```|
| endsWith(s)                 | Returns true if string ends with the string passed as a parameter                                                                              | ```const s = "This is a test string"  const r = s.endsWith("This"); // false                                                             ```|
| substring(s,e)              | Returns a substring between start (incl.) and end indexes (not incl.)                                                                          | ```const k = "Aardvark"  const ardva = k.substring(1,6);                                                                                 ```|
| split(regx)split(delim)     | Splits a string by a delimiter character or regular expression and returns an array                                                            | ```const result = s.split(" ");  // result =  //   ["This","is","a","test","string"]                                                     ```|
| indexOf(s)                  | Returns the index of the first occurrence of a substring                                                                                       | ```const k = "Aardvark"  const i = k.indexOf("ar"); // i = 1                                                                             ```|
| lastIndexOf(s)              | Returns the index of the last occurrence of a substring                                                                                        | ```const k = "Aardvark"  const i = k.lastIndexOf("ar"); // i = 5                                                                         ```|
| charAt(i)                   | Returns char at index i. Also supported as ‘string’[i]                                                                                         | ```const k = "Aardvark"  const v = k.charAt(4);                                                                                          ```|
| trim()                      | Removes whitespace from both ends of a string.                                                                                                 | ```const text = "   data   "  const r = data.trim(); // r = "data"                                                                       ```|
| match(regx)                 | Returns an array as the result of matchin a regular expression against the string.                                                             | ```const k = "Aardvark"  const v = k.match(/[a-f]/g);  // v = ["a", "d", "a"]                                                            ```|
| replace(regx,r)replace(s,t) | Returns new string replacing match of regexp applied to the string with replacement, or all occurrences of source string with a target string. | ```const k = "Aardvark"  const a = k.replace(/a/gi, 'u')``` ```// a = "uurdvurk"  const b = k.replace('ardv', 'ntib')  // b = "Antibark" ```|


## Functions

Functions are typically created in JavaScript using the ```function``` keyword, using one of the forms below:

```js
function f() {
     console.log('function1', this);
 }
const g = function(name) {
     console.log('function ' + name, this);
 }
f(); // calls f
g('test'); // calls g() with a parameter
```

>The ```this``` keyword refers to the object that owns the function. If this code runs in a browser, and this is a top-level function created in the ```<script>``` block, the owner is the global ```window``` object. Any properties accessed via this refer to that object.

```js
const obj = {a: 5, b: 6}
 obj.method = function() {
     console.log('method', this)
 }
obj.method()
```
output:
```method {a: 5, b: 6, method: ƒ}```

Arrow functions were introduced in ES2015. They are much more compact and can lead to cleaner code, but the scope of ```this``` is no longer retained by the object. In the code below, it refers to the global ```window``` object. Code that uses ```this.a``` and ```this.b``` will not find any data in the object and will return **undefined**.

```js
obj.arrow = () => console.log('arrow', this)
obj.arrow()
```

output:
```arrow Window {parent: Window, opener: null, top: Window, length: 0, frames: Window, …}```

## Objects

An object is an unordered collection of data. Values in an object are stored as key-value pairs. You can create an object by declaring a comma-separated list of ```key:value``` pairs within curly braces, or simply a pair of opening-closing curly braces if you want to start with an empty object:

```js
const color = {name: "red", code: "ff0000"};
const empty = {};
```

Objects can contain other objects and arrays, which can also contain objects. They can also contain functions (which have access to local properties and behave as methods):

```js
const city = {name: "Sao Paulo", 
              location: {latitude: 23.555, longitude: 46.63},
              airports: ["SAO","CGH","GRU","VCP"]};
const circle = {
    x: 200, 
    y: 100, 
    r: 50, 
    area: function() { 
        return this.r * this.r * 3.14; 
    }
}
```

A typical dataset used by a simple chart usually consists of an array of objects.

```js
var array2 = [
    {continent: "Asia", areakm2: 43820000}, 
    {continent: "Europe", areakm2: 10180000},
    {continent: "Africa", areakm2: 30370000},
    {continent: "South America", areakm2: 17840000},
    {continent: "Oceania", areakm2: 9008500},
    {continent: "North America", areakm2:24490000}
];
```

You can access the properties of an object using the dot operator or brackets containing the key as a string. You can run its methods using the dot operator:

```js
const latitude = city.location.latitude; 
const oceania = array2[4].continent;
const code = color["code"];
circle.r = 100; 
const area = circle.area();
```

You can also loop over the properties of an object:

```js
for(let key in color) {
    console.log(key + ": " + color[key]); 
}
```

Properties and functions can be added to objects. It's common to write code that declares an empty object in a global context so that operations in other contexts add data to it:

```js
coords = {lat:34.342352435, lng: 108.98375984}

const map = {};
function getCoords(coords) {
    map.latitude = coords.lat;
    map.longitude = coords.lng;
}

console.log(map)
```

output:
```js
{lat:34.342352435, lng: 108.98375984}
```

Objects can also be created with a constructor. You can create an object that contains the current date/time using:

```js
const now = new Date();
```

JSON is a data format based on JavaScript objects. It has the same structure as JavaScript object but the property keys have to be placed within double quotes:

```html
{"name": "Sao Paulo", 
              "location": {"latitude": 23.555, "longitude": 46.63},
              "airports": ["SAO","CGH","GRU","VCP"]};
```

To use a JSON string in JavaScript you have to parse it.

## Maps and Sets

Besides arrays, ES2015 also introduced two new data structures: ```Map```, an associative array with key-value pairs easier to use than simple objects, and ```Set```, which doesn't allow repeated values. Both can be transformed to and from arrays.

You can create a new ```Set``` object using:

```js
const set = new Set();
```

And add elements to it using:

```js
set.add(5);
set.add(7);
```

If you try to add elements that already exist, they won't be included in the set:

```js
set.add(1);
set.add(5);   // not added
set.add(7);   // not added
console.log(set.size); // prints 3
```

You can check if a Set contains an element:

```js
console.log(set.has(3)); // false
```

And convert a Set object into an array:

```js
const array1 = [... set]; // spread operator
```

A Map can be more efficient than using an object to store key-value pairs in an associative array:

```js
const map = new Map();
map.set("a", 123)
map.set("b", 456);

console.log(map)
```
output:
```js
Map(2) {"a" => 123, "b" => 456}
```


You can then retrieve the value for each key using get(key):

```js
console.log(map.get("b"))
```

Or using iterative operations and the keys(), values() and entries() methods:

```js
for (let key of map.keys()) {
    console.log(key, map.get(key))
}

for (let [key, value] of map.entries()) {
    console.log(key, value)
}

map.forEach ((k, v) => console.log(k, v))
```

Maps can be converted into arrays with the spread operator or Arrays.from():

```js
const array2 = [... map.values()];  // an array of values
const array3 = Array.from(map.values()); // an array of keys  
```

