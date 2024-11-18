---
title: Laravel and AngularJS - Part Three - The Dashboard, Seeding, and CRUD
date: 2014-11-22T02:06:17.000Z
description: Laravel and AngularJS - Part Three - The Dashboard, Seeding, and CRUD
---

<div class="alert alert-notice"><span class="alert-before"></span><span class="alert-after"></span><div class="alert-wrapper">**Notice this:** I was trying to get the CRUD service lumped in with the remaining post functionality but it was getting close to 4k+ words long so I am breaking them up. Part four is already done and in the queue to be released next week and will cover listing, creating, and editing posts.  
<span class="ico-st alert-close"></span></div><div class="clear"></div></div>In my [previous post in this series](http://justinvoelkel.me/laravel-angularjs-part-two-login-and-authentication/ "Laravel and AngularJS: Part Two – Login and Authentication") I showed how you can create a simple login with Laravel and Angular. That meant we got to use our Laravel API for the very first time to verify a users credentials. Moving on to the next part of this series we’ll be using that API quite a bit more often. I’m going to setup a service in Angular to do all of our CRUD operations for posts. OK – lets jump in!

Last time we setup the login but it didn’t do anything but verify the user and set a browser session variable. Let’s start off by updating the **loginController.js **file to redirect after our login is verified. Open up the file at **Public > js > controllers > loginController.js **and we’ll add **\*$location.path(/dashboard)\*\*\*** \*\*to our if statement after we set the browser session variable – here’s the updated controller:

login.controller('LoginController',function($scope,$location,Login,SessionService){ $scope.loginSubmit = function(){ var auth = Login.auth($scope.loginData); auth.success(function(response){ if(response.id){ SessionService.set('auth',true); //This sets our session key/val pair as authenticated $location.path('/dashboard'); }else alert('could not verify your login'); }); } });

I’m using [Angular’s location service ($location)](https://docs.angularjs.org/api/ng/service/$location 'AngularJS location service') to redirect the user to the dashboard much the way you would use window.location in normal javascript. Ok, so if you try everything again you’ll get forwarded to the */dashboard *URI but it’s totally blank. I’m sure you’ve guessed by now the reason is because it’s pointing to nothing. Let’s go ahead and setup a template and a route for the dashboard. This is going inside our template folder so head to **Public > js > templates **and create a new file called **dashboard.html**. You can put in some boilerplate stuff if you want but we’ll be coming back to this later. Now that we’ve created the template we can setup a route so that we can use it. Again, this is done from **Public > js > app.js**. We’ll add a really simple route to our template file below the existing route:

$routeProvider.when('/dashboard',{ templateUrl:'js/templates/dashboard.html' });

If everything went to plan you should be able to see whatever you put into **dashboard.html**.

<div class="wp-caption aligncenter" id="attachment_422" style="width: 310px">[![Angular dashboard setup](http://justinvoelkel.me/wp-content/uploads/2014/07/after-dashboard-setup-e1405720121631-300x191.png)](http://justinvoelkel.me/wp-content/uploads/2014/07/after-dashboard-setup-e1405720121631.png)TA DAAAAA!!1!!one!

</div>Alright, I know it’s not so impressive yet – but just you wait. Since this dashboard is going to list all of our posts we’re going to need a controller. If you’ve not recognized the pattern yet head to **Public > js > ****controllers** and we’ll create a new controller. However, we’re not going to be creating a ‘dashboard’ specific controller. As I said this dashboard is going to just list our existing posts so we’re going to be creating a controller for the ‘thing’ that we need – in this case posts. So create a file called **postController.js** and open it up. I’m adding the most basic of basic at this point:

var post = angular.module('PostCtrl',[]); post.controller('PostController',function($scope){ });

Now, we need to rinse and repeat our process to add a new controller – do you remember? Of course you do – you’re clearly a smart and incredibly good looking person if you’re reading this. But, in case it’s slipped your mind be sure to add the **postController.js** script to the **admin.php** view in **App > views** and be sure you add it to our list of dependencies annnnd define it as the controller for our dashboard route in **app.js**:

var app = angular.module('blogApp',[ 'ngRoute', //Login 'LoginCtrl', //Posts 'PostCtrl', //AuthService 'AuthSrvc' ]); app.run(function(){ }); //This will handle all of our routing app.config(function($routeProvider, $locationProvider){ $routeProvider.when('/',{ templateUrl:'js/templates/login.html', controller:'LoginController' }); $routeProvider.when('/dashboard',{ templateUrl:'js/templates/dashboard.html', controller:'PostController' }); });

At this point the post controller should be all wired up and you shouldn’t get any kind of errors from angular. Next, we need to take a quick step back and do a tiny bit more setup. Currently, we have users table and that’s it – we need somewhere to store all of our posts. So, we need to create another laravel migration and seed to create that table and seed it with an example post. To do so we’ll need to jump into the CLI (either git bash or if you’re running osx or linux terminal) and run something similar to this:

$php artisan migrate:make create_posts_table --table="posts" --create

Once the new migration is created we need to set it up with the proper column names and types and seed it with an example post. Open **app > database > migrations > [your posts migration]**. My setup looks like this:

/** _ Run the migrations. _ _ @return void _/ public function up() { Schema::create('posts', function(Blueprint $table) { $table->increments('id'); $table->string('title'); $table->longText('content'); $table->boolean('live')->default(0); $table->timestamps();//creates timestamps for created and udpated at }); } /** _ Reverse the migrations. _ _ @return void _/ public function down() { Schema::drop('posts'); }

It’s a pretty basic setup. We’re adding a self incrementing post id, title, content, and a boolean to tell us if the post is live or in a draft state – we’ll default the draft to zero (draft). Next we need to seed the database with an example post. Head to **app > database > seeds** and create **PostTableSeeder.php**. The contents of mine:

class PostTableSeeder extends Seeder { /\*\* _ Run the database seeds. _ _ @return void _/ public function run() { DB::table('posts')->delete(); Post::create(array( 'title' => 'Example Post', 'content' => 'Hey this is just a test!' )); } }

And, just add it to the end of the seeder file **app > database > seeds > DatabaseSeeder.php**:

$this->call('PostTableSeeder');

Finally, we can create a (nearly) empty model for posts. Head over to **app > models** and create **Post.php**. The contents are simple:

class Post extends Eloquent { /\*\* _ The database table used by the model.CRUD Service _ _ @var string _/ protected $table = 'posts'; }

Now we can re-run our migration and seed with the new data. **_Note_** – this is going to drop the current users table and re-seed it; if you’ve changed anything from the original or added anything you’re going to lose it.

[![migrate and seed laravel](http://justinvoelkel.me/wp-content/uploads/2014/08/migrate-and-seed-posts-e1408047414743.png)](http://justinvoelkel.me/wp-content/uploads/2014/08/migrate-and-seed-posts-e1408047414743.png)

## CRUD Service

Super duper job so far _[insert your name, nickname, or something gnarly you’ve always wanted to be called]_ – give yourself a hearty pat on the back or a hand shake. Now that we’ve got some basics setup ( angular post controller, posts table, post info etc.) we need to setup a way of retrieving and manipulating that information. This is where our CRUD service comes into play. For those of you that don’t know CRUD is an acronym that stands for Create Read Update and Destroy. In other words, all the basic stuff we’ll be doing with the blog.

We’ll start out by creating a new file in **public > js > services ** called **crudService.js**. Let’s start with the basics (explanation to follow) –

var crud = angular.module('CRUDSrvc',[]); crud.factory("CRUD",function($http){ return{ all: function(){ //get all posts }, create: function(data){ //create a new post }, get: function(id){ //get a specific post }, update: function(id,data){ //update a specific post }, delete: function(id){ //delete a specific post } } });

So, you’ll notice we have all the basic CRUD functionality outlined here with the data we’ll need for our call to Laravel. I should note that for the sake of the tutorial I did use reserved words for the functions and that is not good practice but it helps better illustrate what’s going on. When you take this out in the wild be sure you’re using well thought out function names that aren’t making use of reserved words. Anyway, now that we’ve got the service set up we need to include the script on **app > views > \*\***admin.php \*\*so that it can be used. The services section should look like this now –

 <!--angular services--> <script src="js/services/authService.js"></script> <script src="js/services/crudService.js"></script>

Along with that inclusion we need to (again) add it to our list of dependencies in **app.js**. This should be old hat by now –

var app = angular.module('blogApp',[ 'ngRoute', //Login 'LoginCtrl', //Posts 'PostCtrl', //AuthService 'AuthSrvc', //CRUDService 'CRUDSrvc' ]);

With our dependency declared we’re all wired up and ready to go. There is one painfully obvious dismissal here – despite having a service that will create and manipulate our posts and data, we have no way of inputting or viewing our data yet! In part four we’re going to add some front end goodness so that we can actually list, create, and edit our posts from the dashboard.
