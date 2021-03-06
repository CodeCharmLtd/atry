## atry - async try

Asynchronous try-catch based on Node.JS domain module.

 Note: Use wisely, asynchronous exceptions may cause resource leakage [as discussed here](http://stackoverflow.com/questions/15825752/why-would-an-exception-cause-resource-leaks-in-node-js "Stack Overflow: Why would an exception cause resource leaks in Node.JS?")

# API

* `atry(runningFunction, [arg1, [arg2, ...]])`
 * `.catch(errorHandler)` - Catches both synchronous and asynchronous exceptions

 ```js
  atry(function() {
    setTimeout(function() {
      throw new Error("Got error");
    }, 10);
  }).catch(function(err) {
    console.log("Got error", err);
  });
 ```

 * `.ignoreCatch(errorHandler)` - Ignores both synchronous and asynchronous exceptions

 ```js
  atry(function() {
    setTimeout(function() {
      throw new Error("Got error");
    }, 10);
  }).ignoreCatch(); // ignore exceptions
 ```

* `atry.bind(bindFunction).catch(catchFunction)` - Returns function that will be exception safe
 ```js
    fs.readFile('someFile', atry.intercept(function(err, data) {
      if(err) throw err; // this will be caught below
      console.log("Got data");
    }).catch(function(err) {
      console.log("got error during file reading", err);
    }));
  ```

* `atry.intercept(bindFunction).catch(catchFunction)` - Returns function that will be exception safe and its first argument will be handled as if it is `err`. 
 ```js
    fs.readFile('someFile', atry.intercept(function(data) {
      console.log("Got data");
    }).catch(function(err) {
      console.log("got error during file reading", err);
    }));
 ```

## Author
Damian Kaczmarek <rush@rushbase.net>

## Inspired by
http://stackoverflow.com/questions/14301839/javascript-asynchronous-exception-handling-with-node-js
