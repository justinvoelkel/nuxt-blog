---
title: Laravel and Angular - Protect Your Laravel API Routes
date: 2015-01-07T02:20:54.000Z
description: Laravel and Angular - Protect Your Laravel API Routes
---

If any ‘seasoned’ devs happen upon my blog posts they’re probably inclined, and justifiably so, in thinking it’s the blind leading the blind. I’m not afraid to admit that at times it certainly can seem that way. I’ve talked ad nausem about how trial and error have always been my greatest teacher – essentially because of a lack of a solid mentor.

So what does this have to do with anything related to your Laravel API? Well – if you’re a first timer here, first off, welcome – the snacks–are wherever you keep snacks, second, I’ve done a series of posts about [creating a simple blog application with AngularJs and a RESTful Laravel api backend](http://justinvoelkel.me/laravel-and-angularjs-part-one-prep-your-app/ "Laravel and AngularJS: Part One – Prep Your App"). The whole reason for creating those posts was a, seeming, lack of documentation about it on ‘dem googles *and*, the fact that I was creating a more sophisticated app for my full time job. Throughout the lifespan of this project the major selling point has been decoupling the service layer (Laravel) from the view layer (Angular) via the use of http requests and an API. With plans of eventually releasing mobile app(s) based on the service the fact that those API’s were already there to consume made it that much more awesome. The world was sunshine, rainbows, and an assortment of lollipop flavors for a long time.


## Ignorance is Bliss

<div class="wp-caption alignright" id="attachment_569" style="width: 225px">[![computer meme](http://justinvoelkel.me/wp-content/uploads/2015/01/puter-meme-300x246.png)](http://justinvoelkel.me/wp-content/uploads/2015/01/puter-meme.png)A portrait of reality setting in….

</div>This is my first RESTful project and my first time dealing with an API I was responsible for creating. In the midst of reveling in the warm glow of my creation I briefly reflected on just how awesome (and easy) it’s going to be to consume those API’s from other applications and then – **light bulb**. Currently, I have ***no*** security structure in place to lock down consumption of those API’s but more importantly – nothing in place to prevent manipulation of said data. So, in short, if someone wanted to use postman on my beta app and fill a blog post with pictures of nyan cat or – hey, just go ahead and wipe [n] page clean, it was wide open. It was a startling and glaring omission. But hey, I’m not perfect – it’s better to have caught it now, put palm to face, curse myself, and move on to protecting those exposed API routes.

At first, I had no *specific* idea how to go about this. I mean was I going to set a cookie or a session variable with a random secret key that could be rotated on login and destroyed on logout? How do I keep Angular informed what that key is? Do I need OAuth? How do I use OAuth? Should I have just stuck to my high school job at Chuck E. Cheese? When I finally allowed myself to take a step back and forget specifics – I simplified. What exactly did I need? Well, I just needed – for now – a way to keep anonymous users (aka not logged in via my application) away from the API routes. Ok, that doesn’t sound awful, but how? As with most issues I encounter that make a little pee come out at first, the solution was less painful than my wild brain anticipated.


## All Teh Fixin’s

I was already using a routing prefix for my API –

 //Leave the login unprotected - we need that Route::post('api/login/auth','AuthController@Login'); Route::get('api/login/destroy','AuthController@Logout'); Route::group(array('before'=>'auth','prefix'=>'/api'),function(){ //Now we're all protected Route::resource('item','ItemController'); //...etc...

I realize this is kind of a ‘shotgun’ approach. When you think about it do you really need every API route protected? No, probably not. But, the fact is we don’t currently need an unprotected API route for any other apps to consume so this is the easy step forward. When those other apps begin development the necessary routes can be freed from the tyranny of login filtering. Read about it yourself – [straight from the source](http://laravel.com/docs/4.2/security#protecting-routes "protecting laravel routes").


## Conclusion

Another day another lesson learned. Don’t be afraid to feel dumb at times. Always read documentation – Laravel’s is very good. Have a nice day folks.


