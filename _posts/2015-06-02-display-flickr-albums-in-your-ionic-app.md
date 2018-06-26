---
layout: post
title: Display Flickr Albums in your Ionic App.
category: blog
comments: true
---

Hi Folks. Its been a long time since my last post. I was trying to put together what I have learned in previous couple of months about Angularjs and Ionic. In one of my previous app I used Flickr's album(Photosets). So this post is all about Using Flickr's Photo Albums in Ionic App. 

This is very minimal post. We will make a very small app. So lets get started. 

Before doing any coding stuff. We need to configure Flickr.

Flickr API key from here : <https://www.flickr.com/services/apps/create/apply/> You also need Yahoo Account for creating API Key. ![Flickr1]({{ "/img/posts/flickr1.png" | relative_url}}){: .img-fluid} Click on Request an API Key. 

Then you will see : ![Flickr2]({{ "/img/posts/flickr2.png" | relative_url}}){: .img-fluid} You can choose commercial or non-commercial depending upon your requirement. (I clicked on non-commercial).  

You will see this: ![Flickr3]({{ "/img/posts/flickr3.png" | relative_url}}){: .img-fluid} Fill up the detail and press submit. Voila. 

Here is the key. ![Flickr1]({{ "/img/posts/flickr4.png" | relative_url}}){: .img-fluid}   
That's all we need from Flickr. We are basically fetching all albums of a flickr User. Now on to coding. 

Now we need to create a blank ionic app. 
```bash
    ionic start flickr blank
```

In the `www` folder, open `js/app.js` file and create state as follows: 
```js
.config(function($stateProvider, $urlRouterProvider) {
    $stateProvider
      .state("index", {
        url: "/",
        templateUrl: "templates/main.html",
        controller: "MainCtrl"
      })
      .state("album", {
        url: "/album/:id",
        templateUrl: "templates/album.html",
        controller: "AlbumCtrl"
      });

    $urlRouterProvider.otherwise("/");
});
```   

we have two states. one is index state which will list all albums of flickr user and album will list photos of each individual album. Now create a angular.value service for using Flickr API as follows : 
```js
.value("Flickr_data", {
    key: "bdbbafdc270d29a90c8dc33fac42e4a6",
    endpoint: "https://api.flickr.com/services/rest/",
    user_id: "13455964@N02"
});
```

Here is the detail of above service: 
  * key: it is your api key.
  * endpoint: it is the flickr API endpoint for accessing flickr services.
  * user_id: it is the user_id we are going to use for fetching photosets(photo albums).

In real world applications you might not hard-code the user_id in your applications. But rest of the idea will remain same. Now create a factory for fetching the photosets from Flickr for that particular User. 
```js
.factory("Flickr", function($http, $q, Flickr_data) {
    var result = {};
	result.getPhotoSets = function() {
		var url =
			Flickr_data.endpoint +
			"?method=flickr.photosets.getList&api_key=" +
			Flickr_data.key +
			"&user_id=" +
			Flickr_data.user_id +
			"&format=json&nojsoncallback=1";

		return $http.get(url);
	};
	return result;
});
```
In getPhotoSets function we are fetching the list of Photosets from Flickr and returning the promise by returning $http.get(url). Now open the index.html file and edit the body as follows : 
```html
<ion-nav-bar class="bar-positive">
	<ion-nav-back-button>
	</ion-nav-back-button>
</ion-nav-bar>
			
<ion-nav-view>
</ion-nav-view>
```

Now create a MainCtrl as follows : 

```js
.controller("MainCtrl", function($scope, $ionicLoading, $state, Flickr) {
    $ionicLoading.show();

    // Getting Photosets Detail from Flickr Service
    Flickr.getPhotoSets().then(function(result) {
      $scope.photoList = result.data.photosets.photoset;
      $ionicLoading.hide();
    });

    // Opening Album
    $scope.openAlbum = function(photoset_id) {
      $state.go("album", { id: photoset_id });
    };
})
```

Here we are assigning the result of Flickr.getPhotoSets call to $scope.photoList which will contain the list of User's PhotoSets(Photo Albums). $scope.openAlbum() function opens a album route with given photoset_id. Now we have to create a view for list of Photosets in 'templates/main.html'. Here is the code : 
```html
<ion-view view-title="Ionic Flickr">
<ion-content class="has-header">
	<ion-list>
		<ion-item class="item-icon-left item-text-wrap" collection-repeat="album in photoList" ng-click="openAlbum(album.id)">
			<i class="icon ion-record positive"></i>
			{{album.title['_content']}}
		</ion-item>
	</ion-list>
</ion-content>
</ion-view>
```
Save all the files and you will see app as follows : ![Flickr5]({{ "/img/posts/flickr5.png" | relative_url}}){: .img-fluid} 

Next we have to display the pictures in these albums. But the problem with Flickr API is it is not really a REST API. Lets say If we need multiple pictures sizes for a photo and its detail then we have to make two HTTP calls to get those information. That is why Flickr API is really mess. But since we have power of angularjs with us. Here is what we will do now. We are creating two function inside our 'Flickr' Factory as follows : 

```js
// Getting Photos of a photo set
result.getPhotos = function(photoset_id) {
	var defer = $q.defer();

	var url = Flickr_data.endpoint + "?method=flickr.photosets.getPhotos&api_key=" +
			  Flickr_data.key + 
			  "&user_id=" + Flickr_data.user_id + 
			  "&photoset_id=" + photoset_id + 
			  "&format=json&nojsoncallback=1";

	// Getting the Photos from a photoset
	return $http.get(url);
};

result.getInfo = function(id, secret) {
	sizes =
		Flickr_data.endpoint + "?method=flickr.photos.getSizes&api_key=" +
		Flickr_data.key + "&photo_id=" + id + 
		"&format=json&nojsoncallback=1";

	info =
		Flickr_data.endpoint + "?method=flickr.photos.getInfo&api_key=" +
		Flickr_data.key + "&photo_id=" + id +
		"&secret=" + secret + 
		"&format=json&nojsoncallback=1";

	return $q.all([$http.get(sizes), $http.get(info)]);
};
```   

In Flickr API each photo has a id and secret. getPhotos(photoset_id) function fetches id,secret and other information of photos in a photosets. getInfo(id,secret) function fetches information about a particular photo by giving its id and secret and also gives various size information about the pic, which we can display in App. Both function return promises. Now we have to create 'AlbumCtrl' for displaying each album. It will look like below : 

```js
  .controller("AlbumCtrl", function(
    $scope,
    $ionicLoading,
    $stateParams,
    Flickr
  ) {
    $ionicLoading.show();
    $scope.id = $stateParams.id;
    $scope.photoList = [];

    // Getting List of Photos from a Photoset
    Flickr.getPhotos($scope.id).then(function(result) {
      $ionicLoading.hide();
      console.log(result);
      $scope.photos = result.data.photoset.photo;
      $scope.title = result.data.photoset.title;

      angular.forEach($scope.photos, function(photo, key) {
        var id = photo.id;
        var secret = photo.secret;
        Flickr.getInfo(id, secret).then(function(result) {
          $scope.photoList.push({
            sizes: result[0].data,
            info: result[1].data
          });
          console.log($scope.photoList);
        });
      });
    });
});
```
    

We also have to create a view for album state in 'templates/album.html' file as follows :
```html
<ion-view view-title="Album">
	<ion-content>
    	<div class="list card" ng-repeat="photo in photoList">
    	  <div class="item item-text-wrap">
    	    <h2>{{photo.info.photo.title['_content']}}</h2>
    	  </div>

    	  <div class="item item-image" style="min-height:100px">
    	    <img alt="{{photo.info.photo.title['_content']}}"zoom-view ng-src="{{photo.sizes.sizes.size[5].source}}">
    	  </div>

    	  <div class="item item-text-wrap" ng-show="photo.info.photo.description['_content']" >
    	    <p>
    	    	{{photo.info.photo.description['_content']}}
    	    </p>
    	  </div>
    	</div>
    </ion-content>
</ion-view>
```

That is it. Output will look like below :

![Flickr6]({{ "/img/posts/flickr6.png" | relative_url}}){: .img-fluid} 