# MERN Stack developer

### Contents:

- [Node.Js](#nodejs)
- Express.Js
- MongoDB
- Mongoose
- JsonWebToken "JWT"
- Authentication
- Authorization
- Payments

---

## Node.Js

- Node.js is a server-side runtime environment for JavaScript
- Node.js is built on **a non-blocking I/O model**, which allows developers to create fast and resource-efficient applications

### Contents:

- [Modules](#modules)
  - [Built-In modules](#built-in-modules)
- [CommonJs vs ECMAScript Module](#commonjs-vs-ecmascript-module)
- [module.exports vs exports](#moduleexports-vs-exports)
- [Node Package Manager "npm"](#node-package-manager-npm)
  - [package.json](#packagejson)
  - [package-lock.json](#package-lockjson)

---

### Modules

- module is a self-contained unit of code that can be reused in different parts of a program

- It is a way to organize and encapsulate related code into a single unit that can be easily imported and used by other parts of the application.

- in Node.Js, The module system is based on the **CommonJS** specification, which defines a standard for creating reusable modules in JavaScript.

- In Node.Js, each file is trated as a separate module.

Type of Modules :

- **Local Module** :- modules that we create in our application
- **Built-In Modules** :- modules That Node.Js ships with out of the box
- **Third party modules** :- Modules written by other developers that we can use in our application

To load a module into another file, we use the **require** function.

```js
const module = require("moduleName");
```

### CommonJs vs ECMAScript Module

- By default, files with the **.js** extension will be treated as commonJs modules, with the **.mjs** extension files are treated as ES modules.

- ES modules can import commonJs modules, but commonJs modules cannot import ES modules.

- CommonJs imports are dynamically resolved at run time, with ES Modules, imports are static, which mean they are executed at parse time.

### module.exports vs exports

- both **module.exports** and **exports** are reference to the same (empty) object at the beginning. (But only module.exports will be returned)

- If any object is assigned directly to **module.exports**, then **exports** cannot be used to export any object from that module as it no longer refers to **module.exports**.

| module.exports                                                          | exports                                                                               |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| We can call **module. exports** only once in our module.                | We can call or use **exports** multiple times in our module.                          |
| It is the object reference that gets returned from the require() calls. | exports are not returned by require(). It is just a reference to the module. exports. |

```js
// Exporting a single value using module.exports
module.exports = function add(a, b) {
  return a + b;
};

// Not Working with above code
// Exporting multiple values using exports
exports.foo = "bar";
exports.baz = function () {
  console.log("Hello, world!");
};
```

```js
// Will Working
module.exports = {
  addTodo,
  removeTodo,
};

// NOT Working,
// Because the reference doesn't point anymore to the object where module.exports points
exports = {
  addTodo,
  removeTodo,
};
```

```js
// this will not work as expected. the second assignment will override the first one.
module.exports = addTodo;
// Now typeof returning result is 'function'
module.exports = removeTodo;
```

### Built-In Modules

- [FS module](#fs-module)
- [HTTP module](#http-module)
- [URL module](#url-module)
- [Events module](#events-module)

### FS module

- That used for file system operations such as reading and writing files, and so on.
- It provides a set of **asynchronous** and **synchronous** APIs for working with files and directories.
- The fs module can be used in both **server-side** and **client-side** applications

```js
const fs = require("fs");
```

Some of the commonly used functions in the fs module include:

- **fs.readFile()** - used to read the contents of a file asynchronously.
- **fs.readFileSync()** - used to read the contents of a file synchronously.

```js
fs.readFileSync("./txt/input.txt", "utf-8");

fs.readFile("./txt/start.txt", "utf-8", (err, data) => {
  console.log(data);
});
```

- **fs.writeFile()** - used to write data to a file asynchronously.
- **fs.writeFileSync()** - used to write data to a file synchronously.

```js
fs.writeFileSync("./txt/output.txt", "data");

fs.writeFile("./txt/read-this.txt", 'data"', "utf-8", (err) => {
  console.log("done");
});
```

- **fs.mkdir()** - used to create a new directory asynchronously.
- **fs.mkdirSync()** - used to create a new directory synchronously.
- **fs.readdir()** - used to read the contents of a directory asynchronously.
- **fs.readdirSync()** - used to read the contents of a directory synchronously.

### HTTP module

- That used for creating and interacting with HTTP (Hypertext Transfer Protocol) servers and clients

- It provides a set of functions and classes that allow you to build HTTP-based applications such as web servers, RESTful APIs, and more.

```js
const http = require("http");
```

- You can create an HTTP server using the createServer() function

- The **createServer()** function takes a **callback function** as an argument, which is called for every incoming request to the server

- The callback function takes two arguments: a **request** object and a **response** object.

```js
const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.write("Hello, world!");
  res.end();
});

server.listen(3000, () => {
  console.log("Server listening on port 3000");
});
```

### URL module

- It provides a way to parse and manipulate URLs (Uniform Resource Locators) and their components

- It provides a set of functions and classes that allow you to work with URLs in your Node.js application.

```js
const url = require("url");
```

Some of the commonly used functions in the url module include:

- **url.parse()** - used to parse a URL string into its components. \
  The resulting will contains the **protocol**, **host**, **path**, **query parameters**, and **fragment identifier** of the URL.

```js
url.parse("http://example.com/path/to/resource?query=string#fragment");
Url {
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'example.com',
  port: null,
  hostname: 'example.com',
  hash: '#fragment',
  search: '?query=string',
  query: 'query=string',
  pathname: '/path/to/resource',
  path: '/path/to/resource?query=string',
  href: 'http://example.com/path/to/resource?query=string#fragment'
}
```

- **url.format()** - used to format a URL object into a URL string.\

```js
const urlObject = {
  protocol: "http:",
  slashes: true,
  hostname: "example.com",
  pathname: "/path/to/resource",
  search: "?query=string",
  hash: "#fragment",
};

url.format(urlObject);
url: "http://example.com/path/to/resource?query=string#fragment";
```

- **url.resolve()** - used to resolve a relative URL against a base URL.

### Events Module

- It provides a way to handle and emit events in your application

- It is a powerful mechanism for building event-driven applications, where certain actions or behaviors are triggered by specific events.

```js
const events = require("events");

const eventEmitter = new events.EventEmitter();

eventEmitter.on("greet", (name) => {
  console.log(`Hello, ${name}!`);
});

eventEmitter.emit("greet", "John");
```

<!-- - The "\_\_v" field in MongoDB is automatically added by Mongoose,
- which is an Object Data Modeling (ODM) library for MongoDB and Node.js.
- This field is used to track the schema version of a document.

- When you create a new document in MongoDB using Mongoose, the "**v" field is automatically set to 0 to indicate that the document is at version 0. As you update the document, the value of the **v field will be incremented automatically by Mongoose. -->

## Node Package Manager "npm"

- used to manage dependencies and packages in Node.js applications. It allows you to easily install, update, and remove packages and their dependencies

- The NPM program is installed on your computer when you install Node.js

### package.json

- **package.json** is a configuration file used in Node.js-based projects to define project metadata, dependencies, and scripts

- It is used by the npm package manager to manage dependencies and scripts for the project.

Here are the key components of a package.json file:

- **name**: The name of the project.
- **version**: The version number of the project.
- **description**: A short description of the project.
- **keyword**: A collection of keywords that describe a module.
- **main**: The entry point file for the project.
- **dependencies**: A list of dependencies required by the project and their versions.
- **devDependencies**: A list of development dependencies required for developing and testing the project.
- **scripts**: A list of scripts that can be executed using the npm run command.
- **repository**: The URL of the project's Git repository.
- **author**: The name and contact information of the project's author.
- **license**: The license under which the project is distributed.

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My awesome project",
  "keywords": ["HTML","CSS"]
  "main": "index.js",
  "dependencies": {
    "express": "^4.17.1",
    "lodash": "^4.17.20"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  },
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/username/my-project.git"
  },
  "author": "John Doe <johndoe@example.com>",
  "license": "MIT"
}
```

### package-lock.json

- A file that is automatically generated by npm when installing packages in a Node.js project

- It is used to lock the versions of packages installed in the project, including their dependencies,
- to ensure that the same versions are used across different environments and installations.

## libuv

- libuv is a multi-platform support library that is used by Node.js to provide **asynchronous I/O operations**, **timers**, **processes**, and more

- It is a core part of the Node.js runtime, and provides a unified API for working with I/O on different operating systems.

- It allows Node.js to handle a large number of concurrent connections and perform I/O operations efficiently

Some of the key features of libuv include:

- **Asynchronous** I/O: libuv provides a set of functions for performing I/O operations asynchronously, without blocking the main thread of execution.

- **Timers**: libuv provides a timer API that allows you to schedule functions to be called at a later time.

- **Processes**: libuv provides a process API that allows you to spawn child processes, communicate with them, and monitor their status.

- **Threads**: libuv provides a thread pool API that allows you to offload CPU-intensive tasks to worker threads, freeing up the main thread for other operations.

- **Networking**: libuv provides a networking API that allows you to create and manage network sockets.
