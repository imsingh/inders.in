---
layout: post
title: Writing JavaScript in ES6 for today's world.
subtitle: Tanspiling ES6 with Babel and Gulp
category: blog
comments: true
---


Hi Folks. In this blog post, I am going to talk about using ES6 for today's world. In brief, ES6 a.k.a ECMAScript Harmony is the latest standard of `JavaScript` Language which supports tons of new features like `Classes`, `Modules`, `Block Scope` and so on!. 

Unfortunately most of today's browser not fully support ES6, and you are guessing why this post is? This post is for creating a development environment where you can use ES6 in your production apps. It is true that browser doesn't support ES6 fully. But it doesn't mean we can't write in ES6 today. 

Here comes transpilers for the rescue. Transpilers are the softwares which convert source-code from one language to other language. So, for example ES6 to ES5. So basically what we do is, we will write in ES6 and that code will be converted into `ES5` version that all browsers understand. Cool, isn't it. There are bunch of transpilers available for `JavaScrip`t like *babel*, *traceur*, *closure*. I will be using babel. So, Lets get started. 

First I am laying the structure of the app and briefly explaining it. 
```bash
dist
gulpfile.js
index.html
js
node_modules
package.json
```   

`Explanation`

  * **dist:** In this folder, we will save all our ES5 browser friendly code.
  * **gulpfile.js:** It is the configuration file for Gulp Task Runner. Which we will be using to automate things.
  * **index.html:** It is the entry point to our app.
  * **js:** In this folder we will write our ES6 code.
  * **node_modules:** npm install node_modules in this directory. You can ignore it.
  * **package.json:** This files holds list of dependencies that we require to do configure our development enviornment.
Let's create a folder for it, you can name it anything. I like es6-play, and create a package.json as follows. 


```js
{
    "name": "es6-play",
    "version": "1.0.0",
    "dependencies" : {
    "gulp": "^3.9.0",
    "gulp-babel": "^5.1.0",
    "gulp-connect": "^2.2.0"
    }
}
```

Now run following command for installing npm packages. 
    
```bash
$ npm install
``` 

Now create a gulpfile.js for our gulp task runner. If you are not familiar with it check it at [gulpjs.com](http://gulpjs.com). gulpfile.js 
```js
//gulpfile.js
var gulp = require('gulp');
var babel = require('gulp-babel');
var connect = require('gulp-connect');

// app settings
var settings  = {
    port : 8080
};

// Default Task
gulp.task('default', ['serve','watch'], function(){

});

// Transpile Task
gulp.task('transpile', function() {
    gulp.src('js/**/*.js')
    .pipe(babel())
    .pipe(gulp.dest('dist'));
});

// Watch the js source files and transpile it.
gulp.task('watch', function() {
    gulp.watch('js/**/*.js', ['transpile']);
});

gulp.task('serve', ['transpile'], function() {
    connect.server({
    port: settings.port,
    livereload: true
});
```

Briefly Explaining each function. 

  * **gulp.task('default',** ... : is for creating default task for gulp. When we run gulp command in cli. It wil run.
  * **gulp.task('transpile'**, ... : It basically converts our es6 code written in js folder into es5 code in dist folder.
  * **gulp.task('watch',** ... : It watches the js folder for any changes and then again transpile our code.
  * **gulp.task('serve',** ... : It runs the local server for running our app.
Now create a file name app.js in js folder as follows : 

```js
// js/app.js
class jslibrary {

}
```

We are creating an empty class named jslibrary. Now run the command in terminal or cmd:

```bash
$ gulp
```

You will see the ES5 code in dist/app.js as follows : 
```js
// dist/app.js
"use strict";

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

var MyGod = function MyGod() {
    _classCallCheck(this, MyGod);
};
```

TADA. We have setup our development environment for working with ES6. Wanna play with ES6? Just Checkout my [es6-play](https://github.com/imsingh/es6-play) Github repo. That's it for today's post.