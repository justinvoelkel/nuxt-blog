---
title: Laravel and AngularJS - Bonus Coverage - Restricting Routes in Angular
date: 2015-03-14T00:29:49.000Z
description: Laravel and AngularJS - Bonus Coverage - Restricting Routes in Angular
---

![security](http://justinvoelkel.me/wp-content/uploads/2015/03/Secure-300x225.jpg)In my last bonus post I realized that, despite having authentication setup in both Laravel and Angular, I hadn’t restricted any routes inside of angular — meaning, while the login looks good, all of the admin area that should be protected is actually available to the public. Let’s take care of that.

## Paths, They Are Changin’

So, what we need to do is verify whether a user is logged in or not when a specific page is requested. Now, this can change depending on how you’ve implemented your authentication. For this simple app I’ve chosen to just set a key in session storage identifying the user as logged in. If you want to review go ahead and re-read [part two](http://justinvoelkel.me/laravel-angularjs-part-two-login-and-authentication/ 'Laravel and AngularJS: Part Two – Login and Authentication'). Anyways, what we’re aiming for is – when a user requests a new route we need to see if they’re authenticated for that route. Lucky for us there is a simple way of registering a route change inside of angular. See, when angular receives a request to change routes it broadcasts that change so that it can begin building the dependencies of that new ‘page’. We can intercept that broadcast as a signal of a route change and check that the user is still logged in.

Let’s start by opening <span class="highlight">public > js > app.js</span>. We’re going to be accessing the [rootscope ](https://docs.angularjs.org/api/ng/type/$rootScope.Scope 'angular rootscope documentation')to listen for our broadcast event. If you open up the angular docs for [rootscope ](https://docs.angularjs.org/api/ng/type/$rootScope.Scope 'angular rootscope documentation')you’ll see, in the life cycle section, **$on** is listed as a listener for ‘events of a given type’. Looks promising — after all, we’re listening for a very specific event, right? OK enough foreshadowing — inside of **_app.js_** you should have _app.run_ in your source. We’ll be putting our listener right here so that it’s invoked at run time — _run_ blocks are essentially what you could think as angular’s main function. We’ll just dump the event to the console to verify everything is working:

app.run(function($rootScope){ $rootScope.$on('$routeChangeStart', function(event, next, current) { console.dir(event); }); });

Now, if you reload, you should see something similar to this in your console:

![route change start angular](http://justinvoelkel.me/wp-content/uploads/2015/03/routechangestrt-angular.jpg)

We don’t need to be particularly concerned with any of this information, but it’s proof that our listener is firing on a new request. Alright, so lets build on that. We’re going to start by making an array of **whitelist** routes. We’re whitelisting because the majority of our routes are assumed to be protected — so it’s easier to just say _‘hey, here are the accepted no auth routes’_.  We’ll add method of checking login status, and check to see if the current route is whitelisted. Check out the updates below — I’ll explain further in a sec:

app.run(function($rootScope,$location,Login){ $rootScope.$on('$routeChangeStart', function(event, next, current) { var whiteList = ['/']; //the login is the only unguarded route - everything else needs to check session auth var loggedIn = Login.checkLoginStatus();//boolean - if user is logged in var routeSafe = !$.inArray($location.path(),whiteList);//boolean - is route safe or protected }); });

Ok so a few things. Starting from the top — we’re injecting [$location](https://docs.angularjs.org/api/ng/service/$location 'angular location') and our Login service (more on that in a minute). Next we have our whitelist array — we’re only whitelisting the route which equates to our login page. Next, a check for the user login status — this is why we injected our Login service. Again, in the interest of separation of concerns we’re adding this check to our existing login service along with the existing auth and destroy functions. Finally, we’re setting a boolean after we check to see if the path is in the white list. Most of this is inconsequential so far but it sets the table for what we’re doing. The crux of what we’re doing relies on the ‘checkLoginStatus’ function in our Login service so let’s set that up.

## Credentials Plz

Checking the login status is remarkably easy — in this app. Again, as I mentioned previously, this method of using session storage is a pretty primitive and certainly insecure way of authentication (because it can be easily spoofed) but it suits a beginner application. Ok, reiteration over – open up <span class="highlight">public > js >services > authService.js</span> and we’re adding our function below the existing **_destroy _**function we used for the logout. Ok, check it out:

checkLoginStatus:function() { return SessionService.get('auth') ? true : false; }

Make note here, that you’ll need to add a comma after the end curly bracket of **_destroy_**. Alright, so this is simple right? I’m just using a ternary statement to check if the session storage has auth set — since it’s either true or empty we don’t really have a need to check the contents. The statement will return the boolean we’re looking for back in **app.js**.

## Lock It up

Great, so now that we’ve got all of our necessary variables set we really just need a conditional to check the various states required for the route to load. We just add the following below our variable definitions:

if(!loggedIn && !routeSafe) { $location.path('/'); alert('You must be logged in to view this page!'); }

So, here we just check our two booleans — if the user is not logged in _and_ the route requested is protected then we just redirect right back to the login screen and alert to let the user know why they’re not allowed in. And that’s really it. Now, if you logout of your application you can try to reach the dashboard at ‘/dashboard’ and you should be greeted with our alert.

## [![angular restricted route](http://justinvoelkel.me/wp-content/uploads/2015/03/angular-app-restricted-300x141.jpg)](http://justinvoelkel.me/wp-content/uploads/2015/03/angular-app-restricted.jpg)Conclusion

So, now you should have your logout capabilities and your route restrictions in place. You can expand on the security of this process by looking into using the rootscope to store the login status. That’ll make it harder to bypass than the session storage. In terms of being able to visualize and easily check login status this works just fine. Ok, now that these are wrapped up I’m hoping to have the final segment — part six — up next week! Check out my [github repo](https://github.com/justinvoelkel/laravelandangular 'laravel and angular ') as well — I’ve updated the source to use current stable versions of angular, jquery, and bootstrap.
