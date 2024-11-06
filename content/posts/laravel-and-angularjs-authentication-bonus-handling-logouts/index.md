---
title: Laravel and AngularJS - Authentication Bonus - Handling Logouts
date: 2015-03-11T20:15:21.000Z
description: Laravel and AngularJS - Authentication Bonus - Handling Logouts
---

![logout meme](http://justinvoelkel.me/wp-content/uploads/2015/03/logout-meme-300x300.jpg)I woke up today with a few comments on this series – thanks for reading! One of those comments came from Mei Gei asking how to handle logouts. It honestly did not even dawn on me that I had forgotten to include this in the initial series so I thought I’d put together a quick post to address it.


## Moar Buttonz

Yep, first thing is first….we need a new button. Open up <span class="highlight">public > js > templates > dashboard.html</span>. I’m going to show you what I’m adding in for the button and I’ll explain after. By the way, I am adding the logout button just after the h1 tag in the dashboard area and simply floating it right to make sure it’s out of the way. Ok, here we go:

<h1>The Most Dashing of Boards</h1> <div class="col-md-12"> <a class="pull-right btn btn-danger" ng-controller="LoginController" ng-click="logout()">Logout</a> </div>

Alright, so this is like every other button we’ve made with a few exceptions. You’ll notice we’re, again, using the ‘ng-click’ directive to fire the logout function when the button is clicked – fair enough. But, we don’t want to mix concerns and have that function reside with all of our posts logic. So, angular provides us a way of specifically saying this element should use controller ‘foo’. That is the [‘ng-controller’ directive](https://docs.angularjs.org/api/ng/directive/ngController "angular ng-controller directive"). Now, we can house our logout function inside of the correct controller. So, with that said let’s now open <span class="highlight">public > js >controllers > loginController.js</span>.

We need to add a the *logout* function that will be fired on our click. From there, well, I guess we need to logout right? Way back in [part two ](http://justinvoelkel.me/laravel-angularjs-part-two-login-and-authentication/ "Laravel and AngularJS: Part Two – Login and Authentication")we setup our authService.js. This was to handle all things authentication related — much like our CRUD service handles the manipulation of post data, the auth service handles user authentication actions. So, inside of this function we’re creating we want to make a call to that service, scrap the authorization inside of laravel, and send back a confirmation to angular. I’m getting ahead of myself a bit — let’s cover that logout function in the controller first. Add the following after ***loginSubmit***:

$scope.logout = function(){ var logout = Login.destroy(); logout.success(function(){ $location.path('/'); }); };

So, since we already have the ‘Login’ factory injected as a dependency in our Login Controller we can easily call out to a new function we’ll be adding in just a second — **destroy**. Again, we’re using a call and a promise to verify response. Once we know that call was a success we’re routing the user back to the login page — in this case, inside of angular, that is simply the root (/). Remember, angular is only aware of it’s existence under the hashbang (#) of your url — Laravel handles everything else. Alright, let’s open setup the destroy function in our auth service.


## Serving up Destruction

Open up <span class="highlight">public > js > services > authService.js</span>. We’ll be updating our **‘Login’ **factory. You’ll see, currently, we have an *auth* function — this is where we’ll be adding our destroy function. Also, we’ll need to inject our SessionService (the factory below Login) as a dependency. Why? Well, after we make our call to Laravel and destroy the user’s authorization session we need to unset our session variable ‘auth’ so that angular knows the user is no longer authenticated. Check it out:

login.factory('Login',function($http,SessionService){ return{ auth:function(credentials){ var authUser = $http({method:'POST',url:'api/login/auth',params:credentials}); return authUser; }, destroy:function(){ var logoutUser = $http.get('api/login/destroy'); logoutUser.success(function(){ SessionService.unset('auth'); }); return logoutUser; } } });

So this just makes the http ‘get’ call to the appropriate api route within laravel, on success (laravel has already logged the user out) we simply remove the ‘auth’ key in our session storage. Believe it or not, we actually have the logout api route set up in our Laravel routes.php file — we just never used it. You can verify by opening up <span class="highlight">app > routes.php</span> and checking for ‘login/destroy’ registered with the get http verb. And, the accompanying controller, <span class="highlight"> app > controllers > AuthController.php</span> should have a Logout method already included from part two. Finally, we just return the success of this logout to our original call from loginController.js — which will then fire the redirect — $location.path(‘/’) — to send the user back to the login page.


## Wrapping Up

So, we’ve basically fed our logout request through several layers. We use the logout button to fire a function inside of our, appropriate, login controller. That function then makes the appropriate request to Laravel through our auth service. And, after we’ve verified Laravel has unauthenticated the user, on the way back to our login controller, we remove the users auth state that is set in our session storage. Finally, when we’ve come full circle back to our original logout call — we can be sure we’ve logged the user out so we just redirect them back to the login page.

Now, I noticed when I went through all of this that I didn’t setup any auth check for protecting routes in angular. So, right now the login looks good but the dashboard is still publicly available. I did mean to do this at some point so some time before the end of the week I’m going to put up another post on making sure a user is logged in when they request a protected route, else, they should be prompted to login. Ok, that’s all for now!


