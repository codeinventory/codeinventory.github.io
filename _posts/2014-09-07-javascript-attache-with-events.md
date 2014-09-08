---
author: ameyjadiye
layout: post
title: "Javascript - ♥ Attach with events"
date: 2014-09-07 23:51
comments: true
tags:
- javascript
---

Javascript is going very populer now days, javascript was created in 10 days in 1995 by _Brendan Eich_, JavaScript was not always known as JavaScript: the original name was Mocha, a name chosen by Marc Andreessen, founder of Netscape. In September of 1995 the name was changed to LiveScript, then in December of the same year, upon receiving a trademark license from Sun, the name *JavaScript* was adopted. This was somewhat of a marketing move at the time, with Java being very popular around then. I love javascript too much and started digging in that through Node.js which made boom of javascript everywhere as you can use it on serverside too.

javascript is so populer just because its more near to actual users of your system; server side code is as important as client side because user is belives what he see; javascript responds to events whatever user is doing with his actual gestures like click, mouseover, keypress etc, thats why the core of javascript is not just a data manupulation but the event handling.

Its very easy wayto make your component responsive.

```html
<html>
    <body>
        <div id='myId'>Click Me!!</a>
    </body>
    <script>
	var id = document.getElementById("myId");
	id.addEventListener("click", function(){
    		document.getElementById("myId").innerHTML = "Wow, you just clicked me, i'm on 7th sky!";
	},true);
    </script>
</html>
```

it's so easy right? you can attach helllot of events, few of them i described below.


|Event      | Description                                        |
|-----------| ---------------------------------------------------|
|onchange   | An HTML element has been changed                   |
|onclick    | The user clicks an HTML element                    |
|onmouseover| The user moves the mouse over an HTML element      |
|onmouseout | The user moves the mouse away from an HTML element |
|onkeydown  | The user pushes a keyboard key                     |
|onload     | The browser has finished loading the page          |

#### Note : dont use 'on' when passing your event to method addEventListener

Now as you see there are 3 parameters to method :

+ First is event you would like to attach.
+ Second is function you want to execute on event happen.
+ Third is called useCapture, its little intresting, let me explain you in brife below.

When the browser event system was first designed, there were two conflicting ways that how shoudl it behave. That is called as *capture* and *bubbling*

If an event (e.g. a click) happens on the a element, should the parent elements know? It was widely accepted that they should. But the question was in what order they should be notified. The Microsoft and Netscape developers had differing opinions.

One model was event capture (advocated by the Netscape developers). This notified the html element first and worked its way down the tree:

+ html
+ body
+ div

The other model was event bubbling (advocated by the Microsoft developers). This notified the target element first, and worked its way up the tree:

+ div
+ body
+ html

According to [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget.addEventListener)  ***useCapture*** is :

```
If true, useCapture indicates that the user wishes to initiate capture. After initiating capture, all events of the specified type will be dispatched to the registered listener before being dispatched to any EventTargets beneath it in the DOM tree. Events which are bubbling upward through the tree will not trigger a listener designated to use capture. See DOM Level 3 Events for a detailed explanation
```

Happy Codeing .... :)
