---
layout: post
title: "PeerJS tutorial part 3: A simple chat app"
categories: javascript peerjs
---
### Sending Data 

In the last 2 parts of our tutorial we've managed to connect to the server and set up a reciprocal connection to our peers. Now we want to send a message to the other user. We do this in the same way that we send our ids to the other user. We use `conn.send(msg)`. Looking at the HTML, clicking on the Send button should call a function called "sendChat". We implement this here:
```
function sendChat() {
    var data = document.getElementById("chatSend").value
    conn.send(data)
    document.getElementById("chatArea").innerHTML += "<p class=\"alignRight\">" + data + "</p>"
    document.getElementById("chatSend").value = ""
}
```
Here we extract the data in our chatbox with `document.getElementById("chatSend").value`, and send it to the peer. We also want to display our own messages, so we do that with `document.getElementById("chatArea").innerHTML += "<p class=\"alignRight\">" + data + "</p>"`. 

!! Warning - this is actually a crappy way of inserting text into a webpage. It's very vulnerable to simple text insertion attacks, but serves our purpose as a demonstration here. 

Now you should be able to send messages by typing text into the textbox and pressing "Send".

### Styling
Now that we have a *very* basic chat program working, we should do some pretty simple text styling. I made a file called `index.css` and inserted the below in, but frankly it's been a while since I've worked with CSS in any meaningful way. 

```
*, *:before, *:after {
    box-sizing: border-box;
}
#caststatus, #connectedStatus {
    padding:5px;
}

#connectedStatus {
    background-color: #4CBB17;
    visibility: hidden;
    margin-left: auto;
}
.alignLeft{
    text-align: left;
    margin:0px;
}
.alignRight{
    text-align: right;
    margin:0px;
}
.chatPage{
    width:50rem;
}
.statuses{
    display: flex;
}
#chatArea{
    width:595px;
    height:20rem;
    padding:5px;
    overflow-y:scroll;
    overflow-wrap:break-word;
    background-color: #1c2833;
}
#myText{
    background-color:dimgray;
    border:0px;
    padding:5px;
    color:antiquewhite;
}
#chatSend{
    background-color:dimgray;
    border:0px;
    padding:2px;
    color:antiquewhite;
    height:78px;
    width:543px;
}
.conn-btn {
  background-color:#d0451b;
  border-radius:3px;
  border:1px solid #942911;
    display:inline-block;
    color:antiquewhite;
  cursor:pointer;
  font-size:14px;
  padding:5px 22px;
  text-decoration:none;
    text-shadow:0px 1px 0px #854629;
    margin:2px;
}
.conn-btn:hover {
  background-color:#bc3315;
}
.conn-btn:active {
  position:relative;
  top:1px;
}
body{
    background-color:  #283747;
    color:antiquewhite;
}
#sendMessage {
  background-color:#2dabf9;
  border-radius:3px;
  border:1px solid #0b0e07;
  display:inline-block;
  cursor:pointer;
  color:antiquewhite;
    font-family:Arial;
    height:78px;
    position:relative;
    top:-30px;
  font-size:15px;
  text-decoration:none;
}
#sendMessage:hover {
  background-color:#0688fa;
}
#sendMessage:active {
  position:relative;
  top:1px;
}
```



