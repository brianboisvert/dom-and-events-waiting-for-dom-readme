# DOMContentLoaded event

## Objectives

+ Understand when to use `DOMContentLoaded`
+ Understand how to implement JavaScript with `DOMContentLoaded`

## A problem of timing

When working with JavaScript code, we must be careful about timing.  Things happen in a specific order that can trip us up.  Open up the `index.html` file in a browser.  Then open up your console.  You will see a `null` in the console (refresh the page if you do not).  Where did that come from?  Is that supposed to be there?

Well, if you open up the `index.html` file in the text editor, we can find out.  In the `head` tag of our `index.html` file, we link to JavaScript code located in the `index.js` file.  Let's open that `index.js` file.  The code there should select our `h1` and then log that element to the console.  Our debugger only is hit if the console is open.  So refresh the page with the console open.  

With the code paused on the debugger, if you type in `ada` in the console, you can see that `ada` is `null`.  Do you see why?  There isn't a typo.  Instead, the reason why we cannot select an `h1` is because there is no `h1` on the page.  In fact, if you look at the browser at the time the debugger is hit, you can see that there are no elements on the page.  

The reason why is because our code is simply following our directions.  In our `index.html` file we tell the browser to load our `index.js` file in the `head` tag before any of our HTML is loaded.  So, when the `index.js` file is loaded and run, our JavaScript code attempts to find a tag of `h1` before anything is loaded.  It then logs what it finds, which is nothing, so we see the `null` in our console.

## A quick fix

If we want to quickly fix this, we can simply move that `<script src="index.js"/>` line from inside the `head` tag to down below our `body` tag.  If you do so, and refresh the page, you can see that `ada` now properly points to the `h1` element.  So the point is that the timing really matters.  If we try to select elements before they are on the page, we cannot then attach event listeners or any other behavior to them.  

Let's say that again, this time with feeling: If we try to select elements *before they are on the page*, we cannot then attach event listeners *or any other behavior to them*.  

We really don't want to be dependent on linking to our JavaScript code at precisely the correct spot on the page.  What if more than one HTML file is using our JavaScript code.  It's easy to forget about this for all of our HTML pages.  Instead, we want to tell our JavaScript code to wait until everything is loaded up, before running the code that will select elements and attach behavior.  Well lucky for us, we can do this with use of a JavaScript event.

## DOMContentLoaded

Instead of immediately executing our selectors and event listeners when a file is loaded, we can have this code wait until our page is loaded.  How do we do this?  Well the page "being loaded" is an event, and we can ask JavaScript to only run the code to select elements and add behavior after the all of the elements are on the page.  Let's move our `index.js` file back up to the top to text that our fix properly works. Here is what our new code should look like:

```js
document.addEventListener("DOMContentLoaded", function(event) {
  let ada = document.querySelector('h1')
  console.log(ada)
});
```   

Ok, so as you can see, we are no longer directly selecting elements when the `index.js` file is run.  Instead, running the `index.js` file calls the `addEventListener` method on `document` and only when the HTML content is loaded does it look for an `h1` and then log the element.  That is why our code now works.  It is not unusual to wrap all of JavaScript code related to the DOM in a `document.addEventListener('DOMContentLoaded')` call.  

Another way to do that is to simply execute methods that attach the behavior after the DOM has loaded.  Let's see how:

```js

function queryAndLog(){
  let ada = document.querySelector('h1')
  console.log(ada)
}

document.addEventListener("DOMContentLoaded", function(event) {
  queryAndLog()
});

```

This code works identically to the code above.  So choosing one way versus the other is just a matter of style preferences.  Here we say execute the `queryAndLog` function only when the DOM content is loaded, and that `queryAndLog` function finds the `h1` tag and logs tag. 

### Summary

In this lesson, we saw that we can run into problems by invoking our JavaScript code immediately.  This is because our code may be attempting to take actions like selecting elements or adding event listeners before the elements have been added to the browser.  By making use of the `DOMContentLoaded` event we can wait until the content is loaded before running our DOM dependent JavaScript code.  

## Resources

+ [DOMContentLoaded](https://developer.mozilla.org/en-US/docs/Web/Events/DOMContentLoaded)


<p class='util--hide'>View <a href='https://learn.co/lessons/js-DOMContentLoaded'DOMContentLoaded</a> on Learn.co and start learning to code for free.</p>
