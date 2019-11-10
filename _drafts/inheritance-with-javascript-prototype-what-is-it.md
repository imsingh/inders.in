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

Let's imagine, we need an iPhone class, which should capture images along with some special features like face id scan. Here comes the inheritance; a way to inherit features from other classes/objects. 

Following is how we can inherit capturePictures method from SmartPhone class, in our new Iphone class, in C++ :

    class Iphone: public SmartPhone {
      public:
      void faceIDScan() {}
    };
    
    // creating object of Iphone
    Iphone x;
    x.captureImages();
    x.faceIDScan();

Above is a trivial example of inheritance, where we didn't have to write capturePictures method again.

## What is Prototype?

### Accessing Prototype

#### **proto** and prototype

#### Get and Set Prototype Methods

## Various ways of Prototypical Inheritance

### Object Literal

    let obj = {};
    console.log(obj);

### Using Object Constructor

    let obj = new Object();
    console.log(obj);

### Constructor Method

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