---
layout: post
title: Creating Radio Player with Ionic 2 and HTML 5
category: blog
comments: true
---

Hi, Folks. Ionic 2 is creating a buzz in Hybrid Development. Developers are loving various features of Ionic 2. If you are not familiar with Ionic 2 yet. Read my previous [post on Ionic2.](/blog/2015/10/28/introduction-to-ionic-2/) 

<!--more-->

Recently I created a Radio Streaming app in Ionic 1. So I decided to put together an Ionic 2 demo for a Radio App. There will be no nitty gritty features. I will just create a basic demo of Radio Player with play and pause buttons. So let's do it. You need to install the beta version of ionic.

```bash    
npm install -g ionic@beta
```    

Now create a blank project with ionic cli. 
    
```bash
ionic start radio blank --v2 --ts
cd radio
ionic platform add ios
ionic platform add android
```    

We can start coding our Radio Player Functionality. Create a radio.ts at /app/radio/radio.ts 

```typescript   
export class RadioPlayer {
  url:string;
  stream:any;
  promise:any;
  
  constructor() {
    this.url = "http://akalmultimedia.net:8000/GDNSLDH";
    this.stream = new Audio(this.url);
  };

  play() {
    this.stream.play();
    this.promise = new Promise((resolve,reject) => {
      this.stream.addEventListener('playing', () => {
        resolve(true);
      });

      this.stream.addEventListener('error', () => {
        reject(false);
      });
    });
    
  return this.promise;
};

pause() {
  this.stream.pause();
};

}
```    

This is our basic Angular 2 Service for Radio Player. We have used HTML5 Audio Component and ES6 Promise. It is pretty basic. The constructor initializes the URL and stream class members, play function play the stream. Promise resolves when playing event is fired and it is rejected when an error event is fired. pause function pauses the audio. Here URL is shoutcast stream url. You can also replace it with any file hosted on Internet. Now edit the home.html file by deleting the unwanted content and adding play and pause button. 
    
```html
<ion-navbar *navbar> 
  <ion-title> Radio Player </ion-title> 
</ion-navbar> 
<ion-content class="home"> 
  <button dark (click)="play()"> 
    <ion-icon name="play"></ion-icon>
  </button> 
  <button dark (click)="pause()"> 
    <ion-icon name="pause"></ion-icon>
  </button> 
</ion-content>
```   
    

Remember that we haven't yet added the play and pause button in our home.ts file. So let's edit our home.ts at /app/pages/home/home.ts 
    
```typescript
import {Page} from 'ionic-framework/ionic';
import {RadioPlayer} from '../../radio/radio';

@Page({
  templateUrl: 'build/pages/home/home.html',
  providers: [RadioPlayer]
})
export class HomePage {
  player:any;
  constructor(player: RadioPlayer) {
    this.player = player;
  }

  play() {
    this.player.play().then(() => {
      console.log('Playing');
    });
  }

  pause() {
    this.player.pause();
  }
}
```
We imported our RadioPlayer class and used that class in our HomePage Class. Take a look at @Page annotation that we have used providers in it. Also check the constructor where we have player parameter of RadioPlayer type. play and pause functions are just calling play and pause of RadioPlayer Class. 

I hope you will be able to create a radio Player with Ionic 2 and HTML5. You can check the source code of this demo at Github repo of [Ionic2-radio](https://github.com/imsingh/ionic2-radio).

Thanks for reading!