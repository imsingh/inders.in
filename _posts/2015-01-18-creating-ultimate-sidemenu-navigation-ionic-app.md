---
layout: post
title: Creating Ultimate Sidemenu Navigation for Ionic App.
category: blog
comments: true
---

One of important feature of Mobile App is its navigation system. So far best UI Element for creating navigation is sidemenus. Ionic as a framework is evolved so much but its default sidemenu is very simple.(Although my app still uses the default :p). In this post I am going to create a better sidemenu navigation system for Ionic Apps. 

These example enhanced version of [ionic-drawer](https://github.com/imsingh/ionic-ion-drawer) by @maxlynch of Ionic Framework. 

**1.a** Link the drawer's CSS and JS of ionic-drawer module and add that module to your app's dependencies. 
```html
<script src="js/drawer.js"></script>
<link rel="stylesheet" type="text/css" href="css/drawer.css">
```
and add the drawer module to app's dependencies. 
```js
angular.module('ionicApp',['ionic','ionic.contrib.drawer'])
```

**1.b** Now layout your Sidemenu in your html file. (This is basic code). 
```html
<ion-side-menus>
  <!-- Center content -->
  <ion-nav-bar class="bar-positive nav-title-slide-ios7 has-tabs-top">
    <ion-nav-buttons side="left">
      <button class="button button-icon icon ion-navicon" ng-click="toggleDrawer()">
      </button>
   </ion-nav-buttons>
    </ion-nav-bar>

  <ion-side-menu-content>
      <ion-view title="Ionic App">
        <ion-content>
        </ion-content>
      </ion-view>
  </ion-side-menu-content>

  <!-- Left menu -->
  <drawer side="left">  
      <ion-nav-bar class="bar-stable nav-title-slide-ios7 has-tabs-top"> 
        <ion-nav-buttons side="left">
          <button class="button button-icon icon ion-navicon" ng-click="toggleDrawer()">
          </button>
        </ion-nav-buttons>
      </ion-nav-bar>
  		
        <ion-view title="Menu">
          <ion-content>
          </ion-content>
        </ion-view>
  </drawer>
</ion-side-menus>
```

You will get something like shown below :

![Ionic-Drawer-Full]({{ "/img/posts/ionic-drawer-full.png" | relative_url}}){: .img-fluid}

[Github Repo](https://github.com/imsingh/ionic-drawer-demo) This is something good. But we can get sworkit like navigation. If you want Sworkit like SideMenu you have to tweak the css of drawer. 

**2.a** Edit the drawer.css of the Github Repo and add top:44px; property to drawer's css like below. 

```css
    drawer {
      display: block;
      position: fixed;
      top:44px;
      width: 270px;
      height: 100%;
      z-index: 100;
    }
```

**2.b** Remove the Navigation button from drawer's html because we won't need it now. Like shown below : 
```html
<drawer side="left">  
  <ion-nav-bar class="bar-stable nav-title-slide-ios7 has-tabs-top"> 
  </ion-nav-bar>
  
  <ion-view title="Menu">
    <ion-content>
    </ion-content>
  </ion-view>
</drawer>
```
The End result of this would be something like below:

![Ionic-Drawer]({{ "/img/posts/ionic-drawer1.png" | relative_url}}){: .img-fluid}

Source Code of it is at [Github repo](https://github.com/imsingh/ionic-sworkit-sidemenu) Is it cool?. Probably good. But not very good. 

3.For third version we are adding little animation. First we are creating a directive for toggling the Navigation. It is very simple. 
```js
.directive('drawerToggle', function() {
  return {
    restrict: 'A',
    link: function($scope, $element, $attrs) {
      var el = $element[0];
      if($attrs.animate === "true") {
        $element.addClass('animate drawerToggle');
      }
      
      $element.bind('click', function(){
        if($attrs.animate === "true") {
          if($scope.toggleDrawer() === "open") {
            el.style.transform = el.style.webkitTransform = 'translate3d(' + -5 + 'px, 0, 0)';
          } else {
            el.style.transform = el.style.webkitTransform = 'translate3d(' + 0 + 'px, 0, 0)';
          }   
        } else {
            $scope.toggleDrawer();
        }
      });
    }
  };
})
```

With this directive we are toggling the drawer and added some animation to the navigation button by adding animate class. You also need to add following css styles in your app. 

```css
.animate {
  -webkit-transition: 0.4s all ease-in-out;
  transition: 0.4s all ease-in-out;
}

.drawerToggle {
  right:25px;
} 
```

and also you have to put the animate="true" attribute to your navigation button. Like Below. 
```html
<ion-nav-buttons side="left">
  <button class="button button-icon icon ion-navicon" drawer-toggle="" animate="true">
  </button>
</ion-nav-buttons>
```
[Live Demo](http://imsingh.github.io/ionic-animated-drawer/)

Source Code of it is at [Github repo](https://github.com/imsingh/ionic-animated-drawer) 

This is all for now. Thanks for reading folks!