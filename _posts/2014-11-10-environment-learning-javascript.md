---
layout: post
title: Environment for Learning JavaScript
category: blog
comments: true
---

Sometimes it becomes very complicated for programmers to learn JavaScript if they follow traditional Environment. In Simple words, if you just want to learn JavaScript you don't need to create a HTML File and Attach a JavaScript file to it and save and reload again and again for experimenting with code. Following are some easy ways to create a perfect environment for learning JavaScript. 

  * Firefox way : If you are fan of Mozilla Firefox, then you have a good news. Firefox has a built in tool called [Scratchpad](https://developer.mozilla.org/en-US/docs/Tools/Scratchpad). It allows you to type multiple lines of JavaScript code and see the output in Console. Click [here](https://developer.mozilla.org/en-US/docs/Tools/Scratchpad) for Official Page of Scratchpad.

  * Chrome : If you are using Chrome, then you can still type multiple line of JavaScript code in Web Console. Instead of Pressing Enter, press Shift + Enter and you will get a newline for typing code and then press Enter for Running your code.
  
  * NodeJS: It is my personal Favorite. If you have nothing to do with Browser's DOM and you are only experimenting with JavaScript Code. It is perhaps the best way for learning JavaScript.  You need to install [node.js](nodejs.org) in your system. 
    * type your code in a file say **_index.js_**
    * run it with nodejs as _**node index.js **_in command prompt.

You can also use nodemon with nodejs for automatically reloading your JavaScript code when it changes.

  1. install nodemon with _**npm install -g nodemon**_
  2. Type your code in a file say _**index.js**_
  3. Instead of running it with node, run it with _**nodemon index.js**_
