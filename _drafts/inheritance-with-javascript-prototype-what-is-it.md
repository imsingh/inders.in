---
layout: post
title: 'Inheritance with JavaScript prototype: What is it?'
category: blog
comments: true

---
**TL;DR:** In this article, we will see how inheritance works in JavaScript world. We will deep-dive into prototypes and how to use them to do inheritance in JavaScript. Furthermore, we will see also see how the prototypical approach is different from class-based inheritance.

## Inheritance

Inheritance, a prominent feature of a programming language, emerged with the introduction of object-oriented programming languages. Most of these languages were class-based languages. Meaning, in order to create an object, first we have to create a class.

Following is an example of how we create a class and an object in C++:

    class SmartPhone {
      public:
      void captureImages() {}
    };
    
    // creating object of SmartPhone
    SmartPhone x;
    x.captureImages();

We created a class named _SmartPhone_ and it has a method named _capturePictures,_ to capture images.

Let's imagine, we need an iPhone class, which should capture images along with some special features like face id scan. **This is where inheritance comes into play;** _a way to inherit features from other classes/objects._

Following is how we can inherit capturePictures method from SmartPhone class, in our new Iphone class, in C++ :

    class Iphone: public SmartPhone {
      public:
      void faceIDScan() {}
    };
    
    // creating object of Iphone
    Iphone x;
    x.captureImages();
    x.faceIDScan();

Above is a trivial example of inheritance, where we simply re-used capturePictures method.

## What is Prototype?

In JavaScript, objects have a special internal property; basically a reference to another object. This reference depends upon how the object is created. In ES6/JavaScript specification, it is called as `[[Prototype]]`

Since `[[Prototype]]` itself is an object, it has its own `[[Prototype]]` reference. This is how a chain is built, termed as the **prototype chain.**

> This chain of prototype is the building-block of Inheritance in JavaScript.

Almost all the objects in JavaScript are derived from `Object`, which is why we can access all the methods available on `Object` constructor in other objects.

### `__proto__` object

To access the object's `[[Prototype]]`, all the browsers have __proto property.

This is how we can access it:

    obj.__proto__

It's important to notice that, this property is not part of the ECMAScript Standard. It is a de-facto implementation by the browsers.

### Get and Set Prototype Methods

Apart from the `__proto__` property, there is a standard way to access the `[[Prototype]]`.

Following is how we can access the `[[Prototype]]` of an object:

    Object.getPrototypeOf(obj);

There is a similar method to set the \[\[Prototype\]\] of an object. This is how we do it:

    Object.setPrototypeOf(obj, prototype);

### `[[Prototype]]` and `prototype`

We have discussed **\[\[Prototype\]\]**. _It's nothing but a standard notation to designate the prototype of an object._ Many developers get confused with **.prototype** property, which is an entirely different thing.

Let's explore the **.prototype** property.

In JavaScript, there are many ways of creating an Object. One way is using a constructor function. Following is how you create and use a constructor function :

    function SmartPhone(os) {
      this.os = os;
    }
    
    // creating an object from constructor function
    let phone = new SmartPhone('Android');

When you **console.log** the `phone` object, this is what you get: 

    {
    	os: "Android",
    	__proto__: {
    		constructor: ƒ SmartPhone(os),
    		__proto__: Object
        }
    }

Now, if we want to have some methods on the SmartPhone object, we can do the following:

    SmartPhone.prototype.isAndroid = function() {
    	return this.os === 'Android' || 'android';
    }

Now, if we create the phone object again, we would see following in the console.log:

    {
    	os: "Android",
    	__proto__: {
        	isAndroid: ƒ (),
    		constructor: ƒ SmartPhone(os),
    		__proto__: Object
        }
    }

_In short, .prototype property, is basically like a blueprint for the **\[\[Prototype\]\]** object created by the given constructor function._ Anything that you declare in .prototype property/object will pop up in object's \[\[Prototype\]\]

Can we have methods inside the constructor function?

### What happens when we access a property?

## Various ways of Prototypical Inheritance

In JavaScript, there is just prototypical inheritance. No matter how we create an Object. But still, there are subtle differences, that we should take a look upon.

### Object Literal

The easiest way to create an object in JavaScript is by using an object literal. This is how we do it:

    let obj = {}; 

If we log the obj in the browser's console, we will see the following:

// browser log image

// it has the default object prototype

### Using Object Constructor

One another, not-so-common way of creating an object is using Object constructor. JavaScript provides a built-in Constructor method named, _object_ to create Objects.

Following is how we use it:

    let obj = new Object();

if we log the obj in the browser's console, we will  see the following:

// browser log image

// it has the default object prototype

### Constructor Method

Just like we have Object constructor function provided by JavaScript runtime. Similarly, we can create our own constructor, to create an object which suits our needs.

A constructor function is very similar to a class. In fact, in the next example, we will see a detailed comparison.

    function MyObject {
    
    }
    
    let obj = new MyObject();
    console.log(obj);

### ES6 Class

    class MyClassObject {
    	aMethod() {
          // do something
        }
    }
    
    let obj = new MyClassObject();

### Object.create

    let obj = Object.create();