---
layout: post
title: Adding AdMob Ads in Ionic App
category: blog
comments: true
---

Using Ionic and Cordova for My Mobile App was great. But When I started, I feel so much difficulty as a newbie to display ads in my Mobile App (Check it on PlayStore : [Ragakosh](https://play.google.com/store/apps/details?id=com.ragakosh.app)). I was using this[ plugin](https://github.com/floatinghotpot/cordova-plugin-admob) by floatinghotpot. But I was doing it wrong way. In this post I will share my way of adding Admob Ads in Ionic App. 

Please first follow the steps mention in Plugins [Github Page](https://github.com/floatinghotpot/cordova-plugin-admob) for downloading the plugin! 

I was thinking to display banner ads on my every view of app. So figured out that that Code of Ads should be in place which getsexecuted when the app loads first time. In angularjs, it is **app.run** function and inside **$ionicPlatform.ready**. 

This is the Gist of that Code. 
```js
app.run(function($ionicPlatform) {
  $ionicPlatform.ready(function() {
    var admob_ios_key = "Your Key";
    var admob_android_key = "Your Key";
    var adId =
      navigator.userAgent.indexOf("Android") >= 0
        ? admob_android_key
        : admob_ios_key;

    function createAd() {
      if (window.plugins && window.plugins.AdMob) {
        var am = window.plugins.AdMob;
        am.createBannerView(
          {
            publisherId: adId,
            adSize: am.AD_SIZE.BANNER,
            bannerAtTop: false
          },
          function() {
            am.requestAd(
              { isTesting: false },
              function() {
                am.showAd(true);
              },
              function() {
                alert("failed to request ad");
              }
            );
          },
          function() {
            alert("failed to create ad view");
          }
        );
      } else {
        alert("AdMob plugin not available/ready.");
      }
    }

    createAd();
  });
});
```
    
And If you want to display Interstitial Ads in start of any view. Well I did it in a bad way. I don't want anyone to follow it. Better way is to make it a service but still showing my method. I put the following code from Plugins Github page to start of controller of that particular view. 
    
```js
controllers.controller("myCtrl", function($scope, $http) {
  //Ads Code

  $am = window.plugins.AdMob;
  $am.createInterstitialView(
    {
      publisherId: "Key"
    },
    function() {
      $am.requestInterstitialAd(
        { isTesting: false },
        function() {},
        function() {
          alert("failed to request ad");
        }
      );
    },
    function() {
      alert("Interstitial failed");
    }
  );

  //Rest of Controller's Code
});
```  

I was doing one thing wrong and it was not mentioned by the plugin developer is, you have to create different ad unit for banner ad and interstitial ads. Each ad unit has its own key. So if you are using key of banner ad to display interstitial ads then it won't work. Create different ad units for banner ad and interstitial ads and use their respective key in right place. 

That is it. Thanks.
