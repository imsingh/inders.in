---
layout: post
title: Creating Ultimate Sidemenu Navigation for Ionic App.
category: blog
comments: true
---

One of important feature of Mobile App is its navigation system. So far best UI Element for creating navigation is sidemenus. Ionic as a framework is evolved so much but its default sidemenu is very simple.(Although my app still uses the default :p). In this post I am going to create a better sidemenu navigation system for Ionic Apps. These example enhanced version of [ionic-drawer](https://github.com/imsingh/ionic-ion-drawer) by @maxlynch of Ionic Framework. **1.a** Link the drawer's CSS and JS of ionic-drawer module and add that module to your app's dependencies. 
    
    
      
      
    

and add the drawer module to app's dependencies. 
    
    
    angular.module('ionicApp',['ionic','ionic.contrib.drawer'])
    

**1.b** Now layout your Sidemenu in your html file. (This is basic code). 
    
    
    
      
      
        
          
          
       
        
    
      
          
            
            
          
      
    
      
        
           
            
              
              
            
          
      		
            
              
              
            
      
    
    
    

You will get something like shown below : ![ionic-drawer-full](http://inders.in/wp-content/uploads/2015/01/ionic-drawer-full.png) [Github Repo](https://github.com/imsingh/ionic-drawer-demo) This is something good. But we can get sworkit like navigation. If you want Sworkit like SideMenu you have to tweak the css of drawer. **2.a** Edit the drawer.css of the Github Repo and add top:44px; property to drawer's css like below. 
    
    
    drawer {
      display: block;
      position: fixed;
      top:44px;
      width: 270px;
      height: 100%;
      z-index: 100;
    }
    

**2.b** Remove the Navigation button from drawer's html because we won't need it now. Like shown below : 
    
    
     
        
           
          
      		
            
              
              
            
      
    

The End result of this would be something like below:![ionic-drawer](http://inders.in/wp-content/uploads/2015/01/ionic-drawer1.png) Source Code of it is at [Github repo](https://github.com/imsingh/ionic-sworkit-sidemenu) Is it cool?. Probably good. But not very good. 3\. For third version we are adding little animation. First we are creating a directive for toggling the Navigation. It is very simple. 
    
    
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
    

With this directive we are toggling the drawer and added some animation to the navigation button by adding animate class. You also need to add following css styles in your app. 
    
    
    .animate {
      -webkit-transition: 0.4s all ease-in-out;
      transition: 0.4s all ease-in-out;
    }
    
    .drawerToggle {
      right:25px;
    }
    
    

and also you have to put the animate="true" attribute to your navigation button. Like Below. 
    
    
     
          
          
       
    

[Live Demo](http://imsingh.github.io/ionic-animated-drawer/)

Source Code of it is at [Github repo](https://github.com/imsingh/ionic-animated-drawer) This is all for now. Thanks for reading folks!

## Comments

**[LorenH](#7 "2015-01-22 18:35:00"):** Nice work Indermohan! The mobile drawer menus are what attracted our company to the Ionic Framework initially. Your enhancements match the Android specs well. http://developer.android.com/design/patterns/navigation-drawer.html

**[tno2007](#8 "2015-04-15 20:48:00"):** thanks so much, the ionic default drawer is very basic, i was looking how to style it better. personally i like the first picture's style over the other.

**[musicalinder](#9 "2015-04-16 04:15:00"):** @tno2007:disqus It is good to see that my work is helpful to you. Thanks and Every one has their own preference. Use whatever you like.

**[ryan](#11 "2015-05-11 05:39:00"):** Hi I follow instructions step by step but I don't get the result as you discribe. I linked drawer js and css, then I add sidemenu.html. But my view title doesn't appear on sidebar. I checked html source. My html has many more class. Could you help me? I added my html belowed. It is different from your demo http://imsingh.github.io/ionic-animated-drawer/#/main

**[musicalinder](#12 "2015-05-19 17:46:00"):** @disqus_2OaQMnlxx8:disqus Can you create a codepen and send link to my email at indermohansinghk7@gmail.com. I will be happy to help you. Thanks.

**[kevin jose](#14 "2015-05-28 03:13:00"):** thank you so much for the share ..!! :)

**[piyush aggarwal](#183 "2016-01-28 18:16:00"):** How disable drawer on inner pages.. I want it to be enable on only on home page.

**[piyush aggarwal](#184 "2016-01-28 18:18:00"):** How to close the drawer on clicking outside the drawer?

**[Chirag thaker](#191 "2016-07-14 06:42:00"):** hi, nav drawer looks good. style is working but how to implement in live real app. means i added items but clicking on items does not open view

