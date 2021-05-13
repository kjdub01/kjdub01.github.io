---
layout: post
title:      "Expanding Knowledge on Promises"
date:       2021-05-13 16:54:45 -0400
permalink:  expanding_knowledge_on_promises
---


It seems like so long ago we were learning about promises and I have seen a lot of blogs and writing about interview questions that focus on a programer's understanding of promises. And since having a good understanding of promises is something required in our React-Redux project, I thought it would be a good chance to review the concept and expand on my knowledge.

##What is a promise?

Promises are one way we deal with asynchronous operations. Which if you think about what a promise is in human interactions makes since. When I promise to do something for a friend I have a pending obligation which is usually time bound. Now that time may be more or less defined. If my friend goes out of town, I may promise to feed her cat at 5pm everyday. But I may also promise my friend that the next time their favorite band come into town I'll go to the concert with them. I don't know when their band will be in town next so the promise is pending the band's tour announcement. When my friend goes out of town, if I fullfil my promise, then I went every day to feed her cat, or when the band was in town I went to the concert. If I didn't do these things then I broke or rejected my promise. 

Then we can infer from this that promises are JS objects that may produce a single value sometime in the future: either resolved(fulfilled) or rejected(broken). Thus there are only three states of a promise pending, resolved, or rejected.[1]

##What does this mean for me?

Aren't we solving a problem that really isn't a problem? JavaScript used callbacks for a really long time right? 

Yes, JavaScript has used callbacks for a really long time and made asyncronous operations work. And there are still some who prefer callbacks in many situations to promises. But one of the biggest advantages to promises is that they employ chaining. And chaining makes code easier to read and understand, which makes is easier to debug! It also didn't get rid of callbacks, .then() takes 2 callbacks one to handle the resolved promise and one to handle the rejected promise and can optionally return a new promise to be chained.[2] 
```
//employing callbacks
firstRequest(function(response) {  
    secondRequest(response, function(nextResponse) {    
        thirdRequest(nextResponse, function(finalResponse) {     
            console.log('Final response: ' + finalResponse);    
        }, failureCallback);  
    }, failureCallback);
}, failureCallback);


//same operation using a promise
firstRequest()
  .then(function(response) {
    return secondRequest(response);
}).then(function(nextResponse) {  
    return thirdRequest(nextResponse);
}).then(function(finalResponse) {  
    console.log('Final response: ' + finalResponse);
}).catch(failureCallback);
```
[3] Much easiser to read! This doesn't mean that chaining is always easy to understand. As with any code the deeper nested your logic the harder to debug.

##Error Handling

Ok.... but I don't see two callbacks in promise version of this example. No! you don't. There may be cases where a rejected promise needs to be handled immediately but if this isn't the case it is a lot simpler to just leave the rejection error till the end. For this we can use .catch(), which is really just a .then() without a callback function for resolving the promise.[2]

You should get into the default habit of using .catch() instead of relying on the second callback in the .then method. Look at the example from Eric Elliot below.

```
save().then(
  handleSuccess,
  handleError
);
```

If handleSuccess throws an error in this pattern it is going to be swallowed up because there is nothing to catch the the rejection of handleSucess. Instead this pattern is considered better.

```
save()
  .then(handleSuccess)
  .catch(handleError)
;
```

because the .catch() with handle the errors from save and handleSuccess.[2]

##Summary
* Promises are JS objects that may produce a single value sometime in the future: either resolved(fulfilled) or rejected(broken). There are only three states of a promise pending, resolved, or rejected.
* Promises employ chaining and chaining through .then() and .catch() making code easier to read, understand, and debug! 
* Always use .catch() at then end of your chains, unless you need to know sooner about a rejected promise


1[](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261) 
2 [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
3 [](https://www.freecodecamp.org/news/javascript-es6-promises-for-beginners-resolve-reject-and-chaining-explained/)
