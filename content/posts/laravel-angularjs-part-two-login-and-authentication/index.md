---
title: Laravel and AngularJS - Part Two – Login and Authentication
date: 2014-07-17T23:19:45.000Z
description: Laravel and AngularJS - Part Two – Login and Authentication
---

Well it’s been a while* again*; the past few months have been an absolute whirlwind of busy but I’ve finally mapped out time to continue with this series. OK – so, time to keep rollin’ on[ Part One – Prep Your App](http://justinvoelkel.me/laravel-and-angularjs-part-one-prep-your-app/ 'Laravel and AngularJS: Part One – Prep Your App'). In case you haven’t read that post we’ve only gotten as far as setting up a fresh Laravel install with a migration and seed, as well as adding AngularJS to the project. Not impressed? I can’t blame you – let’s dig deeper into setting up our Laravel API and Angular login for the blog application.

In part one we had setup two templates inside of the **App > Views** directory. One of those was **index.blade.php** – don’t worry about that one for now – our blog views are going to live there so we can save that for some time later. We’re going to first focus on **admin.php **because this is where our administrative dashboard is going to live. This is the area we’re looking to lock down unless your an authenticated user. To start out lets go in and remove our placeholder **h1** tag – it’s purpose hath’ been served. In it’s place drop in this snippet –

```language-markup
    <body ng-app="blogApp">
        <div id="wrapper">
            <div class="container" id="view" ng-view>
            </div>
        </div>
```

Make sure you don’t miss that **body **tag, it’s pretty important. You’ll notice the **ng-app** directive included in the open **body** tag – this is how Angular identifies the [root scope](https://docs.angularjs.org/api/ng/service/$rootScope 'AngularJS root scope'). Root scope is sort of the skeleton key to your application – all other scopes are descendants of your root scope. You’ll see later this is referenced in **Public > js > app.js** which we created in part one. Also, kindly notice the **ng-view **directive on the **view** div. This is going to be the main injection point for our angular views. So, if you’ve worked with Laravel before you can this of this as the** master template**. Our login view, post editor, post list, everything — it’s just going to be injected right through that div. Super cool, right? Ok, lets move on – make sure that’s saved. Let’s make our login form. Go to **Public > js > templates > login.html** (created in part one) and open it up. There should be a div in there already but if there isn’t we’re using bootstrap to make things a bit more organized so you’ll want to start with –

```language-markup
    <div id="login" class="col-md-4 col-md-offset-4">
        <div id="login-form">
        <h4>Login</h4>
        <form>
            <div class="form-group">
                <input id="username" type="text" /></div> <!--password-->
                <div class="form-group">
                <input id="password" type="password" /></div> <!--submit-->
            </div>
            <div class="form-group">
                <button class="btn btn-primary"type="submit">Login</button>
            </div>
        </form>
        </div>
    </div>
```

That just gives us a centered box inside of our template with the login form (note there’s some css going on here but I’ll leave you to look at that as this isn’t a post on css). You’ll notice more Angular directives on our form fields as well. If you’re wondering about how we were going to access the values in our form – those are how.

To give you a brief overview (we’ll be touching on this again in a few minutes) ng-model is what allows us to access the information in the scope. It turns ‘loginData’ into an object; if you’ve worked with javascript objects before you’ll recognize the dot notation. So, when we go and setup a controller, we’ll be able to work directly with that loginData object for the username and password. As of right now, we have our master template, **admin.php**, setup to inject our angular views (for the admin dashboard) such as the one above but if you load /admin nothing is going to show up. That’s because we’ve not yet setup any routes for angular to deal with. Let’s setup our first route now – open up **public > js > app.js** and you can find the following –

```language-javascript
var app = angular.module('blogApp',['ngRoute']);
app.run(function(){ });  //This will handle all of our routing
app.config(function($routeProvider, $locationProvider){ });
```

As I eluded to earlier, there is our reference to blogApp – our application name. This is how the whole application is bootstrapped through angular. If you misspell or name that module differently nothing is going to load inside of your master template. To give you a brief explanation of what’s going on in **app.js** – we’re basically bootstrapping Angular and defining the app’s dependencies. In this case we’ve only named ngRoute as a dependency. That dependency of ngRoute allows us to route url’s through Angular and it is not included with Angular core. Don’t worry about **app.run** just this minute – we just need to focus on creating a route in **app.config**. The updated **app.config** is below – take a look:

```language-javascript
app.config(function($routeProvider, $locationProvider){
   $routeProvider.when('/',{ templateUrl:'js/templates/login.html', controller:'LoginController'});
});
```

Ok, you can kind of get a feel for how easy it is to route things with angular. We’re taking our injected dependency of routeprovider and calling it’s method ‘when’ to match a case for a requested URI. This can be a little tricky to interpret off the bat – the route that we’re matching is the root ‘/’ route but the URI in relation to the domain is actually something like laravelandangular.com/admin. Why is it setup like this and why does it work? Well, it’s because we’re utilizing two routing systems between Laravel and Angular. In our **app > routes.php** file you’ll see the route to /admin that builds the admin.php view, which in turn invokes angular – so, to angular that **_is_** the ‘root’. You’ll also notice I’ve thrown a controller into the mix there – if you save and run anything at this point it’s not going to work because that controller is non-existant. Let’s set it up. In the interest of keeping ourselves clean and sane go ahead and setup a folder just for controllers at **public > js**. We’ll end up having one folder for our templates, controllers, and services (if you want to create one for services now knock yourself out). Inside of the newly created controllers directory create a new js file- **loginController.js**. Just like any javascript file -ever- it needs to be included as a script. Head back to **App > views > admin.php** and include it as you would any other javascript. It’s important to note here that the path to *anything* in the public folder is relative to the root dir. So, even though our Laravel template is under **App > views** the path the the javascript is **\*still\*\*\*** **going to be **js/controllers/loginController.js\*\*.

```language-javascript
<script src="js/controllers/loginController.js"></script>
```

Since this is independent of **app.js** (it could’ve been built directly into app.js but it’s good practice to separate these pieces for modularity and sanity) it needs to be injected as a dependency. That’s actually really easy, we already have **ngRoute** injected as a dependency so just follow the same formula. I named my module **LoginCtrl** but you can name it whatever as long as it’s descriptive and you can remember it when we’re setting up the controller in a minute. **App.js **should now look similar to this:

```language-javascript
var app = angular.module('blogApp',[ 'ngRoute', //Login 'LoginCtrl' ]);

app.run(function(){ }); //This will handle all of our routing

app.config(function($routeProvider, $locationProvider){
    $routeProvider.when('/',{
      templateUrl:'js/templates/login.html',
      controller:'LoginController'
    });
});
```

**Notice this:** I bumped the dependency injections to a new line just for cleanliness and so I could throw in a comment but it’s not necessary. With the controller file included and injected as a dependency we need to actually set it up for any of this jazz to work. Open up Public > js > controllers > loginController.js. I’ll post what mine looks like and explain after:

```language-javascript
var login = angular.module('LoginCtrl',[]);
    login.controller('LoginController',function($scope){
      $scope.loginSubmit = function(){
        console.dir($scope.loginData);
    }
});
```

As I said, I injected the controller as **LoginCtrl **in **app.js** so that’s the module name here – if you named it differently change it appropriately. From there we define a new controller named **LoginController **and inject **$scope**. If you don’t inject scope you’re not going to be able to access our scope data (loginData). In our template **login.html** we set the ngSubmit directive on our form to a function called – can you guess? – yep – **loginSubmit** and this is where that function lives. If everything is saved and updated you should be able to fire the URI /admin and see the form – if you add a test username and pass you’ll be able to see that object in your dev tools console. [![login screen shot](http://justinvoelkel.me/content/images/2014/07/login-console-dir-e1405606318413-1200x605.png)](http://justinvoelkel.me/content/images/2014/07/login-console-dir.png)

## Services

Now that we have a simple controller that captures the login information we need a way of sharing that information with Laravel to verify the login. We’ll be creating an auth service to do this – essentially it’s going to communicate the login credentials to Laravel over our API, authenticate the user, and send back the appropriate response.

If you haven’t already, create a new folder in **Public > js ** called **‘services’**. Again, we’re doing this to keep things nice and modular and clean. Inside of **services **create a new js file called **authService.js**. Now, full disclosure – I’m going to be lazy and just set a session key/val pair to successfully authenticate a user. This isn’t super secure (it can be hi-jacked fairly easily) so I’d suggest either using c[ookies or tokens to lock this down](https://auth0.com/blog/2014/01/07/angularjs-authentication-with-cookies-vs-token/ 'Angular Cookies vs Tokens') in a production environment.

We’re going to just add some basic boilerplate type stuff in here for now. You can add the following and I’ll explain later when we come back to it:

```language-javascript
var login = angular.module('AuthSrvc',[]);
login.factory('Login',function($http,$rootscope,SessionService){ });
```

And, just like our controller we have to add the script source to **App > views > admin.php** as well as **Public > js > app.js** so that it’s properly bootstrapped by the application. It’s the same process as before, just make sure you have the correct path in **admin.php **and make sure you have the correct name (in my case **AuthSrvc**) injected into **app.js**.

```language-javascript
var app = angular.module('blogApp',[
  'ngRoute',
  //Login
  'LoginCtrl',
  //AuthService
  'AuthSrvc'
]);
```

## Laravel API

Before we fiddle with **authService.js** we need to actually setup our Laravel API so we have something to call out to from our auth service. Go ahead and open up your routes at **App > routes.php**. What we’re going to do is add a prefixed [route group](http://laravel.com/docs/routing#route-groups 'laravel route group') to segregate our API calls from any run of the mill Laravel routes. That just means that any http calls made by a CRUD service or the auth service will made to a URI that will look like *‘api/login/auth’*. Again, it’s for cleanliness and sanity. Add this above any existing routes:

```language-php
Route::group(array('prefix'=>'/api'),function(){ });
```

That creates our prefixed group. Now, we can drop in regular routes that will all assume an *‘api/’* prefix. They work just like every other route that routes to a closure or a controller. I’m going to be modular and route to a controller for our auth. To do that let’s add a new Laravel controller at **App > controllers ** and call it **AuthController.php**. You can add in the following – explanation to follow:

```language-php
class AuthController extends BaseController{
  public function Login(){
    if(Auth::attempt(Input::only('username','password'))){
      return Auth::user();
    } else {
      return 'invalid username/pass combo';
    }
  }
  public Function Logout(){
    Auth::logout(); return 'logged out';
  }
}
```

#### Note

I just wanted to note something important and really awesome about Laravel here. In the sample code above you’ll notice I’m just returning ‘Auth::user()’. Laravel will automatically cast that to JSON for you – no need to do anything else. That’s particularly helpful when you’re handing that data back to a JS front end like angular or ember.

You can see we’ve added a method for logging in and logging out. If you’ve done some basic user authentication with Laravel this probably looks standard. We’re making use of Laravel’s auth attempt method but we’re getting our username and pass from posted inputs. Now we just need to add the appropriate routes to our **App > routes.php** to access these methods. The updated route group is below – we’re posting for login and getting for logout:

```language-php
Route::group(array('prefix'=>'/api'),function(){

  Route::post('login/auth','AuthController@Login');
  Route::get('login/destroy','AuthController@Logout');

});
```

Now, we’ll be able to send an http post request with credentials from angular to either **_‘api/login/auth’_** to login or **_‘api/login/destory’_** to logout. With this good to go we can jump back into our **AuthService.js** file and start adding in some functional code to interact with the API.

## Initiating API Calls from Angular to Laravel

We need to update two files straight away. To make the call to the API we need to add an http request to our **authService.js** file. Our ‘Login’ factory is now going to look like this:

```language-javascript
login.factory('Login',function($http){
    return{
      auth:function(credentials){
         var authUser = $http({method:'POST',url:'api/login/auth',params:credentials});
        return authUser;
    }
}});
```

Notice we’re making the http post request to our prefixed Laravel URL *‘api/login/auth’* and sending credentials as a parameter. those credentials are going to come from our $scope.loginData that is being submitted from the login form. That is all located in **loginController.js**. We need to replace our console test with a call to our authService and a subsequent promise to catch the response. See below for the updated login controller:

```language-javascript
login.controller('LoginController',function($scope,Login){
  $scope.loginSubmit = function(){
    var auth = Login.auth($scope.loginData);
    auth.success(function(response){
      console.log(response);
    });
}});
```

You’ll see I’ve injected our service ‘Login’ which allows me to call to auth and send it the entered login data ($scope.loginData). The variable auth is a promise object and [it requires a method to be called to extract that data](http://stackoverflow.com/questions/16385278/angular-httppromise-difference-between-success-error-methods-and-thens-a 'AngularJS promises'). I am using success here because it’s just a bit more convenient but you can bundle success/error with .then instead. So, right now I am just logging the API’s response – assuming everything is setup correctly you should see something like the following:

[![Login console response](http://justinvoelkel.me/content/images/2014/07/login-console-after-e1405619803342-1200x608.png)](http://justinvoelkel.me/content/images/2014/07/login-console-after-e1405619803342.png)

With any luck you’re able to see the console responses. The first came from entering an incorrect combination of test/test and the second came from entering the correct user and pass for the user we seeded in part one admin/password. It’s worked as expected and returned the user data from our database. We’ve made our first successful call to our Laravel API!!!

Now that we’ve got an authenticated user we can set a session variable to indicate that this user is verified authenticated and is free to move about the application. To do this we’ll add another factory to our **authService.js** file and make use of the browsers session storage. The following is the new factory added to the **authServices.js **file:

```language-javascript
login.factory('SessionService',function(){
return{
  get:function(key){
    return sessionStorage.getItem(key);
  },
  set:function(key,val){
    return sessionStorage.setItem(key,val);
  },
  unset:function(key){ return sessionStorage.removeItem(key);
  }
}});
```

Now, we can inject **SessionService** into our **Public > js > controllers > loginController.js** file. Now we can set a session variable that says the user has been authenticated.

```language-javascript
login.controller('LoginController',function($scope,$location,Login,SessionService){
$scope.loginSubmit = function(){
  var auth = Login.auth($scope.loginData);
  auth.success(function(response){
    if(response.id){
    SessionService.set('auth',true);
  } else {
    alert('could not verify your login');
  });
}});
```

In the above I just replaced the console dump of our response to a conditional to check if the response contains a user ID – if it does that means we had a successful authentication – if not just alert that we couldn’t verify. I know it’s not super secure but this is only for demonstration purposes. With the correct credentials entered you’ll be able to see the auth session variable set in your browser now:

[![Angular Session Auth Set](http://justinvoelkel.me/content/images/2014/07/session-auth-set-e1405621896316-1200x647.png)](http://justinvoelkel.me/content/images/2014/07/session-auth-set-e1405621896316.png)

## What’s Next?

Essentially that covers the user authentication process. Moving forward we’ll be able to check for that *auth* session variable to validate the user on all the *protected* pages (i.e anything under dashboard). In the next post I’m going to cover creating the dashboard, redirecting authenticated users to that dashboard, logging users out, and setting up a CRUD service for our blog posts. I’m hoping to have that up in a few weeks but at this point I can’t promise anything – work has been insane and I’m in the middle of planning a wedding for pete’s sake! I hope you all enjoyed part two though, and if there are any improvements I can make to any of this please feel free to leave them in the comments!
