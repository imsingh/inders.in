---
layout: post
title: 'Inheritance with JavaScript prototype: What is it?'
category: blog
comments: true

---
**TL;DR:** In this article, we will see how inheritance works in JavaScript world. We will deep-dive into prototypes and how to use them to do inheritance in JavaScript. Furthermore, we will see also see how the prototypical approach is different from class-based inheritance.

## Inheritance

Inheritance, a prominent feature of a programming language, emerged with the introduction of object-oriented programming languages. Most of these languages were class-based languages. Meaning, in order to create an object, first we have to create a class.

Following is how we create a class and object in C++: 

    class SmartPhone {
        capturePictures() {
          // magic code to captsure pictures
        } 
    }
    
    // creating object of SmartPhone
    class mobile = new SmartPhone('mobile');

Following is how we create 

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