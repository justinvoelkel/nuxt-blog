---
title: App Development - Getting started with PhoneGap
date: 2013-11-28T03:39:36.000Z
description: App Development - Getting started with PhoneGap
---

My company has been looking to release an iphone app for over a year now. Now that our website redesign and rebuild is completed the app is next on the list. Since I’m not a dedicated app developer objective C wasn’t exactly an option. In my search for options I came across [PhoneGap](http://phonegap.com/ "PhoneGap"). It’s a great framework that allows you to build an with HTML/CSS/JS. Considering our needs were minimal as far as functionality this would work extremely well. Getting up and running was a bit confusing, so I wanted to put together a ‘zero to app’ list that should get you up and running in 10-20 minutes assuming you’re starting fresh.

<div class="alert alert-info"><span class="alert-before"></span><span class="alert-after"></span><div class="alert-wrapper">**Heads up!** This is for version 3.1 as of November 2013 and is intended for development of and iOS app on a Mac  
<span class="ico-st alert-close"></span></div><div class="clear"></div></div>
## Install node.js

I prefer to do things through the CLI options but node can be downloaded and installed from [nodejs.org](http://nodejs.org "Node Js"). The CLI requires you have [homebrew](http://brew.sh/ "Home brew") installed. Once it is –

    brew install node

Super easy right?


## Install PhoneGap

Now, to actually install PhoneGap from your terminal –

    npm install -g phonegap


## Install Cordova

You’ll also need apache cordova moving forward –

    npm install cordova

You should have all the necessary components to create an iOS app in xCode and run it in the simulator.


## Create a Project

If you don’t already have it, you’ll need [xCode](https://developer.apple.com/xcode/ "apple xcode") and the [iOS SDK](https://developer.apple.com/devcenter/ios/index.action#downloads "iOS SDK"). Assuming you have said items you can create a new PhoneGap app easily. Again, in your terminal –

<code>
$ cd path/to/dev/folder/you/want 
$ cordova create appName com.reversedomain.appName "Hello World" 
$ cd appName 
$ cordova platform add ios 
$ cordova prepare
</code>

You should be all set now!


## Open Your Project in xCode

Just navigate to your ‘appName’ folder that was just created in the finder. From there your going to platforms > ios and simply just opening your appName.xcodeproj. You can hit the build button (play button – top left) and see the default Hello World app.

Hopefully this helps someone just getting started. It’s a condensed version of the time I spent figuring all this stuff out. PhoneGap is super useful but the documentation can be pretty outdated/confusing. Happy coding!


