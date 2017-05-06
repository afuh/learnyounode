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

myModule.js
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
program.js
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
