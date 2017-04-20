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
const spl = process.argv.splice(2);
const sum = JSON.parse(`[${spl}]`).reduce((a, b) => a + b);
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
  if (err) {
    return console.log(err);
  } else {
    const countLines = data.toString().split('\n').length - 1;
    console.log(countLines);
  }
});
```

### FILTERED LS (Exercise 5 of 13)
Create a program that prints a list of files in a given directory, filtered by the extension of the files. You will be provided a directory name as the first argument to your program (e.g. '/path/to/dir/') and a file extension to filter by as the second argument.

For example, if you get 'txt' as the second argument then you will need to filter the list to only files that **end with .txt**. Note that the second argument _will not_ come prefixed with a '.'.

```javascript
const fs = require('fs');
const path = require('path');

const pathname = process.argv[2];
const extension = `.${process.argv[3]}`;

const read = fs.readdir(pathname, (err, list) => {
  if (err) {
    return console.log(err);
  } else {
    list.map(i => {
      if (path.extname(i) === extension) {
        console.log(i);
      }
    });
  }
});
```
### MAKE IT MODULAR (Exercise 6 of 13)
