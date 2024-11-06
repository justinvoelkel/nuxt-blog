---
title: Basic Website Optimization with Gulp
date: 2016-12-13T21:43:49.000Z
description: Basic Website Optimization with Gulp
---
In the past couple of years we've seen an explosion in the importance of basic website optimization techniques. With search engines like google placing more and more emphasis on speed and measurable metrics, optimizing your site has become more important than ever. I figured we could take a look at some of the basics of optimization and I could show you how to leverage a JS task runner like Gulp to handle a lot of these things quickly and efficiently.

I'm going to set up a public github repository that you can use to see all of these source files and fork or clone them for your own personal use. That repo can be found here:

[Gulp Basics Github Repo](https://github.com/justinvoelkel/site-optimization-gulp-basics)

There are a few prerequisites to get out of the way before we jump into this.
 
###Things You'll Need
1. Terminal with git or [git bash on windows](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) (if you have git on your machine you have git bash)
2. [Node/npm installed](https://nodejs.org/en/download/)
3. An IDE

That's really it. It would be nice to have a local development environment like wamp/mamp, vagrant, or docker but it's not necessary as we'll just stick to html extended files so we can open them up in the browser and see the results.

###Important Files
The crux of what we're going to talk about is going to be centered around a couple of main files. Those would be your `package.json` and `gulpfile.js` files. These two work hand in hand in defining the dependencies for your project and carrying out your optimization orders respectively. 

####package.json
Just to touch on this quickly if you're new to this - a `package.json` file is used to define dependencies for your project. Gulp is my js taskrunner of choice (more to come on that) and is a good example of one of the dependencies this project requires. Gulp relies on a handful of other packages to do things like minify files, concatinate files, and compile sass (among other things). Basically, it tells `npm` (the node package manager) to go out and download a bunch of stuff for you. The premise is similar to a `bower.json` (front end assets) or `composer.json` (php) file if you're more familiar with those.

####gulpfile.js
I mentioned above that the main dependency for this project is Gulp. Quickly, Gulp is a javascript task runner - don't over complicate that title it's exactly what it is; you define tasks for gulp to do (minify, compile, etc.) and it just does them. Those tasks are defined in the `gulpfile.js` file and we'll spend a lot of time walking through these. There are other options, namely [Grunt](http://gruntjs.com/), for your js task runner needs but I prefer to use Gulp for a few reasons beyond the scope if this post.

###Foundation
Literally and figuratively - I'm using [Zurb's Foundation framework](https://github.com/zurb/foundation-compass-template) as the boilerplate files for doing all of our special optimization stuff. We'll be compiling the scss into css, combining and minifying our css and js into dev and production files, and we'll do some image minification for good measure. 

**Note: for the Foundation files, I've just cloned the repo locally and ran the proper commands to pull in the dependencies then extracted the js and pertinent scss files into my own directories.**

##Getting Going
First thing is first - you'll want to fork, clone, or download the repository that accompanies this post: https://github.com/justinvoelkel/site-optimization-gulp-basics.git 

Once you have that pulled down locally you should be able to open up that file locally and open `index.html` in your favorite web browser. With any luck you'll see this:

![foundation boilerplate](/content/images/2016/12/Screen-Shot-2016-12-12-at-1.31.21-PM.png)

Ok, not horribly impressive but let's take a peek behind the curtain and see how we're optimizing the few assets that are going into this page.

##Basic File Structure
####development
How you structure all of this is really up to you but I prefer to split the js and css directories into production and development files. I keep multiple, un-optimized files in the `development` folder. 
![](/content/images/2016/12/Screen-Shot-2016-12-12-at-3.59.10-PM.png)
This allows me to keep everything nice and modular in multiple files. There is nothing worse than having to sift through giant js files for a small fix - this keeps things nice and organized. Same with the css, this allows you to write very compartmentalized css files that address specific parts of pages (nav.scss, footer.scss, sidebar.scss, mobile.nav.scss etc.)

####production
After we run gulp to do our optimization tasks we want to output nice clean production files. We'll target the `production` folder for those optimized files, and these will be the files you would traditionally serve up in your production environment as they are the most streamlined versions you have.
![](/content/images/2016/12/Screen-Shot-2016-12-12-at-4.16.22-PM.png)
**Note: The `dev.css` and `dev.js` files are un-minified versions of the production files. These are best used for development environments. If something goes wrong (especially with js) it's tough to diagnose with browser developer tools when you're using a minified file. Using the un-minified versions will give you true line numbers for debugging both css and js.**

##Dependencies
I had mentioned above that we manage our dependencies in the `package.json` file. Let's walk through it: 
```javascript
{
  "name":"gulp-basics",
  "description":"A basic example of how to install gulp and perform site optimization techniques.",
  "author":{
    "name":"Justin Voelkel",
    "email":"justin@cinpleweb.com"
  },
  "devDependencies": {
    "gulp": "^3.9.0",
    "gulp-concat": "^2.6.0",
    "gulp-imagemin": "^3.1.0",
    "gulp-clean-css": "^2.2.0",
    "gulp-rename": "^1.2.0",
    "gulp-uglify": "^2.0.0",
    "gulp-watch": "^4.3.0",
    "gulp-sass":"^2.3.0"
  }
}
```
If you're familiar with json or js objects this isn't too daunting. The first bit at the top is just basic meta information; the project name, a description, and some author info. The meta info mostly comes into play if you put something up on [npmjs.com](https://www.npmjs.com/).

We care most about `devDependencies`. This is a naming convention that we need to use to keep our development dependencies separate from production dependencies. Since we'll optimize locally and deploy the optimized files we don't need to worry about production dependencies. If you were building something that did have production dependencies you would list them as normal `dependencies`.

Each entry under `devDependencies` is a package that can be found at [npmjs.com](https://www.npmjs.com). On the left, you have the package name `gulp`, `gulp-concat`, `gulp-imagemin` etc. On the right you have the package version you want. 

You'll notice the `^` in front of the version numbers - this is the less strict versioning definition. Packages are typically updated to address bugs or to add features and they adhere to semantic versioning (`major`.`minor`.`patch`). Using the `^` will allow you to run `npm update` and packages will be updated to the newest `minor` version. If you used `~` the update would only update the package to the latest `patch` version. According to semantic versioning only `major` updates should include breaking changes but just be aware that sometimes mistakes happen and things slip through.

Ok, so what do all these things do? You can look up of these packages up on npmjs.com but lets define what they'll do in terms of our project. In order:

* `gulp` - our js task runner
* `gulp-concat` - concatenate indv files into one big one
* `gulp-imagemin` - minify images
* `gulp-clean-css` - minify css
* `gulp-rename` - rename files
* `gulp-uglify` - minify js
* `gulp-watch` - watch for changes in our css and js `development` directories
* `gulp-sass` - compile `.scss` files into `.css` files

Now that we know what everything does - how about we actually install them? Access your project root from your terminal or git bash (or equiv unix-like env) on windows. From here you can run `npm install`. If everything goes to plan npm should create a new directory called `node_modules` - this is where all of our local dependencies will live.

**Note: If you run into issues you're likely not alone. I love node and npm but they have their own quirks and they can fall over sometimes. If you run into errors a log file should be created that will allow you to see the description of the issue. 99% of the time it's an issue someone else has encountered so google is your friend.**

So, with our dependencies now installed, we can move on to our gulp optimization tasks.

##Gulp Tasks

At this point let's have a look at `gulpfile.js` section by section.
####package definitions
```javascript
var gulp        = require('gulp');
var minifyCSS   = require('gulp-clean-css');
var concat      = require('gulp-concat');
var uglify      = require('gulp-uglify');
var imagemin    = require('gulp-imagemin');
var rename      = require('gulp-rename');
var sass        = require('gulp-sass');
```
Here, we're assigning our dependencies to variables for use later on in our tasks. No real magic here - just pick something descriptive and succinct. 

####styles tasks

```javascript
gulp.task('styles',function(){
    var stylesSrc = [
        './styles/development/scss/**/*.scss'
    ];
    return gulp.src(stylesSrc)
        .pipe(sass())
        .pipe(concat('dev.css'))
        .pipe(gulp.dest('./styles/production/'))
        .pipe(rename('prod.min.css'))
        .pipe(minifyCSS())
        .pipe(gulp.dest('./styles/production/'));
});
```
When creating new tasks we use the Gulp method `task` that accepts two main arguments. The first is the name of the task - in this case `styles` and the second is the callback for the task we want to address. In this case we're addressing everything we need to do relating to our style sheets. Later, if we run `gulp styles` from our terminal, this task will be called.

Other things to note here - starting from the top and working down:

* we define our source path with the variable `styleSrc`
  * I'm using an array here to illustrate that you can pass several paths or streams to gulp to do it's magic on - so if for instance we wanted things compiled in a very specific order we could define them in order in our array
  * You'll also notice the path is in a peculiar format `./styles/development/scss/**/*.scss` - this is a recursive path matching any file found under `./styles/development/scss` or it's descendant directories that has the file extension `.scss`.
* the `return` value is the task we want to perform with chained methods
  * the `src` method defines the paths or streams we want to make available
  * the `pipe` method is simply a way of chaining processes - no real need to look any deeper than that unless you want to
  * we start off by calling on `sass()` which if you recall from our definitions is the `gulp-sass` plugin. The plugin will take in the source from our `scss` files and compile the sass into `css` files.
  * we then use `concat()` to concatenate all of our new css files into one file `dev.css`
  * now that we have our compiled and concatenated file we store it in the production directory using the gulp method `dest`
  * now we just have to create our optimized production file - we use the `rename` method to create a copy of `dev.css` as `prod.min.css` and call the minification plugin with `minifyCss` - finally we store this new file in the production directory, again using the `dest` method

So, perhaps that was a little more detail than you needed but if you're new to gulp a little explanation goes a long way in what you're seeing. For the next few tasks I'll go a little lighter and just explain a few of the finer points.

####js tasks
```javascript
gulp.task('scripts',function(){
    var scriptSrc =[
        './js/development/vendor/**/*.js',
        './js/development/foundation/**/*.js',
        './js/development/app.js'

    ];
    gulp.src(scriptSrc)
        .pipe(concat('dev.js'))
        .pipe(gulp.dest('./js/production/'))
        .pipe(rename('prod.min.js'))
        .pipe(uglify())
        .pipe(gulp.dest('./js/production/'));
});
```

You'll notice a lot of similarities to our css task as we're doing much of the same - just concatenating multiple files and minifying the results (in this case `gulp-uglify` is the plugin we use to minify the js files). You will notice the source assignment makes use of the array of values this time. 

**Note: In javascript fall through is very important - if you define the order of your scripts wrong here it can definitely cause issues. Make sure you have things like jquery and other libraries defined first and other dependent scripts defined last.**

####images task
```javascript
gulp.task('images',function(){
    return gulp.src('./images/*')
        .pipe(imagemin())
        .pipe(gulp.dest('./images'));
});
```
This task is specifically designed to minify our images directory with the `gulp-imagemin` plugin. The only real important thing to note here is what we're doing with the optimized images. Some people have a directory for raw un-optimized files and output the optimized images to a production directory. I've opted to simply overwrite the contents of the images directory with the optimized images.

**Note: It may be in your best interest to output to a production directory especially if you are dealing with a large amount of images or don't have a backup. If the optimization plugin gets a bit too ambitious it can affect image quality. I've never had an issue personally but it is certainly worth noting.**


####watch and default tasks
```javascript
gulp.task('watch', function() {
    gulp.watch('./styles/development/**/*.scss', ['styles']);
    gulp.watch('./js/development/**/*.js', ['scripts']);
});

gulp.task('default', ['styles','scripts','images']);
```
We'll wrap up by looking at the `watch` and `default` tasks. 

The watch task makes use of gulp's `watch` method - this method takes two arguments, the file(s) you'd like to watch for changes (in our case the contents of the css and js `development` directories) and the task you want to run when there are changes (`styles` and `scripts` respectively). Remember `styles` and `scripts` are the names of our defined tasks above. The watch task allows us to run `gulp watch` from our terminal while were actively making changes and the optimization we outlined above will happen automatically.
![](/content/images/2016/12/Screen-Shot-2016-12-13-at-11.34.26-AM.png)
**some sample output of gulp watch**

Finally, we have the default task. This is simply to define what happens when you run `gulp` without any defined task from the terminal. In our case we're just going to run everything except watch. This way we can quickly and completely run the optimization tasks we need by default.
![](/content/images/2016/12/Screen-Shot-2016-12-13-at-11.38.39-AM.png)

##Conclusion
Hopefully this wasn't too much to take in a once. Looking at the word counter now this seems to have gotten a little long winded but I am hoping it's for the best if you were in need of a little more in depth explanation.

Gulp makes our lives so much easier in terms of optimization and to be honest this walkthrough is barely skimming the surface of things you can do. A few years ago I was manually minifying files will cli tools and optimizing images with a totally separate program and it was a total pain - this stream lines that process into something you never really have to think about if you don't want to. As always if you have questions, comments, or complaints please leave them in the comments below and thanks for reading!





