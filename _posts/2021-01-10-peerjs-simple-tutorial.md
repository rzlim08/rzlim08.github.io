---
layout: post
title: "PeerJS tutorial: A simple chat app"
categories: Miscellaneous
---
```
npm init
npm install express --save
npm install peer --save
npm install pug --save
npm install peerjs --save
```

```
export PATH=$PATH:"./node_modules/.bin/"
```

```
peerjs --key peerjs --path /myapp --port 8080

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

```
node server.js
```


