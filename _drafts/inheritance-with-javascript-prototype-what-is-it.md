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

### `[[Prototype]]` and `.prototype` property

We have discussed **\[\[Prototype\]\]**. _It's nothing but a standard notation to designate the prototype of an object._ Many developers get it confused with **.prototype** property, which is an entirely different thing.

Let's explore the **.prototype** property.

In JavaScript, there are many ways of creating an object. One way is using a constructor function. Following is how you create and use a constructor function :

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

Now, if we want to have some methods on the _phone_ object, we can use .prototype property on the function, as follows:

    SmartPhone.prototype.isAndroid = function() {
    	return this.os === 'Android' || 'android';
    }

When we create the phone object again, we would see following in the console.log:

    {
    	os: "Android",
    	__proto__: {
        	isAndroid: ƒ (),
    		constructor: ƒ SmartPhone(os),
    		__proto__: Object
        }
    }

We can see the isAndroid() method in the object's \[\[Prototype\]\].

_In short, .prototype property is basically like a blueprint for the **\[\[Prototype\]\]** object created by the given constructor function._ Anything that you declare in .prototype property/object will pop up in object's \[\[Prototype\]\]

It's worth noting that, we can also create methods inside the constructor function. Instead, we did it using the function's prototype. There is a good reason to do so.

Let's take a look at the following example:

    function ObjectA() {
      this.methodA = function() {}
    };
    
    let firstObj = new ObjectA();
    console.log(firstObj);
    
    // log
    /**
    {
      methodA: ƒ ()
      __proto__: Object
    }
    **/

The problem with this approach is when we initiate a new object. All the instance gets their own copy of _methodA_. But when we create it on function's prototype, all instances of the object share just one copy methods. Which is more efficient.

### What happens when we access a property?

When we try to access a property on an object. Following happens:

1. JavaScript engine looks for the property on the object.
   1. If it finds the property, then it returns/execute it.
   2. Otherwise, it does the following.
2. JavaScript Engine then checks the inherited property of an object by looking at \[\[Prototype\]\].
   1. If the property is found, it gets executed/return.
   2. Otherwise, it looks into \[\[Prototype\]\] of \[\[Prototype\]\]. This chain ends when either the property is found or there is no \[\[Prototype\]\] left, which means that we have reached the end of the prototype chain.

## Various ways of Prototypical Inheritance

In JavaScript, there is just prototypical inheritance. No matter how we create an Object. But still, there are subtle differences, that d we should take a look upon.

### Object Literal

The easiest way to create an object in JavaScript is by using an object literal. This is how we do it:

    let obj = {}; 

If we log the obj in the browser's console, we will see the following:

// browser log image

// it has the default object prototype

So basically, all the objects created with literal notation inherits properties from Object.prototype.

### Using Object Constructor

One another, not-so-common way of creating an object is using Object constructor. JavaScript provides a built-in Constructor method named, _object_ to create Objects.

Following is how we use it:

    let obj = new Object();

This approach results in the same object as object literal notation.

### Constructor Method

Just like we have Object constructor function provided by JavaScript runtime. Similarly, we can create our own constructor, to create an object which suits our needs.

A constructor function is very similar to a class. In fact, in the next example, we will see a detailed comparison.

    function SmartPhone() {}
    SmartPhone.prototype.captureImages = function() {};
    
    function Iphone() {} 
    Iphone.prototype = SmartPhone.prototype;
    Iphone.prototype.faceIDScan = function() {}
    
    let x = new Iphone();
    x.captureImages();
    x.faceIDScan();

### ES6 Class

    class SmartPhone {
      captureImages() {}
    };
    
    class Iphone extends SmartPhone {
       faceIDScan() {}
    }
    
    let x = new Iphone();
    x.captureImages();
    x.faceIDScan();

### Object.create method

With this helper method, we can create an object with an another object as it's \[\[Prototype\]\].  Following is an example:

    let SmartPhone = {
      captureImages: function() {}
    }
    
    let Iphone = Object.create(SmartPhone);
    
    
    Iphone.captureImages();
    Iphone.faceIDScan();

### Object.assign method

Ever wonder, if you can create an Object which has no inherited methods. With Object.assign, you can do that.

    let SmartPhone = {
      captureImages: function() {}
    };
    
    let Iphone = Object.create(SmartPhone);
    Iphone.faceIDScan = function() {}
    
    let iPhoneX = Object.assign(SmartPhone, Iphone);

## Conclusion