---
layout: post
title: "How to Escape Single Quotes"
categories: Bash
---
I always have to look this up, so I figured I'd write a post about it. 


The way to escape a single quote on the command line is `'\''`. This closes the outer single quote, escapes another quote, and opens a new single quote. 

For example, this doesn't work: 

`bash -c 'echo "Hello, World" | sed 's/, World/, Earth!/' '`

while this does: 

`bash -c 'echo "Hello, World" | sed '\''s/, World/, Earth!/'\'' '`

with some extra spacing, you can see that each single quote in sed has an closing, escaped quote, and reopening quote:

`bash -c 'echo "Hello, World" | sed ' \' 's/, World/, Earth!/ ' \' ' `

Hopefully this makes it less confusing for anyone reading




