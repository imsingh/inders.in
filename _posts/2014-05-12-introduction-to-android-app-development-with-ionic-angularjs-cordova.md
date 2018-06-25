---
layout: post
title: Introduction to Android App Development with Ionic + AngularJS + Cordova
category: blog
comments: true
---

In this article, I am gonna discuss how to set the environment for Android App Development with **[Ionic](http://ionicframework.com/)**, **[AngularJS](http://angularjs.org/)** and **[Cordova](http://cordova.apache.org/)**. It is good to mention that [Ionic](http://ionicframework.com) is a CSS + Javascript Framework for building Mobile Applications. Ionic is built on the Top of [AngularJS](http://angularjs.org) , a Javascript MVC Framework for building Web Applications backed by Google. Apache Cordova is a bridge between your web application and native components of Mobile App. In other words, you can access the **Camera**,**Bluetooth**,**Wifi** etc. of a Mobile with the Cordova. Cordova not only Supports **Android,** but **iOS**,**windows mobile**, **blackberry** and other Mobile OS also. When I started Android App Development with these tools, there were not really good guides for beginners. This article is not for programming help. It is merely a getting started guide.

## Installing and Setting Up Java.

  1. You must have Java installed on your System in order to run Cordova. You need JDK from Oracle. Download [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) . Select the Appropriate Version for your System.
  2. You we need to set the JAVA_HOME and Path Variable for JDK to work.
  * Copy the path of directory where JDK is installed. In my Case, it is : **C:\Program Files\Java\jdk1.7.0_05** . It is the default location, where java is installed (for Windows).
  * Now Right click **My Computer** and select **Properties**. Click on **Advanced System Settings** on Left of Window. Now System Properties Window will open ( Windows 7/8 ).
  * On **Advanced** Tab Click on **Environment Variables** Near to Bottom of Window.
  * In System Variables Click on **New**.
  * Enter the Name : **JAVA_HOME** and Variable Value to the path of jdk. In my case it is **C:\Program Files\Java\jdk1.7.0_05**
  * Now you also have to set the Path Variable by inserting  JAVA_HOME in it. Example : **;%JAVA_HOME%\bin;** in Path Variable.

## Setting Android SDK

  * Now Download the Latest Version of Android SDK. Go to <https://developer.android.com/sdk/index.html> .
  * After Downloading the Zip File, Unpack it into any Folder of your choice.
  * Open the **sdk** Folder and run **SDK Manager** and Download the API for your choice(for Different Android Versions) and download the **Google USB Drivers** also.
  * You will find two folders named **tools** and **platform-tools** in **sdk** directory. Now Again insert the location of both of these to path variables. In my case : **;C:\android\sdk\tools; **and** ;C:\android\sdk\platform-tools;**

## Install the Apache Ant.

  * Download the latest version and Set the **ANT_HOME** with the directory of your ANT installation and in Path Variable add **;%ANT_HOME%\bin;**

## Installing Cordova and Ionic

  * You first need the Node.js. Download the latest version of Node.js from **[nodejs.org](http://nodejs.org) **and install it.
  * Now open the nodejs command prompt and type **npm install -g cordova ionic **. This will install cordova and ionic in your system.

## Example App.

  1. Open Node.js Command Prompt and go to the directory where you want to create your app.
  2. type **ionic start myapp blank **. This will create a blank app with name **myapp. **Check the other options at <http://ionicframework.com/getting-started/>
  3. Now go to directory **myapp. **Example : **cd myapp .**
  4. type **ionic platform add android**
  5. type **ionic build android ** for building android version.
  6. type **ionic emulate **for emulating it on the emulator.
  7. You can also test the app in web browser by typing **ionic serve **but remember you will not able to use device features like compass etc.