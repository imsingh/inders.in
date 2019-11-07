---
layout: ''
title: Prototype and Inheritance in JavaScript
category: ''
comments: true

---
## Inheritance

### Class-based

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