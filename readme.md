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
Socket.io can be used on its own or with Express. Since the chat application will be
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

- [Socket.io](https://socket.io/)


### Working with Events

There are two sides to every event, the sender and the receiver. If the server is the
sender, then the client is the receiver. If the client is the sender, then the server is the
receiver.

Events can be sent from the sender using emit. Events can be received by the receiver
using on. The example below shows how this pattern can be used to create a simple
counter application. The following snippet contains the client-side JavaScript code.
```js
  const socket = io()
  // Listen for "countUpdated"
  socket.on('countUpdated', (count) => {
  console.log('The count has been updated!', count)
  })
  document.querySelector('#increment').addEventListener('click', () => {
  // Emit "increment
  socket.emit('increment')
})
```

The client-side code uses on to listen for the countUpdated event. A message will be
logged with the current count when that event is received. The client-side code also uses
emit to send the increment event. This occurs when a button on the screen is clicked.
The server-side code for this example is below.

```js
let count = 0
io.on('connection', (socket) => {
 console.log('New WebSocket connection')
 socket.emit('countUpdated', count)
 socket.on('increment', () => {
 count++
 io.emit('countUpdated', count)
 })
})
```
The server above is responsible for emitting countUpdated and listening for increment.
New users get the current count right after they connect to the server. If a client sends
increment to the server, the count is incremented and all connected clients are notified of
the change.

On the client, socket.emit emits an event to the server. On the server, both socket.emit
and io.emit can be used. socket.emit sends an event to that specific client, while
io.emit sends an event to all connected clients.

### Broadcasting Events

Events can be broadcasted from the server using socket.broadcast.emit. This event will
get sent to all sockets except the one that broadcasted the event. The code below shows
this off. When a new user joins the chat application, socket.broadcast.emit is used to
send a message to all other users that someone new has joined.

```js
io.on('connection', (socket) => {
 socket.broadcast.emit('message', 'A new user has joined!')
})
```

### Event Acknowledgements

To explore acknowledgements, let’s set up the server to screen messages for profane
language. The bad-words module will let you check text for profane language.

```sh
   npm i bad-words@3.0.3
```
You already know there are two sides to every event, the sender and the receiver. In the
example below, the client is the one emitting the sendMessage event. The big change is
the addition of the callback function. This function will run if/when the server
acknowledges the event.

```js
  socket.emit('sendMessage', message, (error) => {
  if (error) {
  return console.log(error)
  }
  console.log('Message delivered!')
  })
```

On the server, the event listener for sendMessage also has a small change. Aside from the
message parameter, it now has access to the callback parameter. This callback function
can be called on the server to trigger the acknowledgement function on the client.

The callback can be called with or without data. In this example, the callback is called with
an error if profane language was detected. The argument would get passed to the client 

where the error could be shown. The callback is called without no arguments if no profane
language was detected. This lets the client know that the message was successfully
processed.

```js
  socket.on('sendMessage', (message, callback) => {
  const filter = new Filter()
  if (filter.isProfane(message)) {
  return callback('Profanity is not allowed!')
  }
  io.emit('message', message)
  callback()
  })

```

### Form and Button States

First up is the form that allows users to send a new message. This form should be disabled
while messages are being sent to the server. This will prevent duplicate messages from
being sent if the user was to double-click the button. The form can be disabled by setting
the disabled attribute on the submit button.

```js
  const $messageFormButton = $messageForm.querySelector('button')
  // Disable button
  $messageFormButton.setAttribute('disabled', 'disabled')
  // Enable buttons
  $messageFormButton.removeAttribute('disabled')
```

The interaction with the text input can also be improved. The text input should be cleared
and focused on when the form is submitted.

```js
  const $messageFormInput = $messageForm.querySelector('input')
  // Clear the text from the input
  $messageFormInput.value = ''
  // Shift focus back to the input
  $messageFormInput.focus()
```

### Form and Button States

First up is the form that allows users to send a new message. This form should be disabled
while messages are being sent to the server. This will prevent duplicate messages from
being sent if the user was to double-click the button. The form can be disabled by setting
the disabled attribute on the submit button.

```js
  const $messageFormButton = $messageForm.querySelector('button')
  // Disable button
  $messageFormButton.setAttribute('disabled', 'disabled')
  // Enable buttons
  $messageFormButton.removeAttribute('disabled')
  The interaction with the text input can also be improved. The text input should be cleared
  and focused on when the form is submitted.
  Version 1.0 116
  const $messageFormInput = $messageForm.querySelector('input')
  // Clear the text from the input
  $messageFormInput.value = ''
  // Shift focus back to the input
  $messageFormInput.focus()
```
### Creating a Template

First up, include these in your HTML. Mustache will be used to render the messages.
Moment and Qs will be used a bit later in the section.

``html
  <script
  src="https://cdnjs.cloudflare.com/ajax/libs/mustache.js/3.0.1/mustache.min.js
  "></script>
  <script
  src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.22.2/moment.min.js"><
  /script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qs/6.6.0/qs.min.js"></script>
```

Rendering messages to the screen will require two changes to the HTML. First up, a place
needs to be created on the page to store the rendered messages.

```html
    <div id="messages"></div>
```

Second, a template needs to be created for the messages. The template below looks like
pretty standard HTML. The only addition is {{message}}. This is the syntax used to inject a
value into the template. In this case, the message text will be shown inside the templates
paragraph.

```html
  <script id="message-template" type="text/html">
  <div>
  <p>{{message}}</p>
  </div>
  </script>
```

### Rendering a Template

The template can be compiled and rendered using client-side JavaScript. The snippet
below renders a new instance of the message template to the screen whenever it
receives a new message event.

```js
  // Select the element in which you want to render the template
  const $messages = document.querySelector('#messages')
  // Select the template
  const messageTemplate = document.querySelector('#message-template').innerHTML
  socket.on('message', (message) => {
  // Render the template with the message data
  const html = Mustache.render(messageTemplate, {
  message
  })
  // Insert the template into the DOM
  $messages.insertAdjacentHTML('beforeend', html)
  })
```

Documentation Links

- [Mustache.js](https://github.com/janl/mustache.js/)


### Working with Time

Every message is going to contain a timestamp. This timestamp will represent the time
when the server sent the message out to everyone in the chat application. The server will
be the one to generate the timestamp, which prevents the client from lying about when a
message was sent.

A JavaScript timestamp is nothing more than an integer. This integer represents the
number of milliseconds since the Unix Epoch. The Unix Epoch was January 1st, 1970 at
midnight, so the timestamp for the current point in time is a pretty big number.

```js
  // The getTime method is used to get the timestamp
  const timestamp = new Date().getTime()
  console.log(timestamp) // Will print: 1549481884646
```

Once the server sends the message and timestamp to the client, the client can format the
timestamp before rendering it. The Moment library provides an easy way to format
timestamps to fit your needs. For the chat app, showing something like “11:48 am” works
well.

```js
moment(message.createdAt).format('h:mm a')
```

You can find a complete list of the formatting options in the documentation below.

Documentation Links

- [Moment](https://momentjs.com/)

- [Moment](https://momentjs.com/)[ ](https://momentjs.com/)[formatting](https://momentjs.com/docs/#/displaying/format/)

### Adding a Join Page

Below is the HTML for the join page. This will be index.html. The HTML for the chat page
will move to chat.html. This will make sure that users visit the join page when pulling up
the site.

```html
  <!DOCTYPE html>
  <html>
  <head>
  <title>Chat App</title>
  <link rel="icon" href="/img/favicon.png">
  <link rel="stylesheet" href="/css/styles.min.css">
  </head>
  <body>
  <div class="centered-form">
  <div class="centered-form__box">
  <h1>Join</h1>
  <form action="/chat.html">
  <label>Display name</label>
  <input type="text" name="username" placeholder="Display
  name" required>
  <label>Room</label>
  <input type="text" name="room" placeholder="Room"
  required>
  <button>Join</button>
  </form>
  </div>
  </div>
  </body>
  </html>
```

### Socket.io Rooms

It’s the server’s job to add and remove users from a room. The server can add a user to a
room by calling socket.join with the room name. Below, the listener for join accepts the
username and the room name from the client. socket.join(room) is then called to add
the user to the room they wanted to join.

```js
  socket.on('join', ({ username, room }) => {
  // Join the room
  socket.join(room)
  // Welcome the user to the room
  socket.emit('message', generateMessage('Welcome!'))
  // Broadcast an event to everyone in the room
  socket.broadcast.to(room).emit('message', generateMessage(`${username}
  has joined!`))
})
```
The join listener above also calls to as part of socket.broadcast.to.emit. The to
method allows an event to be emitted to a specific room. In this case, when a user joins a
room, only users in that room will be notified.

The to method can also be used with io to send an event to everyone in a room.

```js
  // Emit a message to everyone in a specific room
  io.to('Center City').emit('message', generateMessage(message))
```

### Add and Removing Users

When a user attempts to join a room, their username and room name will be passed to
addUser. This will allow the application to track the user, and it’ll also allow the data to be
validated. If an error occurs, that error message will be sent back to the client and the user
won’t join the requests room.

```js
  socket.on('join', (options, callback) => {
  // Validate/track user
  const { error, user } = addUser({ id: socket.id, ...options })
  // If error, send message back to client
  if (error) {
  return callback(error)
  }
  // Else, join the room
  socket.join(user.room)
  socket.emit('message', generateMessage('Welcome!'))
  socket.broadcast.to(user.room).emit('message',
  generateMessage(`${user.username} has joined!`))
  callback()
  })
```

When a user disconnects from the application, removeUser is called to remove them from
the list of active users. If a user was removed, a message is sent to everyone in the chat
room letting them know that someone has left.

```js
  socket.on('disconnect', () => {
  const user = removeUser(socket.id)
  if (user) {
  io.to(user.room).emit('message', generateMessage(`${user.username}
  has left!`))
  }
  })
```

The server uses an acknowledgement to send errors back to the client. The client can set
up the callback function and respond to any errors that might occur. If an error does occur,
the snippet below shows the error message and then redirects the user back to the join
page.

```js
  socket.emit('join', { username, room }, (error) => {
  if (error) {
  alert(error)
  location.href = '/'
  }
  })
```

### Sending Messages to Room

The sendLocation event listener below uses getUser to get the username and room for
the user who sent the message. This allows the server to use to to emit the message to
only users in that chat room.

```js
  socket.on('sendLocation', (coords, callback) => {
  // Get the username and room for the user
  const user = getUser(socket.id)
  // Emit the message to just that room
  io.to(user.room).emit('locationMessage',
  generateLocationMessage(user.username,
  `https://google.com/maps?q=${coords.latitude},${coords.longitude}`))
  // Send an acknowledgement to the client
  callback()
  })
```

### Rendering the User List

The client can only render data it has access to, so the server needs to send a list of all
active users to the client. This data should be sent again whenever a user is added or
removed from a chat room.

getUsersInRoom is used to get a list of all users in a specific room. The server then emits
roomData to all clients in that affected room letting them know to rerender their user list.

```js
  // After a user joins or leaves
  io.to(user.room).emit('roomData', {
  room: user.room,
  users: getUsersInRoom(user.room)
  })
```

On the client-side, the sidebar template will be responsible for rendering the room name
and the list of users. Rendering a list with Moustache requires a new syntax. In the
template below, everything between {{#users}} and {{/users}} will be repeated for
each user. In this case, the username is rendered in a list item that will show up in the
sidebar.

```html
  <script id="sidebar-template" type="text/html">
  <h2 class="room-title">{{room}}</h2>
  <h3 class="list-title">Users</h3>
  <ul class="users">
  {{#users}}
  <li>{{username}}</li>
  {{/users}}
  </ul>
  </script>
```
The client-side is then able to listen for that event and render the user list to the sidebar.

```js
  const sidebarTemplate = document.querySelector('#sidebar-template').innerHTML
  socket.on('roomData', ({ room, users }) => {
  const html = Mustache.render(sidebarTemplate, {
  room,
  users
  })
  document.querySelector('#sidebar').innerHTML = html
  })
```

### Automatic Scrolling

```js
  const autoscroll = () => {
  // New message element
  const $newMessage = $messages.lastElementChild
  // Height of the new message
  const newMessageStyles = getComputedStyle($newMessage)
  const newMessageMargin = parseInt(newMessageStyles.marginBottom)
  const newMessageHeight = $newMessage.offsetHeight + newMessageMargin
  // Visible height
  const visibleHeight = $messages.offsetHeight
  // Height of messages container
  const containerHeight = $messages.scrollHeight
  // How far have I scrolled?
  const scrollOffset = $messages.scrollTop + visibleHeight
  if (containerHeight - newMessageHeight <= scrollOffset) {
  $messages.scrollTop = $messages.scrollHeight
  }
  }
```