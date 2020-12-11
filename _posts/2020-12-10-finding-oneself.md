---
layout: post
title: "'Find'-ing oneself pt 1: Finding the Last File"
categories: Bash
---
I love `find`. Neuroimaging has this compulsive need to put everything into separate files and folders, turning the folder system into a makeshift database (see: BIDS). This can make even the simplest tasks turn into nightmares of `cd`-ing and `ls`-ing in and out of directories to try and get anything done. 

This is where `find` comes in. A single line command with a billion flags that can do almost anything. This probably won't be the last post on find, so let's say this is part 1. 

## Finding the last file

I had the following problem today: Find the last folder created or modified exactly 2 subfolders below the current directory.
For example, in the following diagram I would want the last folder modified at level "C":

`A|-> B -> C -> D

  |-> B -> C -> D `


## Listing the directories

It's easy enough to list the directories, I don't even have to google the command anymore!

`find . -mindepth 3 -maxdepth 3`

That should give us all the directories at a certain level. Printing the timestamp is another story though, and I always forget the specific mapping of month/day/year/hour/second/minute.

## Printing the Time

So according to google, I need the code: `%TY-%Tm-%Td %TH:%TM:%TS`
This gives me the year-month-day followed by the hour-minute-second. Note that the order is decreasing in timescale which allows me to sort everything numerically without having to implement some fancy date sort. Keeping stuff in this format makes life much easier. 

We have to put this in a printf statement, so what we'll have is:

`find . -mindepth 3 -maxdepth 3 -printf "%TY-%Tm-%Td %TH:%TM:%TS %p\n"`

Note the `%p \n ` at the end there. This tells us we want to print the filename and then a newline. 

## Sorting 

Ok, so we end up with something like 
`2020-12-10 18:31:31.7068330430 ./.cache/gnomes/shell-extensions`
`2020-12-10 18:31:49.8657847770 ./.cache/gnome-software/bleh` 

What can we do with it? The nice thing about bash is that we can pipe the outputs to other commands, so `find` doesn't have to do ALL the heavy lifting for us. 

We want to sort the entire file structure, then isolate the last file. We'll use the aptly named `sort` function then take the `tail` file. 

`find . -mindepth 3 -maxdepth 3 -printf "%TY-%Tm-%Td %TH:%TM:%TS %p\n" | sort | tail -n 1 # The -n 1 says I only want the last file`

If you're wondering what the \"\|\" is doing in the command, I'd suggest googling the "pipe" operator in Bash. It's worth learning about if you're spending any time on the command line. 

The output of the above command gives us a single file, we're almost done!
`2020-12-10 18:35:31.7068330430 ./.cache/gnome-bool/research/` 


## Printing and watching

The above result is useful enough, but not very pretty. To just get the name of the file, you can cut off the date and time with `cut`:

`find . -mindepth 3 -maxdepth 3 -printf "%TY-%Tm-%Td %TH:%TM:%TS %p\n" | sort | tail -n 1 | cut -d" " -f3-`

Finally, if you want to continually refresh this command, to watch as new files come in, you can put the entire thing under `watch`

`watch 'find . -mindepth 3 -maxdepth 3 -printf "%TY-%Tm-%Td %TH:%TM:%TS %p\n" | sort | tail -n 1 | cut -d" " -f3-'`


