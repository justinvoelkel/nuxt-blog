---
title: Laravel and AngularJS - Part One - Prep Your App
date: 2014-05-06T01:25:50.000Z
description: Laravel and AngularJS - Part One - Prep Your App
---

I know it’s been over a month but, don’t think I’ve forgotten about my quest to deliver you the first part of my series on combining AngularJS and Laravel. I was having trouble coming up with an idea about – one – what to create for the example application, and – two – how I can effectively present it to you. I decided, since the to-do app is pretty played out by now that I’ll create a simple blog with a login and all that fun stuff. Regarding presenting it to you – well – I don’t have the face, voice, or typing skills required for a screen cast so I settled on a happy medium. I’m going to document, in full, my building of this project here on my blog, but you’ll be able to view the up to date project by going to to [laravelandangular.com](http://laravelandangular.com 'Laravel and AngularJS') annnnd I will be adding the source to my github account so you’ll be able to grab it for yourself. So, yeah, that’s that – lets light this fire.

## Things I’ll Thoughtlessly Assume

1. You have some sort of environment to do this in. Either Vagrant, Virtualbox, MAMP, WAMP, XAMPP, or some other form of a LAMP stack complete with [composer](https://getcomposer.org/ 'PHP Composer').
2. You have a command line environment to run the install, migration, and db seeds. If you’re on windows that would be Git bash or something of the sorts.
3. You know how to and have installed Laravel. If you haven’t done this you can see the [official docs here](http://laravel.com/docs/installation 'How to install Laravel').
4. You have setup a database to use for the app. You’ll need the username, password, and name of this database for Laravel’s database config.

That’s it.

## Laying The Groundwork

In case you don’t know – you can run the php artisan serve command to test locally but I’ve included an htaccess file in the repo that will just point all of your traffic to the public folder. In a production environment you’d follow these [directions here](http://stackoverflow.com/questions/19923091/avoid-public-folder-of-laravel-and-open-directly-the-root-in-web-server 'Setup laravel public folder') on moving the public folder into the web facing public_html directory and keeping the rest of the Laravel bits behind the scenes.

I had mentioned in my [previous post](http://justinvoelkel.me/laravel-and-angularjs-ignite-your-app-development/ 'Laravel and AngularJS: Ignite Your App Development') that one major benefit of this pairing is that it keeps things nice and loosely coupled. As we get going you’ll notice that all of the AngularJS work we’ll be doing will end up inside of the public folder. That front facing AngularJS code will then communicate with Laravel (inside of the App folder) through API routes. That might make more sense later after you see it put together.

## Prepping Laravel

Lets start from square one and setup Laravel to connect with the database. Find your way to **App > Config > database.php**. For this example I’m sticking with mySql as my database of choice but you’ll notice several other options to chose from. Enter your database credentials starting at line 55.

```language-php
'mysql' => array( 'driver' => 'mysql', 'host' => 'localhost', 'database' => '[you db name]', 'username' => '[your db username]', 'password' => '[your db password]', 'charset' => 'utf8', 'collation' => 'utf8_unicode_ci', 'prefix' => '', ),
```

Great – with that you should be able to connect to your mySql database. Rejoice. Next were going to need some tables and data to go along with them. For the purpose of this project I’m going to keep it simple and use one table for users and one table for posts. To create these we’ll make use of table migrations and database seeding.

### Creating The Users Table Migration

To setup our users table we’ll run an artisan migration. This is particularly helpful down the road when you’ll want to setup multiple tables when installing your application. To create that migration run the following from the command line:

```language-bash
$ php artisan migrate:make create-users-table --table=users --create
```

If you’ve done this correctly you should see something similar to the following:

```language-bash
Created Migration: 2014_04_18_171339_create-users-table
Generating optimized class loader
```

If you get an error check it out – they’re fairly verbose. I ran into an issue with folder permissions at first and just had to run a simple chmod on my database folder to allow Laravel to write the migration file. Also, make sure you’re running the migration command from the **_directory with artisan inside_** of it (probably your root unless you’ve cloned the project from github in which case it would be in the directory **/laravel**). Great, so now that you’ve got the migration setup you can edit it. If you head to **app/database/migrations **you’ll see your time-stamped migration. Change the boilerplate they’ve given you in that file to the following:

```language-php
use Illuminate\Database\Schema\Blueprint; use Illuminate\Database\Migrations\Migration; class CreateUsersTable extends Migration { /** * Run the migrations. * * @return void */ public function up() { Schema::create('users', function(Blueprint $table) { $table->increments('id'); $table->string('first_name'); $table->string('last_name'); $table->string('email')->unique(); $table->string('username')->unique(); $table->string('password'); $table->string('remember_token')->nullable(); //required as of 4.1.28 upgrade for remember me $table->timestamps();//creates timestamps for created and updated at }); } /** * Reverse the migrations. * * @return void */ public function down() { Schema::drop('users'); } }
```

If you’ve never worked with Laravel before I’ll give you a brief explanation – this is a table builder, it will convert what you’ve put into the **up** function into proper SQL to build the table you’ve specified. The **down **function will be run when you run a migration refresh or rollback. So, now if we jump back over to the command line we can run

```language-bash
$ php artisan migrate
```

and create our table from the migration above. If it was successful you should see something similar to

```language-bash
Migration table created successfully.
Migrated: 2014_04_18_171339_create-users-table
```

And you should be able to see your new database table in phpmyadmin (if you’re using it).

### Seeding The Users Table

Ok, now that the table is setup let’s fill it with a user account for our example. To do this we need to create a database seed with Laravel. You probably already saw the folder when you went to view your migration – this time head to **app/database/seeds**. You’ll need to create a new file called UserTableSeeder.php inside that folder and add the following:

```language-php
 <?php class UserTableSeeder extends Seeder { /** * Run the database seeds. * * @return void */ public function run() { DB::table('users')->delete(); User::create(array( 'first_name' => 'Justin', 'last_name' => 'Voelkel', 'email' => 'justin@justinvoelkel.me', 'username' => 'admin', 'password' => Hash::make('password'); //hashes our password nicely for us )); } }
```

Again, just a simple explanation of what’s going on for you that may be new to Laravel. When this seed is run it will empty the users table if it already has data in it (remember that if you decide to reseed the table), then it creates a new user with the info we’ve provided. Note, that there’s already a Model for users (**app/models/user.php**) so we can use the create method. This is part of the boilerplate that Laravel gives you. If it were something like a post (hint: we’ll be adding these later) you’ll have to have a model setup for it before you can create new instances of it. We will save that for when we begin expanding the blog functionality – since this is only about authorization we don’t have to worry about it just yet. Also noteworthy, it’s important to never, ever, – _like, not even in a trillion years_ – store passwords as plain text. Laravel gives us sugar to make hashing our passwords really simple with **Hash::make()**. Now that the seed is setup we just need to tell Laravel to run it when the appropriate artisan command is given. To do that just open up **DatabaseSeeder.php** (still inside your seeds folder) and un-comment the following line:

```language-bash
 $this->call('UserTableSeeder');
```

This is already in there by default to go along with the User Model that they’ve included. If it were something else, say posts, we’d have to just copy this line and replace UserTableSeeder with the appropriate class (probably PostTableSeeder given our example). So now you can head back to your terminal and run the following:

```language-bash
$ php artisan db:seed
```

And, if everything went as expected you should see

```language-bash
Seeded: UserTableSeeder
```

Annnd voila – we’ve got a users table with our first admin user. Good job.

### Routes and Controllers

Now that the database is setup and seeded with all the info we need (for now) we need to set up some routes and controllers to control the flow of traffic between the frontend – the blog – and the backened – the admin area. Everything on ‘the backend’ is going to live under /admin while everything else will just use the root URI. We’ll start out by making a simple Index view that will end up as our front page and include the blog roll of all our posts. Let’s head over to **app/views** and create two new files –**index.blade.php **and** admin.php**. If you’re familiar with Laravel and have made use of Blade templating in the past just know we’re not using it for the admin views because it’s double mustache syntax {{ }} is the same as Angular’s and, we’ll be referencing data in the views through Angular anyway – long story short we won’t need it for our admin area. Go ahead and open **index.blade.php** and just throw in a quick placeholder message:

```language-markup
 <h1>Blog Goes Here</h1>
```

Odds are you already understand what’s going on so I won’t explain. Now, open up **admin.php**. Admin is going to, basically, be the shell we’re injecting our Angular views into. So, we will start with some boilerplate code:

```language-markup
 <!doctype html> <html lang="en">  <title>Blog Administration</title> <!--css--> <link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css"/> <!--js--> <script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script> <!--angular--> <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.js"></script> <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular-route.js"></script>   <h1>Admin Section</h1>
```

We’re just starting out adding some basic elements like bootstrap, jquery, and angular. Note that I’m not using the minified version of angular. The main reason for this is when angular throws a console error it will come out minified if you’re using the minified version. The errors are pretty verbose so it makes it extremely hard to pick apart – long story short, use the unminified version for development. You’ll notice below I’ve also included angular route, you’ll need to include this for routing as it is no longer packaged with angular’s core.

## Prepping Angular Within Laravel

Since we have included Angular we can begin setting up the essentials for Angular within our application. As I mentioned above all of our angular work will take place within the web facing **public** folder. To start just open up public and create a **js **directory. After you’ve done that open up **js** and create three more directories – **controllers, services,** and** templates. **This will help us separate our concerns when dealing with our angular code. Ok, one more addition – go ahead and create a new file called **app.js**. When you’re all done you’re structure should look like this:

```
-public/
  -js/
    -app.js
    -controllers/
    -services/
    -templates/
    -packages/
  .htaccess
  favicion.ico
  index.php
  robots.txt
```

I think I’m going to wrap things up here as this post has gotten pretty long. Now that the basic prep work has been done with Laravel and Angular Part Two is going to cover creating a login to our administration area. This is where we’ll really start digging in and coding with both frameworks and setting up our API. That post should be coming shortly – well shorter than the month it’s taken me to produce this! Stay tuned during the coming week.

## Part Two

It took some time but I managed to post part two.
[Check it out](http://justinvoelkel.me/laravel-angularjs-part-two-login-and-authentication/ 'Laravel and AngularJS: Part Two – Login and Authentication')
