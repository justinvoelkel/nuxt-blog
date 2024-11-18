---
title: Slim 3 REST API - Part One - Composer Dependencies, Tools, and Bootstrapping
date: 2015-11-11T02:04:47.000Z
description: Slim 3 REST API - Part One - Composer Dependencies, Tools, and Bootstrapping
---

So, as I mentioned in my [introduction](http://justinvoelkel.me/lets-do-something-new-building-and-testing-a-rest-api-in-slim-2/) post we’re going to create a slim 2 REST api from scratch. To do this you’re going to need a few things at your disposal. I’ve compiled a list:

1. An IDE — doesn’t really matter which one just something to write code in.
2. A local dev environment — I’m partial to [Vagrant](https://www.vagrantup.com/), but you choose.
3. A REST client for testing endpoints — I’m using the REST client built into PHP Storm but if you google you’ll find any number of suitable options. [(I’d also suggest postman)](https://www.getpostman.com/)
4. Access to a CLI environment — I’m working on a Mac so all these commands will be in \*nix format but if you’re on a windows machine [git bash](https://git-for-windows.github.io/) is a suitable option.
5. [Composer](https://getcomposer.org/) — This can be a bit tricky if you’ve not yet worked with it. Luckily the docs are really well written. We’ll be using it to pull in and manage our dependencies.
6. MySql Client — Maybe not totally necessary but will def help with viewing and manipulating data. ([Sequel Pro](http://www.sequelpro.com/), [Heidi SQL](http://www.heidisql.com/) et. al)

That’s really all you’ll need moving forward.

## Composition as a Mission

I mentioned above that we’ll be using composer to pull in and manage our dependencies. Our first task is to create the directory for our api to live in and to setup a composer file to tell it all the things our application will depend on. If you haven’t worked with a dependency manager before think of it like an ingredients list. Our api will be running on the [Slim](http://www.slimframework.com) micro framework — so that will be one of the dependencies/ingredients. We’ll use and [ORM](https://en.wikipedia.org/wiki/Object-relational_mapping) to interact with our database so that or will be another dependency/ingredient…and so on. So, in your development env (vagrant/wamp/mamp..) create an <span class="highlight">api</span> directory.

Composer expects to see a file with our list of dependencies in order to fulfill it’s end of the bargain. So, getting this setup is as easy as creating a new file — <span class="highlight">composer.json</span>. As you can tell by now the file is in [json](http://www.json.org/) format. If you’re not familiar with json it’s an easy to read/write representation of data — it’s (sort of) the successor to the xml format from years past.  So now we have this big empty file just waiting to be filled with all the goodies we need to build our api. Let’s fill this sucker out:

```language-javascript
{
    "require":{
        "slim/slim":"~3.0",
        "illuminate/database":"~5.0"
    }
}
```

We’re just defining our dependencies by their package names and specifying which versions of each package we want. To find more packages and to learn more about composer dependencies you can go to [Packagist](https://packagist.org/). The two packages we’re pulling in are the Slim micro framework and our ORM (this will look familiar later if you’ve worked with Laravel). Once you have your file saved you can now head to your CLI (terminal/git bash) and run <span class="highlight">composer install</span>. This will install our dependencies and anything our dependencies depend on (yeah I know that’s some inception type stuff). It will create a <span class="highlight">vendors</span> directory with all of the packages source code along with a super helpful auto load file (we’ll deal with that in a second).

## Starting Up

Now that we’ve got our dependencies defined and pulled in let’s just spend a couple minutes laying the groundwork for the rest of the application. To keep things nice and tidy let’s create and <span class="highlight">app</span> directory. This is where our application code is going to live. After you’ve created that file go ahead and create a new file in <span class="highlight">app</span> called <span class="highlight">start.php</span>. This is where our composer auto load file comes into play. Unlike the olden wild west days of PHP development you no longer need to write your own auto loader, composer just politely does it for you. Requiring this file at the head of the application will allow us to access the namespaced packages within our vendors folder really easily.

Now that we have our auto loader included we should have access to the Slim. To create a new instance of Slim we do the following:

```language-php
use Slim\Slim; require_once __DIR__.'/../vendor/autoload.php';
$app = new Slim(); $app->run();
```

So, you’ll see were including Slim via [namespacing](http://php.net/manual/en/language.namespaces.php) up at the top. We require our auto load file and then instantiate a new instance of slim with the variable <span class="highlight">$app</span>. We can then invoke the run method from our <span class="highlight">$app</span> variable and we’ve effectively bootstrapped our application.

## Wrapping Up

There isn’t too much here to dig our teeth into technically but we’ve laid solid groundwork for what we’ll be doing moving forward with the api application. Moving forward we’ll expand do some configuration to connect to our database and expand the application structure to begin adding routes that will serve as endpoints for our api application.




