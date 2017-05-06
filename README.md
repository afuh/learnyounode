# Learn You The Node.js For Much Win

https://github.com/workshopper/learnyounode


### Hello World (Exercise 1 of 13)  
Write a program that prints the text "HELLO WORLD" to the console (stdout).no
```shell
$ touch program.js
$ echo "console.log('HELLO WORLD');" > program.js
$ node program.js
```

### BABY STEPS (Exercise 2 of 13)  
Write a program that accepts one or more numbers as command-line arguments and prints the sum of those numbers to the console (stdout).

```javascript
const input = process.argv.splice(2);
const sum = JSON.parse(`[${input}]`).reduce((a, b) => a + b, 0);
console.log(sum);
```

### MY FIRST I/O! (Exercise 3 of 13)  
Write a program that uses a single **synchronous** filesystem operation to read a file and print the number of newlines (\n) it contains to the console (stdout), similar to running `cat file | wc -l`.

```javascript
const fs = require('fs');

const read = fs.readFileSync(process.argv[2]);
const countLines = read.toString().split('\n').length - 1;

console.log(countLines);
```

### MY FIRST ASYNC I/O! (Exercise 4 of 13)  
Write a program that uses a single **asynchronous** filesystem operation to read a file and print the number of newlines it contains to the console (stdout), similar to running `cat file | wc -l`.
```javascript
const fs = require('fs');

const read = fs.readFile(process.argv[2], (err, data) => {
  if (err) console.log(err);
  const countLines = data.toString().split('\n').length - 1;
  console.log(countLines);
});
```

### FILTERED LS (Exercise 5 of 13)
Create a program that prints a list of files in a given directory, filtered by the extension of the files. You will be provided a directory name as the first argument to your program (e.g. '/path/to/dir/') and a file extension to filter by as the second argument.

For example, if you get 'txt' as the second argument then you will need to filter the list to only files that **end with .txt**. Note that the second argument _will not_ come prefixed with a '.'.

```javascript
const fs = require('fs')
const path = require('path')

const pathName = process.argv[2];
const ext = process.argv[3];

fs.readdir(pathName, (err, list) => {
  if (err) throw err;
  list.map(file => {
    if(path.extname(file) === `.${ext}`) console.log(file);
  })
})
```
### MAKE IT MODULAR (Exercise 6 of 13)
 Create a program that prints a list of files in a given directory, filtered by the extension of the files. The first argument is the  
directory name and the second argument is the extension filter. Print the list of files (one file per line) to the console. You must use asynchronous I/O.  

**myModule.js**
```javascript
const fs = require('fs');
const path = require('path');

module.exports = function(dirName, extension, callback) {
  fs.readdir(dirName, (err, list) => {
    if (err) return callback(err);
    const data = list.filter(file =>
      path.extname(file) === `.${extension}`
    )
    callback(null, data)
  });
};
```
**program.js**
```javascript
const filter = require('./myModule')

const pathName = process.argv[2];
const ext = process.argv[3];

filter(pathName, ext, (err, list) => {
  if (err) throw err;
  list.map(file => console.log(file));
})
```

### HTTP CLIENT (Exercise 7 of 13)  
Write a program that performs an HTTP GET request to a URL provided to you as the first command-line argument. Write the String contents of each "data" event from the response to a new line on the console (stdout).

```javascript
const http = require('http')

const url = process.argv[2];

http.get(url, (response) => {
  response.setEncoding("utf8")
  response.on("data", data => console.log(data))
}).on('error', error => console.log(error))
```
### HTTP COLLECT (Exercise 8 of 13)  
Write a program that performs an HTTP GET request to a URL provided to you as the first command-line argument. Collect all data from the server (not just the first "data" event) and then write two lines to the console (stdout).

The first line you write should just be an integer representing the number of characters received from the server. The second line should contain the complete String of characters sent by the server.

```shell
$ npm i --save bl
```

```javascript
const http = require('http')
const buffer = require('bl')

const url = process.argv[2];

http.get(url, response => {
  response.pipe(buffer((err, data) => {
    if (err) throw err
    console.log(data.length);
    console.log(data.toString());
  }))
})
```
### JUGGLING ASYNC (Exercise 9 of 13)
This problem is the same as the previous problem (HTTP COLLECT) in that you need to use http.get(). However, this time you will be provided with three URLs as the first three command-line arguments.

You must collect the complete content provided to you by each of the URLs and print it to the console (stdout). You don't need to print out the length, just the data as a String; one line per URL. The catch is that you must print them out in the same order as the URLs are provided to you as command-line arguments.
```javascript
const http = require('http')
const bl = require('bl')

const info = []
let count = 0

for (let i = 0; i < 3; i++) {
  getResponse(i)
}

function getResponse(i) {
  http.get(process.argv[2 + i], (response) => getData(response, i))
}

function getData(res, i) {
  res.pipe(bl((err, data) => {

    if (err) return console.log(err)
    info[i] = data.toString()
    count++;

    count === 3 && logResults();
  }))
}

function logResults() {
  info.map(log => console.log(log))
}
```
### TIME SERVER (Exercise 10 of 13)  
Write a **TCP time server**!

Your server should listen to TCP connections on the port provided by the first argument to your program. For each connection you must write the current date & 24 hour time in the format:

```
"YYYY-MM-DD hh:mm"
```

followed by a **newline** character. Month, day, hour and minute must be *zero-filled* to 2 integers. For example:

```
"2013-07-06 17:42"
```

After sending the string, close the connection.
```javascript
const net = require('net')

const server = net.createServer((socket) => {
  console.log('client connected');
  socket.end(`${getDate()}\n`)
});

server.listen(process.argv[2], () => console.log("server bound"))


function getDate () {
  const date = new Date();

  const year = date.getFullYear();
  const month = date.getMonth() + 1;
  const day = date.getDay();

  const hour = date.getHours();
  const minutes = date.getMinutes();

  return `${year}-${addZero(month)}-${addZero(day)} ${addZero(hour)}:${addZero(minutes)}`;
}

const addZero = i => (i < 10 ? '0' : "") + i;
```
### HTTP FILE SERVER (Exercise 11 of 13)
Write an HTTP **server** that serves the same text file for each request it receives.

Your server should listen on the port provided by the first argument to your program.

You will be provided with the location of the file to serve as the second command-line argument. You **must** use the `fs.createReadStream()` method to stream the file contents to the response.
```javascript
const http = require('http')
const fs = require('fs')

const server = http.createServer((req, res) => {
  fs.createReadStream(process.argv[3]).pipe(res)
})

server.listen(Number(process.argv[2]))
```

### HTTP UPPERCASERER (Exercise 12 of 13)  
Write an HTTP **server** that receives only POST requests and converts incoming POST body characters to upper-case and returns it to the client.

Your server should listen on the port provided by the first argument to your program.
```shell
$ npm i --save through2-map
```
```javascript
const http = require('http')
const map = require("through2-map")

const server = http.createServer((req, res) => {
  if (req.method === 'POST') {
    req.pipe(map(chunk => chunk.toString().toUpperCase())).pipe(res);
  }
});

server.listen(Number(process.argv[2]))
```

### HTTP JSON API SERVER (Exercise 13 of 13)  
Write an HTTP **server** that serves JSON data when it receives a GET request to the path '/api/parsetime'. Expect the request to contain a query string with a key 'iso' and an ISO-format time as the value.

For example:

  /api/parsetime?iso=2013-08-10T12:10:15.474Z

The JSON response should contain only 'hour', 'minute' and 'second' properties. For example:

```json
{
  "hour": 14,
  "minute": 23,
  "second": 15
}
```

Add second endpoint for the path '/api/unixtime' which accepts the same query string but returns UNIX epoch time in milliseconds (the number of milliseconds since 1 Jan 1970 00:00:00 UTC) under the property 'unixtime'. For example:

```json
{ "unixtime": 1376136615474 }
```

Your server should listen on the port provided by the first argument to your program.
```javascript
const http = require('http')
const url = require("url")

function handleResponse(path, time) {
  const date = new Date(time)

  if (path.includes("parsetime")) {
    return {
      hour: date.getHours(),
      minute: date.getMinutes(),
      second: date.getSeconds()
    }
  }
  else if (path.includes("unixtime")) {
    return {
      unixtime: date.getTime()
    }
  }
}

const server = http.createServer((req, res) => {
  const parseurl = url.parse(req.url, true)
  const path = parseurl.pathname;
  const time = parseurl.query.iso;
  const json = handleResponse(path, time);

  res.writeHead(200, { 'Content-Type': 'application/json' })
  res.end(JSON.stringify(json));
});

server.listen(Number(process.argv[2]))
```
