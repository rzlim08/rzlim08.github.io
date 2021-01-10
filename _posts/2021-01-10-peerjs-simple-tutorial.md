---
layout: post
title: "PeerJS tutorial: A simple chat app"
categories: Miscellaneous
---
First things first, I want to give proper credit, a lot of the code in this tutorial was derived from "https://medium.com/samsung-internet-dev/building-an-internet-connected-phone-with-peerjs-775bd6ffebec". I found it still a bit complex as a first introduction to peerjs, so I made this tutorial to hopefully be a bit of a softer first step for people who (like me) aren't well versed in javascript or PeerJS. It actually ended up being fairly simple, and it's amazing how much WebRTC and PeerJS can do. 

## The Boring Parts: Setup and the PeerJS Server
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
The other way to run the peerjs server is manually writing the server using an ExpressPeerServer. Writing the following code will run a server at port 8000. 
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


