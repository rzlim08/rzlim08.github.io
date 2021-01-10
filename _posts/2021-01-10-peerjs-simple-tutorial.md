---
layout: post
title: "PeerJS tutorial: A simple chat app"
categories: Miscellaneous
---
First things first, I want to give proper credit, a lot of the code in this tutorial was derived from "https://medium.com/samsung-internet-dev/building-an-internet-connected-phone-with-peerjs-775bd6ffebec". I found it still a bit complex as a first introduction to peerjs, so I made this tutorial to hopefully be a bit of a softer first step for people who (like me) aren't well versed in javascript or PeerJS. It actually ended up being fairly simple, and it's amazing how much WebRTC and PeerJS can do. 

## The Boring Parts: Setup & PeerJS Server
I'll try and get the boring parts out of the way first. Installing the NPM packages and running the server can be tricky, but are more or less formulaic. To install the packages run `npm init` and respond to the command prompts. I usually just use all the defaults, but if you're making a real project, you should give it some thought. 

Then run:
```
npm install express --save
npm install peer --save
npm install pug --save
npm install peerjs --save
```
As of Jan 2021, this seems to work fine.

## Running the server

There are two ways of running the PeerJS server. The first is a bit easier, but the second is more customizable. 

### The First Way: 
Running the `peerjs` tool. I don't like npm installing things globally, as I often work on the same server as other users. So the first thing to do is add the `peerjs` binary to the environment PATH using
```
export PATH=$PATH:"./node_modules/.bin/"
```
Then we can run 
```
peerjs --key peerjs --path /myapp --port 8080
```
Which starts the server. 


### The Second Way:
The other way to run the peerjs server is manually writing the server using an ExpressPeerServer. Writing the following code will run a server at port 8000. We'll be using this way of running Server when we write the client. 
```
const express = require("express");
const http = require('http');
const path = require('path');
const app = express();
const server = http.createServer(app);

const { ExpressPeerServer } = require('peer');
const port = process.env.PORT || "8000";

const peerServer = ExpressPeerServer(server, {
    debug: true,
    path: '/myapp',
    ssl: {}
});

app.use(peerServer);

app.use(express.static(path.join(__dirname)));

app.get("/", (request, response) => {
    response.sendFile(__dirname + "/index.html");
});

server.listen(port);
console.log('Listening on: ' + port);
```
Running the server is as easy as:
```
node server.js
```
## The Fun Part: The Client

First lets write some html to read in the peerjs javascript file and give our webpage a little bit of structure. Below, the line that begins with `<script src=` reads in peerjs. 

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="index.css">
    <!-- import the peer javascript file -->
    <script src="https://unpkg.com/peerjs@1.3.1/dist/peerjs.min.js"></script>
</head>
<body>
    <div class="chatPage">
        <div class="statuses">
            <p id="caststatus" class="big">
                The peerjs id is ???
            </p> <p id="connectedStatus"></p>
            <br />
        </div>
        <input type="text" id="myText"><input type="submit" value="Submit" onclick="connectToPeer()">
        <div id="chatArea">
        </div>
        <textarea id="chatSend" rows="4" cols="80"></textarea>
        <input type="submit" value="Send" onclick="sendChat()">
    </div>
</body>
</html>
```
Open localhost:8000 (or whatever port you used on your server) and you should be able to see the rendering of the app. Most of the components won't work yet, and you won't have connected to the server yet. However connecting to the server is easy. In the `head` tags, after peerjs is imported, add the following:
```
<script> 
const peer = new Peer(''+Math.floor(Math.random()*2**18).toString(36).padStart(4,0), {
  host: location.hostname, // Right now just using localhost
  port:8000, // Using the port of our server
  debug: 1,
  path: '/myapp' // We set this when we made the server
});
window.peer = peer;
peer.on('open', function () {
    window.caststatus.textContent = `Your device ID is: ${peer.id}`;
});
</script>

```
Once you open your localhost:8000 page, instead of seeing: `The peerjs id is ???` at the top, it should be `Your device ID is: <id>`

This tells you that you've successfully created an ID and connected to the server. 

