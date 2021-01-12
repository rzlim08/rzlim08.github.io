---
layout: post
title: "PeerJS tutorial part 2: A simple chat app"
categories: javascript peerjs
---
### Filling in the Javascript
Continuing on from part 1 of the tutorial, so far, we've managed to get a client that connects to the server that we've created. However, the functions that we've created have not been implemented yet. 

Let's start adding them one by one. 

So after we connect to the server, probably the first thing we will want is to connect to the peers. 
We need to manually put in the code for the peer, then submit it to connect to the peer. 

The below code pulls the data from the "myText" section, and uses it to connect to the other peer. Once we have a connection, we want to 'open' the connection, and send our own peer id so they can connect to us. 
```
var conn;  // connection variable, we want to keep the scope here
function connectToPeer() {
    code = document.getElementById("myText").value; // get value from "myText"
    conn = peer.connect(code); // connect to code
    // on open will run when the peers are connected
    conn.on('open', function(){
        conn.send(peer.id); // send the peer id
        document.getElementById("connectedStatus").innerHTML = `Connected to: ${code}`; // set connected to
        document.getElementById("connectedStatus").style.visibility = 'visible';
    });
}
```

Now we need to figure out what we do when someone else tries to connect to us. Below, the code takes an incoming connection, and if we haven't set up a connection yet, uses the incoming data to set up a connection. 

```
peer.on('connection', function(msg_conn) { // on connect
    msg_conn.on('data', function(data){ // recieves data from connection
        if(!conn){ // if conn is null
            document.getElementById("connectedStatus").innerHTML = `Connected to: ${data}`
            document.getElementById("connectedStatus").style.visibility = 'visible';
            // connect to peer on our side
            conn = peer.connect(data)
            conn.send(peer.id)
        }else{ // if connection is not null
            // set chat area to incoming message
            document.getElementById("chatArea").innerHTML += "<p class=\"alignLeft\">" + data + "</p>"
        }
    });
});

```
