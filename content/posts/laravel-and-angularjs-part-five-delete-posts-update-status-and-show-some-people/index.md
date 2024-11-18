---
title: Laravel and AngularJS - Part Five - Delete Posts and Status Updates
date: 2015-03-08T04:33:48.000Z
description: Laravel and AngularJS - Part Five - Delete Posts and Status Updates
---

So, lets try and bring this sucker home, shall we? The [previous part to this series](http://justinvoelkel.me/laravel-angularjs-part-four/ 'Laravel and AngularJS: Part Four – The Dashboard Part Deux and a Posts Party') covered *most* of our CRUD capabilities – but we haven’t yet covered how to delete posts from our blog application. Let’s start there and eventually we’re going to move into displaying a blog roll of our posts on our blog’s index and links to individual posts.

## Just Something to Click

Let’s start with the easiest thing we can do and create the button to delete a post in our admin area list. Open up <span class="highlight">**Public > js > templates > dashboard.html**</span>. We’re going to add a button in much the same way we did for our edit button — I’m just adding in a new column, at the end of our rows, to house the button. The updated source for the table —

<table class="table table-striped"> <tr> <th></th> <th>id</th> <th>title</th> <th></th> </tr> <tr ng-repeat="post in posts"> <td><a href="#/edit/{{post.id}}" class="btn btn-success btn-xs">edit</a></td> <td>{{post.id}}</td> <td>{{post.title}}</td> <td><a ng-click="remove(post.id,$index)" class="btn btn-danger btn-xs">delete</a></td> </tr> </table>

We’ve added in a simple delete ‘button’ – I’ve used an anchor and a class – but you’ll notice no link attached. It’s because we’ll be using angular’s [ng-click directive](https://docs.angularjs.org/api/ng/directive/ngClick 'AngularJS ng-click directive') to call a function in our controller. It’s a simple as it sounds — when the button is clicked, it calls the remove function in our controller and feeds it the id of our current post along with it’s index value within our scope array of posts – this will be important a bit later. Since it’s part of our ng-repeat, and a new button is created unique for each post the post.id will correspond exactly with the post id in our database. Ok great, now lets setup this function.

## Wiring Up Delete

So, we need to wire our newly created delete button to take the id of the post we’d like to delete, form a proper RESTful to submit to Laravel, and output the result. No big deal. Since were deleting directly from our list of posts we’ll be working in the ‘PostController’ we already have setup. Full disclosure, this is not a great separation of concerns. Ideally you’d work in an intermediary to allow you to create a new controller for deletion. In my full scale application I use modals to verify the user does in fact want to delete the item and then performs the request. But, for our simple application we’ll go the quick, easy, and efficient route and place it inside of our existing controller. Open <span class="highlight">**public > js > controllers > postController.js**</span> and we’re looking to edit the first controller in there ‘PostController’. We’re going to add the following to the very end of that controller:

$scope.remove = function (id,index){ var removePost = CRUD.delete(id); removePost.success(function(response){ $scope.flash = response.status; }) }

So, this is our remove function call. This process should be fairly familiar at this point. We’re calling the delete function within our CRUD service and feeding it the appropriate post id, creating a promise, and waiting for the response. Don’t worry about the index value passed in just yet, we’ll be coming back to that.

To complete our work in the angular side we just need to fill out our delete function in our CRUD service to create the correct request. Remember we’re working RESTful so we’re using the ‘delete’ http verb in our request.

delete: function(id){ var request = $http.delete('api/posts/'+id); return request;}

Using the correct http verb here is very important. In previous posts we’ve set up a resource for our Post model. That means that there is already a pre-configured route and method in our controller to handle this request! Fantastic, we’ve gotten our delete request setup and sending Laravel the id of the post we want to remove. Now, we just need to catch that request, perform the requested action, and send a result back. Onward!

## Deleting with Laravel

Open up**<span class="highlight">app > controllers > PostController.php</span>**. Scroll to the bottom and you’ll find the ‘destroy’ method. For now, let’s just test things out:

public function destroy($id) { return 'we\'re deleting post '.$id; }

Now, if the stars have aligned, when you open up your developer tools console and click your ‘delete’ button you should see:

[![laravel delete return](http://justinvoelkel.me/wp-content/uploads/2015/03/delete-request.jpg)](http://justinvoelkel.me/wp-content/uploads/2015/03/delete-request.jpg)

So, now we’ve got proof that our request is being made and sent back correctly. Great, let’s remove the appropriate record now.

public function destroy($id) { $remove = Post::where('id',$id)->delete(); if($remove){ return array('status'=>'Post successfully deleted!'); } return array('status'=>'Could not delete post '.$id); }

A brief explanation — we doing a straight forward delete using our ‘Post’ model where the column ‘id’ matches our given $id. The result of that statement is truthy so we can simply check the value of $remove pass/fail. If it fails we just return a statement with some text and the id that was given.

Just a few last tidbits here — let’s go back to our <span class="highlight">**dashboard.html**</span> and add in a flash element to display our return message. We’ll add it right under our h1 tag:

<h1>The Most Dashing of Boards</h1> <div class="col-md-12"> <p class="bg-info"> {{flash}} </p> </div>

Also, if you delete a post right now, you’ll notice you get your flash message of success or failure but the record remains in our list. Why don’t we clean that up. Remember that second value passed into our remove function? You guessed it, time to put it to good use. Within our delete function inside of our ‘PageController’ controller we just need to [splice the array](http://www.w3schools.com/jsref/jsref_splice.asp 'javascript array splice') after we know we’ve successfully deleted the record from the database. The new remove function should look like this:

$scope.remove = function (id, index){ var removePost = CRUD.delete(id); removePost.success(function(response){ $scope.flash = response.status; $scope.posts.splice(index,1); }) }

Now, when the post has been confirmed deleted we can officially remove the record from our list.

## Setting The Post Status

Wayyyy back in [part one of this series](http://justinvoelkel.me/laravel-and-angularjs-part-one-prep-your-app/ 'Laravel and AngularJS: Part One – Prep Your App') we created a migration for our post tables and included a column called ‘live’ defaulted to 0 (false). We want to be able to control the status of our posts moving forward and not just automatically output every half written post we have in our queue. Since we just wired up our delete button you can probably figure out a similar process we might go about for setting the post status. We’ll need to add another button that will fire a function to change the post’s ‘live’ status from 0 (false) to 1 (true) and update our record in the database. Luckily we already have an update request setup in our CRUD service. First the button, I’ve updated our table with a new blank header and a button below — this may look confusing at first:

<tr> <th></th> <th></th> <th>id</th> <th>title</th> <th></th> </tr> <tr ng-repeat="post in posts"> <td><a href="#/edit/{{post.id}}" class="btn btn-success btn-xs">edit</a></td> <td> <a class="btn btn-xs" ng-click="status(post)" ng-class="{'btn-success':post.live==1,'btn-warning':post.live==0}"> <span ng-show="post.live==1">click for draft</span> <span ng-show="post.live==0">click for live</span> </a> </td> <td>{{post.id}}</td> <td>{{post.title}}</td> <td><a ng-click="remove(post.id,$index)" class="btn btn-danger btn-xs">delete</a></td> </tr>

So, I’ve highlighted the lines that are really important additions. To break this down let’s start with the ‘ng-click’ function call. We’ll be calling to a function in our controller called status and we’re sending the entire post. Since this is within our ng-repeat we’ll be able to access the post id and the post live status. Next, we’re conditionally applying a new class to our button. If the post is live (1) we’re going to apply a green color if the post is in draft form (0) we’re applying yellow. And, similarly, we’re using a couple of span’s with ng-show to show the correct text in the button based on the status of the post. Now, let’s take a look at the function that will be added to our post controller.

$scope.status = function (post){ post.live = post.live==0 ? 1 : 0; var setStatus = CRUD.update(post.id,post); setStatus.success(function(response){ $scope.flash = response.flash; }); }

I’ve highlighted one particular line from our function. Notice I’ve used a ternary expression to handle changing the live status between 1 and 0. If you’re not familiar it simply reads like a shorthand if statment — if post.live is equal to zero (our default) then set it to one else set it to zero. This way, we take into account our default value as such and if there is a change we reflect it accordingly. The remainder of the function should look somewhat familiar. We’re simply calling our update function and sending it the appropriate post id and the updated post information (with live now set to the appropriate value). Finally, there is one thing we need to address in our PostController.php file within Laravel. We originally set the update method to accept the post title and post content because that’s all we were updating. To record the ‘live’ status change in the database we just need to accept the ‘live’ input and it’ll be saved with the rest of the post information. The lone addition is highlighted below:

public function update($id) { $post = Post::find($id); if($post){ $post->title = Input::get('title'); $post->content = Input::get('content'); $post->live = Input::get('live'); if($post->save()){ return array('status'=>'Updated!'); }else{ return array('status'=>'Could not update!'); } } return array('status'=>'Could not find post!'); }

If all of that is looking good, you should now be able to toggle the post status between live and draft with just a click of a button!  
[![blog status update](http://justinvoelkel.me/wp-content/uploads/2015/03/blog-status-update-result.png)](http://justinvoelkel.me/wp-content/uploads/2015/03/blog-status-update-result.png)

## Obligatory End Part

Ok, so we covered a few additional items here — how to delete posts and how to update a posts status from draft (0) to live (1). This one ran a little long, when do they not? So in the final post in this series, part six, I’m going to cover setting up the front end templates to display our blog roll (all the posts with a post excerpt and link) and the ‘single’ (if your familiar with wordpress) post templates. Till then, keep it class interwebs!
