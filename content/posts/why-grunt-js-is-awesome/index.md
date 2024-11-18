---
title: Why Grunt is Awesome
date: 2014-01-10T03:28:56.000Z
description: Why Grunt is Awesome
---

<div class="wp-caption alignright" id="attachment_225" style="width: 190px">[![Grunt JS](http://justinvoelkel.me/wp-content/uploads/2014/01/gruntjs.jpg)](http://justinvoelkel.me/wp-content/uploads/2014/01/gruntjs.jpg)War Pig!

</div>I’ll admit it – until about a month ago I had no idea what the hell Grunt did. I kept hearing [Dave Rupert](http://daverupert.com/ "Dave Rupert") and [Chris Coyier](http://chriscoyier.net "Chris Coyier") reference it on the [Shop Talk Show podcast](http://shoptalkshow.com/ "Shop Talk Show Web Development Podcast") and, slowly, I picked up little bits and pieces of what it was all about. After recently installing NodeJS on my server to get a [Ghost blog up and running](http://justinvoelkel.me/setting-ghost-blogging-platform/ "Getting Started on The Ghost Blogging Platform") I decided that I should give Grunt a go. The real kicker was Chris Coyier’s write up at 24ways – [*Grunt for People Who Think Things Like Grunt are Weird and Hard*](http://24ways.org/2013/grunt-is-not-weird-and-hard/ "Chris Coyier on Grunt JS"). That will walk you through the baby steps of getting up and running and will probably provide an ‘a-ha’ moment or two, I can’t recommend reading it enough.

## What Is Grunt? (in human speak)

Directly from their homepage – Grunt is a javascript task runner. Oh…of course… If that doesn’t mean much to you don’t worry, I think that’s a common reaction. While Grunt _is exactly this_, what it **_DOES_** is the real revelation. Most developers work flows involve a certain amount of optimization of production websites. That list typically would involve, but not be limited to:

- Image optimization – reducing the weight of images to improve mobile performance.
- CSS and Js concatenation – combine all your nice bite size scripts.
- CSS Minification – compressing styling weight by removing excess white space.
- Js Minification – see above.
- HTML minifcation – see above.
- Compile Sass or Less

All of those things have their own ways of being accomplished. For me, I was running images through something like [RIOT](http://luci.criosweb.ro/riot/ 'Riot image optimizer') or [CodeKit](http://incident57.com/codekit/ 'Code Kit') on a mac. CodeKit and, I believe, programs like [PrePros](http://alphapixels.com/prepros/ 'PrePros ') can do the concatenation and minification. As you can imagine (or don’t have to imagine because you suffer through it in practice) the process of making changes is a pain in the ass. Depending on your current work flow, making small modifications to CSS could require three or four steps just to move from development to production. This is the problem Grunt aims to solve.

## So, What Does Grunt Do?

Well, take those work flow processes listed above and roll them into a single command and that is what Grunt does. This was hard for me to come to grips with, my reaction was – ‘yeah, right’. But, yeah, it was right. Grunt makes use of plugins such as uglify, cssmin, imagemin, and bevy of others, to bang out that list above in one command (if you wish).  You can install those plugins individually and run them individually, or you can run them as part of your ‘master’ Gruntfile. If you choose the latter, you just define the processes you’d like to run and the files you’d like run them on inside the Gruntfile.js, upload it, and, inside of a shell, run grunt from your directory. Boom, done. I know that sounds complicated – it’s not.

## How Do I Do It?

I’m not going to go into a lot of depth here, I feel like [Chris’s write up](http://24ways.org/2013/grunt-is-not-weird-and-hard/ 'Chris Coyier on Grunt JS') does a great job of walking you through everything and that would be my recommended reading. None the less, I’ll provide you with a few brief nuggets from that article here.

First and foremost, you have to have NodeJS installed. The process for this can vary depending on your flavor of server so I’ll leave that step to you. After you’ve gotten that done you need to install grunt. To do this create a package.json file in the root directory of your site (locally or on the server, doesn’t matter for now). The contents should look like this (modified from Chris’s article):

````
{<br></br>
"name": "your-project-name",<br></br>
"version": "1.0", //your given version number<br></br>
"devDependencies": {<br></br>
"grunt": "~0.4.1" //the version of grunt you'd like - check the site for this number<br></br>
}<br></br>
} ```

If you’ve done that locally, upload it, if you did it on the server, you’re good to go. This just tells NPM (the node package manager) what you want. You should be able to now run `npm install `from your shell. That will go out and grab Grunt, and it’s dependencies, and install it for you. After completion make sure you also run `npm install -g grunt-cli`. That will install the grunt command line interface so you can run things with the grunt command. That’s it to get started.

Grunt depends on plugins to do it’s awesomeness. I mentioned a few earlier (cssmin, imagemin, uglify) but you can see a more comprehensive list [here](http://gruntjs.com/plugins "Grunt JS Plugins"). Install of those plugins needs to be done, again, through npm. Big surprise right? (modified from Chris’s article)

````

<br></br>
npm install grunt-contrib-pluginName --save-dev<br></br>```

That will update your dependencies automagically while also installing the plugin. When you run the `grunt` command it turns to your Gruntfile.js for instruction. That’s how you tell Grunt what plugins to use and how to use them. Below is a simple example (also modified from Chris’s article):

```
module.exports = function(grunt) {

grunt.initConfig({
pkg: grunt.file.readJSON('package.json'),

yourPlugin: {
//yourPlugin config goes here. Generally, you can find good examples on the github page
//for your plugin.
}

});

grunt.loadNpmTasks('grunt-contrib-yourPlugin');

grunt.registerTask('default', ['yourPlugin']);

};
```

Substitute yourPlugin for cssmin or any plugin you have installed. I usually just do all of this locally and send it up through FTP – but, if you’re a fan of vi feel free to do it in your shell. After all this you’re finally able to run `grunt` and your plugins should be run. If you run into syntax problems make sure you’ve correctly terminated lines with commas. Being acclimated to PHP I made several mistakes adding semicolons that led to syntax errors. No harm, no foul – fix it and move ahead.

## Conclusion – Why Grunt Is Awesome

Grunt is the most powerful tool, from an optimization standpoint, that I’ve seen in quite some time. Jump on board while things are still pretty fresh because I’d venture to say Grunt will be around for awhile. The benefits to your work flow are undeniable.
