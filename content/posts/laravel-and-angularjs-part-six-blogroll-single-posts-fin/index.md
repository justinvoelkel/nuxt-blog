---
title: Laravel and AngularJS - Part Six – Blogroll, Single Posts, Fin
date: 2015-05-23T01:32:42.000Z
description: Laravel and AngularJS - Part Six – Blogroll, Single Posts, Fin
---

Dudes, ladies and all people who may read this – thank you so much for your support of this series! I’ve been really struggling to find the time to get this last part up. Between work, conferences, pony rides, playing an unhealthy amount of hearthstone, and doing several other not even remotely interesting things that will remain untold – the last few months have flown by. Things have thankfully settled a bit and that leaves me with sufficient time to put the final touches on the Laravel and Angular series.

So, [Part Five](http://justinvoelkel.me/laravel-and-angularjs-part-five-delete-posts-update-status-and-show-some-people/) covered removing posts and updating status from live to draft and vice versa. I also threw in a few bonus posts on [protecting your routes](http://justinvoelkel.me/laravel-and-angularjs-bonus-coverage-restricting-routes-in-angular/) so that users have to be authenticated to use the app and [handling logouts](http://justinvoelkel.me/laravel-and-angularjs-authentication-bonus-handling-logouts/). At this point I’m just going to cover the remainder of what we need to truly call this a blog app and that is displaying posts in a blog roll along with links to view the whole post. Moving forward I’m thinking I may do an additional bonus post on adding an editor like tinyMCE or ckeditor to the admin area because that’s something that can be really useful for you all. Let’s light this candle!


## Getting all the Things

If we’re going to create our blogroll it stands to reason we’re going to need all of our blog posts right? 10/10 logic my friends! Let’s start there. Since our front end (user facing) area is all managed by Laravel we need a means of pulling all of our posts from the database and displaying them on the homepage. Lucky for us, we’ve already set up that resource previously! If you open <span class="highlight">app > controllers > PostController.php</span> you’ll see we already have methods **index** (show all) and **show** (show one) that we’ll be using for our blog roll and single posts. So, lets start out by updating our home page route. Open <span class="highlight">app > routes.php</span> and our root ‘/’ route should be at the very bottom. Currently we’re routing to a closure, which is fine for a small application like this. Ideally, if you’re making a larger application you’d map all these routes to controllers for the sake of your own sanity but this is fine for us. We’ll update the code to look like the following:

Route::get('/', function() { $posts = new PostController(); return View::make('index')->with('posts', $posts->index()); });

You’ll see were creating a new instance of the post controller so we can call our index method to retrieve our posts. Fairly straight forward. Now, we can open our homepage view at <span class="highlight">app > views > index.blade.php</span> and access our posts information from within the view. We can test by just trying to get our post titles in the blog roll:

<h1>Laravel and Angular Blog Application</h1> @foreach($posts as $post) <div> <h2>{{$post->title}}</h2> </div> <hr/> @endforeach

So, that just replaces the ‘Blogroll Goes Here’ content we currently had. If you’re unfamiliar, this is how you would implement a foreach loop in the blade templating system. You’ll see we’re referencing our $posts variable we sent over in our route – this contains our collection of ‘posts’ information. If you vardump that variable you’ll see it’s a fairly elaborate object. So if things have gone to plan (when don’t they?) you should see your post titles displaying nicely formatted on the homepage:

[![all posts](http://justinvoelkel.me/wp-content/uploads/2015/05/screenshot-192-168-56-101-2015-05-22-15-20-14.png)](http://justinvoelkel.me/wp-content/uploads/2015/05/screenshot-192-168-56-101-2015-05-22-15-20-14.png)

Awesome right? WRONG! Well, awesome that we finally have something on the homepage that’s dynamic BUT there’s a problem. This is basically just a straight dump of all our posts from the database. We went through all the work of setting up a live/draft state for all the posts and, well, this method just doesn’t account for that. So, you ask, what do we do? Well, we need a way to grab all ‘live’ posts as an option. Let’s look at our controller and see what we can do. Open up <span class="highlight">app > controllers > PostController.php</span> and let’s add some stuff to the index method:

/** * Display a listing of the resource. * @param live * @return Response */ public function index($live = null) { if (isset($live)) { $posts = Post::where('live', $live)->get(); } else { $posts = Post::all(); } return $posts; }

So, you’ll see I’ve just added an optional pass in for the live status we want to look up. If it’s not set, just pass back everything. This should keep our api call intact with no issues while simultaneously allowing us to retrieve live posts for our blogroll. If we flip back over to our <span class="highlight">routes.php</span> file we can modify our route for the homepage with the live status we’re looking for:

Route::get('/', function() { $posts = new PostController(); return View::make('index')->with('posts', $posts->index(1)); });

You can see that the only change is the addition of the status we want (if you don’t remember or tl;dr it’s a boolean). Soooooo, now if we go back to our homepage and reload we should only see the posts that have been set to ‘live’ in the admin area. If you have everything set to live currently you may want to test it out and try setting some posts back into ‘draft’. It’s looking pretty drab though – maybe we should give it some sugar and add in a post excerpt:

 @foreach($posts as $post) <div> <h2>{{$post->title}}</h2> <p>{{substr($post->content,0,200)."..."}}</p> </div> <hr/> @endforeach

Cool beans now we’ve got a little content to show. If you’re unaware, blade’s double curly braces work like php tags in markup — so, we can use php functions inside of them as we did with substr above. Now, last but far from the least, we need to put in the link to our post. It’s important to remember at this point that we don’t have a template or a route setup to handle our single posts. However, we can just assume for now we’ll handle the routing in the following format — domain.com/post/[id]. This is pretty common and it’ll be easy to match in our routes file. Let’s output the links in the blogroll using that format:

@foreach($posts as $post) <div> <h2>{{$post->title}}</h2> <p>{{substr($post->content,0,200)."..."}}</p> <p><a href="post/{{$post->id}}" class="btn btn-primary">Read More</a></p> </div> <hr/> @endforeach

Beautiful! Well, except that Laravel will throw a fit if you try and click on them, but other than that well done!

<div class="wp-caption aligncenter" id="attachment_684" style="width: 882px">[![updated blogroll](http://justinvoelkel.me/wp-content/uploads/2015/05/screenshot-192-168-56-101-2015-05-22-16-31-41.png)](http://justinvoelkel.me/wp-content/uploads/2015/05/screenshot-192-168-56-101-2015-05-22-16-31-41.png)10/10 stars on stack overflow, gang!

</div>
## All the Single Posts – Put a Template On It

Just because I know you’re asking yourself – yes, that is a Beyonce reference – what of it?! Anyways, since we’ve got our links setup we need to now catch those routes in our <span class="highlight">routes.php</span> file and send it off to a template to display our full post. So, to start we’re just going to basically copy our home ‘/’ route and replace some key things. Again, we’re just going to route to a closure for the time being:

Route::get('/post/{id}', function($id) { $posts = new PostController(); return View::make('single')->with('post', $posts->show($id)); });

Notice, again we’re making use of already existing ‘Post’ resource – this time we’re accessing the show method. The curly braces in our route definition define that portion of the URL as what should be assigned to the variable $id and indicates that it’s dynamic. You’ll also notice we’ve changed the view from ‘index’ to ‘single’ to indicate we’ll be using a template called ‘single’ to render this view. Now, we just need to create a really simple template in <span class="highlight">app > views</span> called <span class="highlight">single.blade.php</span>:

@extends('layouts.master') <!-- META --> @section('meta') <meta name="keywords" content="Laravel 4.2, AngularJS, blog application"> @stop @section('content') <h1>{{$post->title}}</h1> <hr/> {{$post->content}} <hr/> @stop

A real work of art right? Ok, it’s not flashy but it definitely highlights how easy it is to quickly build out and template information. ALAS THOUGH – WE HAVE BUT YET ANOTHER PROBLEM. Sorry but I get really excited about problem solving. This kind of goes along with the previous problem – we’re not doing any kind of error checking to make sure the requested post is live. Let’s refactor our find method:

 /** * Display the specified resource. * * @param int $id * @param int $live * @return Response */ public function show($id, $live = null) { if (isset($live)) { $post = Post::where('id', $id)->where('live', $live)->first(); } else { $post = Post::find($id); } return $post; }

So, we’re really applying the same solution here. Allow the status we’re looking for to be passed in and change the query as necessary. This time we’re using the** ‘first’** method in the query as opposed to the** ‘get’** method. That is because by default the** ‘get’** method returns a collection even if only one record is found. Since we can safely assume our id field is unique we can just get the first record that matches our id *and* is live. Now, we just have to add the second argument in our method call in <span class="highlight">routes.php</span>:

Route::get('/post/{id}', function($id) { $posts = new PostController(); return View::make('single')->with('post', $posts->show($id,1)); });

BOOM – errors checked. Sort of, it’ll still complain with a standard Laravel error but at least we’ve locked down our app to only display live posts. And, well….that’s really it. You’re very own, if not very basic, blog application built on Laravel and AngularJS.


## Conclusion

It’s been a long, arduous, and really fun journey to get to this point. I’ve gotten more feedback than I could have ever imagined on these posts and for that I thank you all. Like I mentioned in the intro I’m thinking I’ll add some additional bonus posts on how to add in an editor for the textarea elements in the admin area and possibly a few more smaller items. If there’s anything you all would like to see posts on please feel free to leave them in the comments and I’ll do my best. God speed, happy development, and thank you for reading!

 


