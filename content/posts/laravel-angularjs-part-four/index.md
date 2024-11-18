---
title: Laravel and AngularJS - Part Four - The Dashboard Part Deux and a Posts Party
date: 2014-12-06T03:34:19.000Z
description: Laravel and AngularJS - Part Four - The Dashboard Part Deux and a Posts Party
---

In the [previous part of this series](http://justinvoelkel.me/laravel-and-angularjs-part-three/ 'Laravel and AngularJS: Part Three – The Dashboard, Seeding, and CRUD') I showed you how to seed the posts table and setup the CRUD service within angular. That service is clearly only one half of the puzzle, we need some help from laravel to get things flowing to and from the database and we also need some front end loving for our sparse dashboard area..

## Setting Up the Dashboard

Right now we have a whole lot of nothing on our dashboard – now that we’ve wired up angular with a controller capable of accepting some data and doing some crud (yep, it’s a pun) that needs to change. First thing is first – open up **public > js > templates > dashboard.html**. We’re going to start really simple and just add a button to add a new post and a table to list all of our posts. By the way, I’m going to refrain from posting too many code updates along the way here, it’s simple html and it’s just easier to post it all at the end. So, I’m going to add a button **Add a New Post** that will link the the /add url. Note that to get internal linking like this to work with the hashbang *(url/admin**#**/add – that hashtag is called a hashbang) *you’ll need to set your links up in the following format:

<a href="#/add">Create New Post</a>

You just need to make sure you have the hash at the beginning or else your internal links wont be working. If you click on that link right now it’s going to take you to a deliciously minimal white screen – aka it doesn’t work. That’s because we haven’t wired that route up in angular yet. The first step is to create a template to display so go ahead and create *add.html* at **public > js > templates**. This will hold the html markup for that page when we view the #/add route through angular. I’m going to hold off on showing any source for right now because it’s strictly html and we’ll be adding the angular goodness to it shortly – but for now add what ever you’d like to that template file, perhaps a catchy phrase or cat picture? _I don’t know your taste…_

Now that there is a template in place we can tell angular how to handle the route. Open up **public > js > app.js** and add the following to _app.config_ –

$routeProvider.when('/add',{ templateUrl:'js/templates/add.html', controller:'PostController' });

This should go right under the existing routes for the root (/) and dashboard (/dashboard). So just as a refresher this is telling angular when it encounters that URL (/add) use add.html as the view and PostController as the controller for manipulation. And that’s it – if you save and head back to the dashboard and click your ‘add new post’ button you should be taken to your add.html template. Note that angular live loads things, so if you’re still seeing a white screen and not your cat picture it is probably just a matter of having to refresh the page.

<div class="wp-caption alignleft" id="attachment_449" style="width: 310px">[![updated-dashboard](http://justinvoelkel.me/wp-content/uploads/2014/11/updated-dashboard-e1416505220618-300x102.png)](http://justinvoelkel.me/wp-content/uploads/2014/11/updated-dashboard-e1416505220618.png)The updated dashboard – create post button on the left and on the right where Ill be listing the existing posts

</div><div class="wp-caption alignleft" id="attachment_450" style="width: 310px">![The new add template. Pretty basic.](http://justinvoelkel.me/wp-content/uploads/2014/11/add-new-post-e1416505472808-300x222.png)The new add template. Pretty basic.

</div> 















So lets make this do things, shall we? Since we migrated and seeded the post table with an example post why don’t we start at the dashboard and have it pull all the posts (in actuality – one – our seeded example) and display it’s meta info on the dashboard.

## Getting Resourceful

Looking down from the ten thousand foot view of the project we know there are a handful of things we’ll want to do with our posts inherently. Namely, we want to do things like show all and perform our CRUD functions. We’ve already set up an uber basic controller within angular but it’s worthless without the appropriate api routes inside Laravel.  Now, we could create individual routes for each action we want to take – for example we could create a route for returning all the posts, one for creating a post, updating…blah blah you get the point. It’s kind of tedious. Since we’re working with a **_collection_** of posts we can use a resource and the controller generator in Laravel to generate the appropriate boilerplate for the controller. Yes, that sounds confusing, let me explain. Resources in Laravel are meant for data types as collections – aka stuff you’ll want to pull and sort and update like posts, songs, pictures etc. By using **route::resource** Laravel allows us several different route variants for all the different things we want to do. Let me illustrate. Open **app > routes.php** and you’ll see our auth routes in there using **route::get **and **route::post**. Those work just fine as is for their purpose. To illustrate those routes that are currently available go to your terminal/console/git bash (however you are using the laravel cli) and go to your project’s root or where ever you have the artisan file (for me it is right in the root). Run the php artisan routes command and you’ll see a nicely formatted table of the routes available to you based on your **routes.php** file. By the way, keep this open we will be going back to it in just a minute.

<div class="wp-caption aligncenter" id="attachment_453" style="width: 755px">![artisan-routes](http://justinvoelkel.me/wp-content/uploads/2014/11/artisan-routes-e1416508216197.png)You see our current api routes that communicate with our angular auth service and the basic routes laravel uses to display the admin and index pages.

</div>So the first thing I’m going to do is just generate the controller we want to use for posts. This is how Laravel is going to perform actions on the database when instructed by our angular front end.

$ php artisan controller:make PostController

Ideally you’ll get a success message saying you’re controller has been created. Now you should be able to open up **app > controllers > PostController.php**. How easy is that? If you look at that file you’ll see that Laravel has graciously generated a bunch of methods for creating, updating, and deleting (among other things) our posts! Now we can route our post resource to that controller that we just generated. So open up **app > routes.php **and we’ll be adding the resource to our api route group –

Route::group(array('prefix'=>'/api'),function(){ Route::post('login/auth','AuthController@Login'); Route::get('login/destroy','AuthController@Logout'); Route::resource('posts','PostController'); });

Now, to illustrate exactly what this is doing head back into your console environment and run $ *php artisan routes *again and see the difference.

[]()

<div class="wp-caption aligncenter" id="attachment_458" style="width: 998px">![artisan-routes-resource](http://justinvoelkel.me/wp-content/uploads/2014/11/artisan-routes-resource-e1416509845415.png)routes table!!

</div>Now, as you can see, the resource added a bunch of new routes for posts and the methods associated with the controller we generated. So how does this work? Basically the idea of the resource is to use basic routes combined with different http verbs to do our bidding (you’ll see that some routes are gets some are posts/patch/delete and so on). This allows us flexibility in dealing with our data type and it saves us from creating individual routes for each of the methods in our controller (it’s cleaner).

We’ll be using the index method to send all of our posts back to angular so lets fill that out with the code we’re going to need. So inside of **app > controllers > PostController.php** update your index method to look like the following –

public function index() { $posts = Post::all(); return $posts; }

I’ll quickly break that down – We previously had set up a ‘Post’ model so we can easily reference the posts table by using only Post::. And, as you can see, we’re using the all method to pull all records from that table. Pretty simple right? Laravel will cast the response in json automagically so we can just return the variable post.

Ok, so! Now that we’ve gotten our route and controller setup for the posts and we’ve setup our method to retrieve the records from the database we can finally get some calls working from our angular CRUD service.

## Total Recall

Head back to our CRUD service in angular – **public > js > services > crudService.js**. We previously set up all of these empty functions and it’s about time we started filling them. To retrieve all of our existing posts and list them we’re going to need to use one of the routes listed above – can you figure it out?

all: function(){ var request = $http({method:'GET', url:'api/posts'}); return request; },

We’ll be making a get request to /api/posts and given the routes we’ve set it will invoke the index method in our controller which we just set up to return all of the records in our posts table. You can actually test this out before you go to the controller if you want by using something like [postman](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0CCAQFjAA&url=https%3A%2F%2Fchrome.google.com%2Fwebstore%2Fdetail%2Fpostman-rest-client%2Ffdmmgilgnpjigdojojpjoooidkmcomcm%3Fhl%3Den&ei=HVpuVJSvELXOsQTa4YD4Ag&usg=AFQjCNHaecLwAKk91gpdCY_y1x_ViIrHwQ 'Postman for chrome') or just by putting the api route straight into your browser –

![api-posts-test](http://justinvoelkel.me/wp-content/uploads/2014/11/api-posts-test-e1416518352122.png)

Well lookie here that’s our example post! Now, we need to take that json and plug it into our dashboard area. To do that let’s open up the posts controller in angular **public > js > postController.js**. We need to add a few things to what is currently in there – the post controller should now look like this –

post.controller('PostController',function($scope,CRUD){ var getPosts = CRUD.all(); getPosts.success(function(response){ $scope.posts = response; }); });

You’ll see that I’ve injected the CRUD service along with $scope – this is how we’re going to get our json and ultimately set it in the scope of the template. You see that we’re invoking the all() method of the CRUD service – this is what is going to make our call to our Laravel controller and use the index method to send back all of our blog posts. getPosts is our http promise – for sake of simplicity I’m using **_success_** but you can use the more verbose **_then_** to handle things as well. So, given our http request is successful we can set our scope variable with the response – that **_response_** variable is going to be exactly what you saw in postman or the browser window above. When we have more than one blog post that response is going to be a json formatted array. With that array we can make use of [angular’s ng-repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat 'angular ng repeat'). Basically ng-repeat is a quick drop in for a loop – feed it an array and it’ll loop until it hits the end, it’s clean and it’s simple so lets take a look at how to do that with the posts response.

Now is the point at which I’m going to post the updated dashboard code, you’ll see the ng-repeat gently nestled inside of the table.

    <title>Angular JS Dashboard</title>   <h1>The Most Dashing of Boards</h1> <div class="col-md-3 well"> <a class="btn btn-primary" href="#/add">Create New Post</a> </div><!--3cols--> <div class="col-md-9"> <table class="table table-striped"> <tr> <th></th> <th>id</th> <th>title</th> </tr> <tr ng-repeat="post in posts"> <td><button class="btn btn-success btn-xs">edit</button></td> <td>{{post.id}}</td> <td>{{post.title}}</td> </tr> </table> </div><!--9cols-->

So now you can see our table row that contains the ng-repeat. Remember we’ve set our scope variable to ‘posts’ so that ng-repeat directive will loop through ‘posts’ as ‘post’ and since we’re dealing with an object we’re using the dot format to display the ‘id’ and ‘title’.

<div class="wp-caption aligncenter" id="attachment_487" style="width: 310px">[![The new -more dashing- dashboard](http://justinvoelkel.me/wp-content/uploads/2014/11/dashboard-list-posts-e1416605245934-300x86.png)](http://justinvoelkel.me/wp-content/uploads/2014/11/dashboard-list-posts-e1416605245934.png)The new -more dashing- dashboard

</div>Isn’t it great seeing stuff ***actually ***showing up in the dashboard? So cool. Ok, we’re going to stick with the dashboard for the time being and work on adding a new post and finally we’ll wrap up by editing an existing post.

## Adding a Post

To start we’ll be adding some angular goodness to **public > js > templates > add.html** – remember this is our add a new post template. Again, I had previously not posted anything from this page for this exact reason. Here it is –

    <title>Add a New Post</title>   <h1>Add a New Post</h1> <div class="col-md-12 well"><a href="#/dashboard"><< Back to Dashboard</a></div> <div class="col-md-12"> <form> <div class="form-group"> <label for="title">Post Title</label> <input ng-model="new.title" class="form-control" name="title" type="text"/> </div> <div class="form-group"> <textarea ng-model="new.content" class="form-control" cols="30" rows="10"></textarea> </div> <button ng-click="submit()" class="btn btn-success">Save</button> </form> </div>

There are a few things to note here. Were using ng-model to store the title and content to a new scope variable conveniently named ‘new’. Again, we’re using the dot format as you would with a javascript object (new.title etc). Also, we’re using the **ng-click** directive to handle the submission. This will fire the submit function within the controller when the button is clicked. To illustrate how that works we can add the following to the post controller (** public > js > controllers > postController.js** ) –

$scope.submit = function(){ console.log($scope.new); }

This is just going to console log whatever we have in our new post fields after we hit the submit button. Open up your web dev tools in your browser and check your console to see something similar to this –

![angular submit test](http://justinvoelkel.me/wp-content/uploads/2014/11/test-submit-e1416609562799.png)

Great! Lets figure out how we’re going to send this object to laravel. Remember we have our CRUD service already set up so we just need to send ‘new’ to the *create* function and , in turn, send the appropriate http request to laravel. After we’ve done that we’ll go into our laravel controller, accept that data, parse it send it to the database.

First, let’s swap out our console.log test. You’ll want the following –

$scope.submit = function(){ var request = CRUD.create($scope.new); request.success(function(response){ console.log(response); }); }

You’ll see we are invoking the create function within the CRUD service and we are sending it our new scope variable. Again, this is a promise so we are going to handle it the same way we handled the promise when we requested all posts. And, for now we are going to just console log the response – this is going to be empty for the time being till we tell laravel to send back an error or a confirmation. Next open up **public > js > services > CRUDService.js**. I’m sure you’ve guessed by now we need to put something into that create function – it’s just going to be another laravel api route – can you guess which? (check out the route table above again). Here is the updated create function –

create: function(data){ var request = $http({method:'GET', url:'api/posts/create',params:data}); return request; },

So we are accepting the new post data (as ‘data’) and including that as the param being sent to our create api url and returning whatever laravel sends us back (currently absolutely nothing). Not too bad right? And you thought this would be hard and time consuming….

Now that we have half of the equation complete we need to set up our laravel controller to accept and store our new post data. Open **app > controllers > PostController.php**. The method we’re looking for is *create()*. For testing purposes let’s throw something basic in for now to make sure our api route is working correctly –



public function create() { return Input::all(); }

Simple right? We’re looking to get the exact same output as the last test except this time it’s going to be a returned JSON string from laravel. If you wanted to try something different you can just throw in a simple string in place of returning the input items. If everything is wired correctly you should get something similar to the following after you hit save-

<div class="wp-caption aligncenter" id="attachment_492" style="width: 678px">![Add post api route test](http://justinvoelkel.me/wp-content/uploads/2014/12/add-api-test-e1417723555297.png)After you submit you should see the console log inside the promise.

</div>That is proof positive that our route is working. So, instead of returning that data lets save it to the database. It is going to look something like the following –

public function create() { $new = new Post; $new->title = Input::get('title'); $new->content = Input::get('content'); if($new->save()){ return array('status'=>'Saved!'); } return array('status'=>'Not Saved!'); }

Once this is saved you can run another test – you don’t even have to refresh the page! Since the change was made to the api route it won’t be invoked until it’s requested from angular. To explain, we’re creating a new record of our model Post and assigning the values of our inputs to the appropriate values as they relate to our model’s table. The save method is truthy so we’re simply checking to make sure the post saves before returning a success statement. If the record does not save we’ll send back an error. Now when you invoke your save method from angular by hitting the save button you should see the following –

![Post save test](http://justinvoelkel.me/wp-content/uploads/2014/12/post-save-test-e1417725651326.png)

Now would be a great time to login to phpmyadmin and check out your DB to make sure the record saved correctly but, in general, you can be pretty confident if you get your saved confirmation it’s been saved as expected. Since we won’t always have a dev tools console log open why don’t we add a simple flash response to show our incoming status. Head back to your **PostController.js** and we’ll be replacing the console log with the following –



$scope.submit = function(){ var request = CRUD.create($scope.new); request.success(function(response){ $scope.flash = response.status; }); }

You can see we are now setting a scope variable with our response status. Now we just need to put that into our angular template. Open **add.html** and add this alert in anywhere you like (I added mine right above my save button) –

 <p class="bg-info">{{flash}}</p>

This will **_only_** show if the value is set. You could set ng-show/hide if you want but it isn’t necessary. If you reload the page and add another post you should now get a status update –

<div class="wp-caption aligncenter" id="attachment_496" style="width: 618px">![post save alert](http://justinvoelkel.me/wp-content/uploads/2014/12/post-save-alert-e1417727256358.png)Not the prettiest but it serves its purpose

</div>Great! That pretty much wraps up adding a post. If you go back to your dashboard VOILA check out all those new posts! Am I the only one genuinely excited about this??? Sure I’m not – you’ve done well.

## Editing a Post

First, we’re going to be lazy. Go copy your **add.html** template to a new template named **edit.html**. It’s literally going to be the same except for our scope variable name and some titles. You could actually combine the two and just use one template but for the sake of clarity I like to separate the two even though they’re nearly exactly the same. Below is **edit.html** –



    <title>Edit a Post</title>   <h1>Edit a Post</h1> <div class="col-md-12 well"><a href="#/dashboard"><< Back to Dashboard</a></div> <div class="col-md-12"> <form> <div class="form-group"> <label for="title">Post Title</label> <input ng-model="post.title" class="form-control" name="title" type="text"/> </div> <div class="form-group"> <textarea ng-model="post.content" class="form-control" cols="30" rows="10"></textarea> </div> <p class="bg-info"> {{flash}} </p> <button ng-click="submit()" class="btn btn-success">Save</button> </form> </div>

Now, we need to create a route for editing a post that will map to this template. How are we going to formulate that route though? Each one of these posts are different. They all have different attributes and different record id’s – oh yeahhhhh. Since the post id is our primary key and is unique to each individual post we’ll be using that in our routing to tell laravel which post to retrieve and edit. Open **app.js** and add a route –



$routeProvider.when('/edit/:id',{ templateUrl:'js/templates/edit.html', controller:'EditPostController' });

A few things to look at here. First, you will see the :id at the end of our route. That denotes that value should be taken and placed in a variable called ‘id’ and made available to our controller as a [route parameter](https://docs.angularjs.org/api/ngRoute/service/$routeParams 'Angular route params'). Second, we have a new controller – ‘PostEditController’. This controller will handle everything related to editing a post. Go ahead and open up **postController.js** and add the following –

post.controller('EditPostController',function($scope,$routeParams,CRUD){ var getPost = CRUD.get($routeParams.id); getPost.success(function(response){ $scope.post = response; }); $scope.submit = function(){ var request = CRUD.update($routeParams.id,$scope.post); request.success(function(response){ $scope.flash = response.status; }); } });

Again, remarkably similar to our first controller. Note that we have injected [$routeParams](https://docs.angularjs.org/api/ngRoute/service/$routeParams 'Angular route params') as a dependency. This is what allows us to retrieve our post id from the route url. We’re now using the ‘get’ function of our CRUD service to find the specific post with our route param id and setting it’s response to the scope variable ‘post’. Finally, our submit function is using the update function of our CRUD service to update the specific record with the values in our scope variable ‘post’ and, on success, updating the user with our flash status. Now, we don’t have anything in the **_get _**or **\*update\*\*\*** functions in our CRUD service. Take a look, once again, at the [routes table ](#routes)from earlier in the post and see if you can figure out what api routes we’ll use.

Since we want to be shown the current values for the post we’ll be using the ‘show’ method (GET route – /api/posts/{id here}) and the ‘update’ method (PUT route – /api/posts/{id here}). We’ll be following the same general guidelines as ‘create’ so ‘get’ and ‘update’ should look like this –

get: function(id){ var request = $http.get('api/posts/'+id); return request; }, update: function(id,data){ var request = $http.put('api/posts/'+id,data); return request; },

Note that in these two examples I changed things up a bit. We are using the shorthand get and put methods. It does the same thing as those above it but it is a little less verbose.

Finally, we need laravel to do things. Remember, these routes are already mapped through our resource in laravel. We just have to fill out the appropriate methods (show,update) with code to perform the actions we’re looking for. First, the ‘show’ method, brace yourself…..

public function show($id) { return Post::find($id); }

Yep, that’s it. Remember that laravel casts that to JSON for you so we don’t need any more magic here. Next the ‘update’ method –

public function update($id) { $post = Post::find($id); if($post){ $post->title = Input::get('title'); $post->content = Input::get('content'); if($post->save()){ return array('status'=>'Updated!'); }else{ return array('status'=>'Could not update!'); } } return array('status'=>'Could not find post!'); }

So, this is a bit more complex. But, in actually, we’re just applying a few of the methods we used earlier. First, find the post by the param ‘id’. Then, check to make sure that record exists ‘if($post)….’ – if not return an error. Finally, set our (successfully found) post’s attributes to our given input’s (title/content) and save (or else return an error). That’s it man! We’re all wired for the thrill of now being able to edit a post. Actually, I’m just playing with you – if you go back to your dashboard and click edit you’ll get a whole bucket of nothing right now. I saved the easiest part for last – we need to wire up the button(s) in **dashboard.html**. Take a look at your ng-repeat where we are displaying the edit button – *Note: I noticed this was actually a button element before, I changed it to a link. The css keeps it looking the same.*

 <tr ng-repeat="post in posts"> <td><a href="#/edit/{{post.id}}" class="btn btn-success btn-xs">edit</a></td> <td>{{post.id}}</td> <td>{{post.title}}</td> </tr>

Boom. Save everything up and head back to your dashboard. If you hover over your edit buttons they should be wired up with the appropriate link with the correct post id. Click into one and behold the magic that awaits your eyes. BEHOLD POST INFORMATION! Yeah so it’s not all that revolutionary but you have a very basic functioning blog. Try saving your edits and feel the power that radiates through your keyboard.

## The Conclusion Part

So now that we’ve covered creating, listing, and editing our posts there are just a few more functions we’ll need to add. In the next part of the series I’ll be covering deleting posts from our dashboard area and, finally, displaying the posts to the public. Depending on the length of that post I may cover some helpful additions in functionality like the tinyMCE editor among other things.


