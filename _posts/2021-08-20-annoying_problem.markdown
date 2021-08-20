---
layout: post
title:      "Annoying Problem"
date:       2021-08-20 06:37:38 +0000
permalink:  annoying_problem
---


This week I had some big frustration around this:

```
22:55:39 web.1  | Something is already running on port 3000.
```

Uhhh!?!?!?!?! What is running on port 3000? I'm not running anything there!  
```
lsof -i tcp:3000
COMMAND     PID                USER   FD   TYPE            DEVICE SIZE/OFF NODE NAME
Google    87126 katejewett-williams   30u  IPv4 0x425e696892d74ed      0t0  TCP localhost:62109->localhost:hbci (ESTABLISHED)
node      89917 katejewett-williams   25u  IPv4 0x425e6968f1fde6d      0t0  TCP *:hbci (LISTEN)
```
 Oh...Google is using my 3000 port? Did a little digging and came to this tidbit 

> In general, any website you visit can install a service worker (assuming your browser supports them). If one got installed on localhost:3000, then I'm assuming you used some sort of local development server running, for example, Web Starter Kit, which does ship with a service worker.
> 
> Visit chrome://serviceworker-internals/ to view a list of service workers.
> Click the "Unregister" button to remove the registration of the offender.
> 

Hmmm... ok maybe.....


I ended up just using the Linux kill command to manually stop the offending processes.

```
kill 87126
kill 89917
```

And for me that seemed to resolve the issue without getting involved with Google serviceworkers for now. But If it continues to be a problem. I'll look into that more. But try the kill command and good luck!
