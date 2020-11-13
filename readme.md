# Chat App 

## Live server https://task-3b-chat-app.herokuapp.com/

The objective of this website/project is to provide a platform to all who can just visit to this website 
and they can create their own chat room where they can talk to their friend anywhere from the world.

---

# Project Author

Check Me Out

[Saidul Mondal](https://github.com/saidul-mondal-au7)

## Requirements

For development, you will only need Node.js and a node global package, Yarn, installed in your environement.

---

### Node
- #### Node installation on Windows

  Just go on [official Node.js website](https://nodejs.org/) and download the installer.
  Also, be sure to have `git` available in your PATH, `npm` might need it (You can find git [here](https://git-scm.com/)).

- #### Node installation on Ubuntu

  You can install nodejs and npm easily with apt install, just run the following commands.

      $ sudo apt install nodejs
      $ sudo apt install npm

- #### Other Operating Systems
  You can find more information about the installation on the [official Node.js website](https://nodejs.org/) and the [official NPM website](https://npmjs.org/).

If the installation was successful, you should be able to run the following command.

```
    $ node -v
    v12.16.2

    $ npm -v
    6.14.4
```
---

## Project Installation
  After installing node, this project will need many NPM Packages, so just run the following command to install all.

    $ git clone Here(https://github.com/saidul-mondal-au7/Task-3B)
    $ cd Task-3B-main
    $ npm i

---



## Running The Project

```sh
    $ npm run dev
```

## NPM Packages Used -

- express -

- socket.io - 

- bad-words -

## Details of this App

### Setting up Socket.io
First up, install the module.
```sh
   $ npm i socket.io@3.0.1
```
- Socket.io can be used on its own or with Express. Since the chat application will be
serving up client-side assets, both Express and Socket.io will get set up. The server file
below shows how to get this done.

```js
  const path = require('path')

const http = require('http')

const express = require('express')

const socketio = require('socket.io')

// Create the Express application

const app = express()

// Create the HTTP server using the Express app

const server = http.createServer(app)

// Connect socket.io to the HTTP server

const io = socketio(server)

const port = process.env.PORT || 3000

const publicDirectoryPath = path.join(\_\_dirname, '../public')

app.use(express.static(publicDirectoryPath))

// Listen for new connections to Socket.io

io.on('connection', () => {

console.log('New WebSocket connection')

})

server.listen(port, () => {

console.log(`Server is up on port ${port}!`)


```

The server above uses io.onwhich is provided by Socket.io. onallows the server to listen

for an event and respond to it. In the example above, the server listens for connection

which allows it to run some code when a client connects to the WebSocket server.

Socket.io on the Client

Socket.io is also used on the client to connect to the server. Socket.io automatically serves

up /socket.io/socket.io.jswhich contains the client-side code. The script tags below

load in the client-side library followed by a custom JavaScript file.
```html
   <script src="/socket.io/socket.io.js"></script>
   <script src="/js/chat.js"></script>
```


Client-side JavaScript can then connect to the Socket.io server by calling io. iois

provided by the client-side Socket.io library. Calling this function will set up the connection,

and it’ll cause the server’s connectionevent handler to run.


io()

Documentation Links

•

[Socket.io](https://socket.io/)
