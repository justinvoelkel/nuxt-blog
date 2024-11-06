---
title: Let's Do Something New - Building and Testing a REST API in Slim 2
date: 2015-09-26T00:20:11.000Z
description: Let's Do Something New - Building and Testing a REST API in Slim 2
---

So it’s been roughly four months since I’ve posted anything — life has a funny way of getting in the way of doing this kind of stuff. Things have taken a turn for the responsible for yours truly — I’ve just purchased a home and have been really caught up at work as our team has grown in the past few months. Alas, now that things have begun to settle I’ve wanted to start up another series of posts. It seems like my last series on pairing [AngularJS](http://laravel.com/) with [Laravel 4](http://laravel.com/) went over pretty well so what the heck, let’s do it again with a fresh subject.

There were a lot of ways I could have gone. After working with Laravel for a while it’s shortcomings (not all bad) become clear…especially when you consider the scope of the projects you’re working on. Utilizing a heavy, feature rich, framework like Laravel for a simple blog or application is, in all fairness, overkill. With that said I kicked around the idea of basically creating the same application as the Laravel/Angular series with a micro framework like [Slim](http://www.slimframework.com/), [Lumen](http://lumen.laravel.com/), [Flight](http://flightphp.com/), or [Bullet](http://bulletphp.com/) to illustrate the importance of using only what you need and nothing more. But that got me thinking…. Lightweight? Only what you need? PHP frameworks? APIIIIIIII, Bro.


## Sharing Is Caring

Over the past several years API’s have exploded onto the scene. Seemingly everywhere you go on the web has some sort of fancy API. Even Granny Mae’s Grand Cheese Emporium is probably serving up a [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API for when you want access to info on the historical cheese of the Eastern European block. So what better way to expound upon the basic CRUD API we wrote for the previous series? Let’s learn how to create something business critical using one of those light weight micro frameworks.

I’ve worked with Slim a little bit previously and I found it really easy to get up and running quickly and painlessly so that’s what I’ve settled on for the framework. Also, along the way we’re going to explore a few different concepts that the previous series did not. First, we’re going to use composer exclusively to manage our dependencies. Composer has quickly become the [npm](https://www.npmjs.com/) equivalent to the PHP community over the past few years — and for good reason, it’s amazing. It’s a great tool that any PHP dev should leverage — so that’s exactly what we’ll do. Second, we’ll take a walk on the wild side, just kidding, we’re going to take a walk on the dark side of PHP development and we’ll actually write tests to test our application. It’s something I’ve been working on quite a bit lately and I think it would be really helpful for you all to see the building of the application and the testing that goes along with it. A lot of the documentation I’ve come across relies on a foo/bar method of showing how to write tests and it’s not immediately apparent how, why, or what you’re testing. Using a more practical real world example should be helpful in tying it all together.


## Things That Are Next

I’m going to attempt one post a week on this. It’s a bit less ambitious than the previous series in terms of scope so I think it’s probably do-able. In fact I’ll be starting the first installment right after this. If you’re a github type you can find the repo [here](https://github.com/justinvoelkel/how-to-slim-api.git) or you can just follow me for the updates. Subject matter is still TBD but I’ll try to make it something interesting and not cats, cars, or other generic silly things.


