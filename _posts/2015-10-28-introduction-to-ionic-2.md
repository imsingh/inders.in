---
layout: post
title: Introduction to Ionic 2
subtitle: Basics of Ionic with Angular
category: blog
comments: true
---

Hi Friends. I am back with a special blog post about the upcoming version of Ionic framework,  Ionic v2. Ionic v2 is now publicly released. I was lucky enough to try the ionic v2 before it's public launch. So this post is my experience with Ionic v2 and also and introduction to new framework. 
<!--more-->

In this blog post, I am going to discuss the very basic of Ionic app and what is happening in your app and little differences between angular 2 and ionic 2 apps. So let's get started. 

Ionic 2 is built on the top of **Angular 2**, **TypeScript**. So,this is a total change in terms of development. We don't have $scope, ng-controllers, angular 1.x services and things that were part of ionic v1. Now we have totally new development stuff. Ionic 2 Allows ynou to write your applications in either **ES6**(ECMAScript 6) or TypeScript (Superset of JavaScript). So we now have to deal with stuff like Classes, Decorators, ES6 Modules, Arrow functions, block scope.

Since Ionic 2 is built on top of Angular 2, so we are basically developing angular 2 apps, but with some differences. Let's first see a typical Angular 2 app.

## Angular 2
```js
import {Component, bootstrap} from '@angular/core';
import {MyServices, MyDirectives} from 'myapp';

@Component({
  selector: 'my-component',
  template: 'Hello, {{name}}',
  directives: [MyDirectives]
})
export class MyComponent {
  constructor() {
  this.name = 'Max';
  }
}
bootstrap(MyComponent, [MyServices]);
```

First we are importing stuff like Component and bootstrap from '@angular/core' module. Every angular 2 apps is centered around Components. Components are UI Elements, which consist of two parts : Component Annotation and Component Controller. We can have components inside our component. So angular 2 apps are tree of components. 

  * **@Component** annotation tells about the selector of our component. In this case, it is **my-component, **and we also have to inject the services required by the component using **providers **key inside component or during** bootstrap. **We also have to Inject the directives that we are using in this component by using **directives **property. In this case, **MyDirective **is the directive that we are using in this component.

  * **class** is basically the **controller** of our component, through which we are controlling the behavior of our Component.

  * At Last, we are bootstraping our applications manually by using **bootstrap**.We are also injecting our service **MyServices** in bootstrap.

When we use the tag in our HTML, this component will be created,our constructor called, and rendered. 

You can easily notice, that it is very different from angular 1 applications. In Angular 1, we start from **ng-app**, which also automatically bootstrap our applications. We also don't have **angular.module** system. 

# Ionic 2

Now we have created the ground for explaining a basic Ionic 2 App. Let's move to Ionic 2. 

Best way to getting started with Ionic 2 is using ionic-cli. Follow this installation Guide by ionic team <http://ionicframework.com/docs/v2/getting-started/installation/>. 

But understanding what is happening in the app is very important. 

## A Basic App

```js
import {ionicBootstrap} from 'ionic-angular';
import {Component} from '@angular/core';
@Component({
  templateUrl: 'templates/main.html'
})
class MyApp {
  constructor() {
    this.name = 'Max';
  }
}
ionicBoostrap(MyApp);
```

Woah? Where is selector on component? How this class gonna match to element on View? That was my first reaction.

**ionicBootstrap **basically, select a hard-coded root element named as **ion-app. ** That's why there is no selector on **@Component****. **Initially Ionic team was doing this using Custom Decorators like** @App **and **@Page. **You can still select use any custom root selector of your choice. Just put that in @Component Decorator. ionicBootstrap also inject various things automatically for you. Like FORM_PROVIDERS, IONIC_DIRECTIVES, HTTP_PROVIDERS. It also makes your app styling matched to the platform, it is running. 

If you have any external Services and Directives. You have to include these as mentioned in angular 2 section. 

For more information go to [http://ionicframework.com/docs/v2](http://v2.ionic.io) .This is all about the introduction to Ionic 2 for today. Stay tuned for Upcoming Ionic 2 post in this series.