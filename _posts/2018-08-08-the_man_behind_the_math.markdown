---
layout: post
title:      "The Man Behind the Math"
date:       2018-08-08 07:05:28 +0000
permalink:  the_man_behind_the_math
---


![](https://i.imgur.com/irtA5mr.jpg?2)

My grandfather was a mathmetician and he died before I was born. For many years I believed I held no mathematical abilities and that made him feel even more like an unknowable ghost to me. But I find the more I code the more I'm interested in my grandfather and because of that, more interested in mathemeticians in general.

George Boole was a 19th century self taught mathemetician who developed ( you probably guess ed it) Boolean logic which is where we get Booleans from in programming. Developed long before the invention of computers and even electric light blubs Boole's findings seemed to have no engineering qualities until they were picked up by Claude Shannon while at MIT who showed of Boolean logic could optimize electromechanical relays, which then go on to form important componets in early computers. 

Boolean logic is essential to both hardware and software because it works on a simplistic and human intuitve level. True or Not True. Yes or No, Up or Down, In or Out, On or Off, 1or 0. Boolean logic is what gave us binary code. Every thing you use today that uses electricity uses Booleans at some point. And as developers we will Booleans over and over so it's vital to understand it. So here are some quick reminders about how Booleans work.

## 1. Exclude the Middle
The Law of Excluding the Middle simply means you take statement P and evaluate whether P is true or false. Unlike in journalism or in moral life there are no "grey areas." When it comes to Booleans it either is or it isn't. That's what it means  to exclede the middle. Forget the grey. 


## 2. Fudemental Opperators
Where Booleans get tricky is when we need to combine statements with other statements to get the answer to a bigger statement. We combine statements together using three opperators And, Or, and Not. 

Here is an example: 

A simple statement might be "is it Sunny" or Sunny?.  If Sunny? is true then locially Not_Sunny? is false. 

But what if the computer doesn't know what Sunny? even means. We'd have to teach it right? So to teach the computer what Sunny? is we might say, "If Not_Cloudy And Not_Rainy And Not_Dark, then Sunny? == true" Just remember each statement is evaluated true or false. And then, with the opperator logic, the larger statement is evaluated true or false. Speaking of which here is a table to help you remember. 

![](https://i.imgur.com/lDS1fIG.png)        



