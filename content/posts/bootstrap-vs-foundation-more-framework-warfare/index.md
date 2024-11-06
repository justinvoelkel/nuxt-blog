---
title: Bootstrap vs Foundation - More Framework Warfare
date: 2014-01-16T22:34:48.000Z
description: Bootstrap vs Foundation - More Framework Warfare
---

[![Bootstrap vs Foundation](http://justinvoelkel.me/wp-content/uploads/2014/01/bootstrap-vs.png)](http://justinvoelkel.me/wp-content/uploads/2014/01/bootstrap-vs.png)

A while back I gave my opinion on a couple of [php frameworks](http://justinvoelkel.me/wonderful-world-php-frameworks-cakephp-laravel/ "The Wonderful World of PHP Frameworks: CakePHP and Laravel") – cakePHP and Laravel. Welp, I’m back to kick another hornets nest so hold on to your butts. This time it’s front-end frameworks – [Twitter’s Bootstrap](http://getbootstrap.com/ "Twitter Bootstrap") and [Foundation](http://foundation.zurb.com/ "Foundation by Zurb").


## Framework Shaming

I wanted to quickly touch on this subject so bear with me. The more I read and dig the more I hear how uber un-cool it is to use frameworks for anything but rapid prototyping. Really? I get the feeling that folks using front-end frameworks for production sites are looked down upon and I really just don’t understand why. I don’t see the shame in using a good frame-work to lay the framework for your design.

Look, I get it, if a client is paying you thousands of dollars for something totally customized from mobile up to the desktop, you are probably better off just starting with a mobile first approach and working your way up so you can get things exactly how you want them. But, if your client is working on a hard dead line, if your client is working on a tight budget, if your client is in need of a very simple responsive site, if your client is you and you’re as impatient as I am – there is no shame in using a framework. Sure, it’s great for prototyping too but if the situation dictate you should feel no shame in using it as a production model.


## What’s The Diff?

There are major differences from a technical standpoint and I’ll touch on those in a moment. But, to me, the major differences are philosophical. If you are looking at these frameworks from a *‘what’s the workflow’* approach, they’re very different.

Bootstrap encourages you to take this beautiful ‘frame’ and put it up, and change colors, and throw in some pictures and text, and *maybe* move some boxes around but **THAT’S IT**. Bootstrap is less accessible in the wholesale changes department and, for that reason, templates become far more valuable. Now, that is a whole bucket of awesome-sauce for rapid releasing a project – it’s less fantastic if you want your site to be unique. In fact, in a recent episode of the [Shop Talk Show](http://shoptalkshow.com/ "Shop Talk Show Podcast") podcast, Dave Rupert made reference to this exact scenario during his work on [The Accessibility Project](http://a11yproject.com/ "The Accessibility Project"). He felt having the site up and running was more important than wasting lots of time on design and front-end development so he just went ahead with a Bootstrap theme. As I said before, if the project dictates it, there is no shame in just using a framework.

So, ok, now you’re asking – how is Foundation *philosophically* different from Bootstrap? Well, IMHO (the kids say this), Foundation is less of a rapid prototype/release framework and more of a responsive framing system. What the heck does that mean? Well, yes, Foundation has a few templates – but – they’re *extremely* basic. They’re more of a responsive, in browser, wireframe, done up in several popular layouts. Now, you could use one of these stock and just fill in the blanks, but they’re really meant to be more of a starting point. Where Bootstrap can be an out of the box solution with very little editing, Foundation gives you the tools to build your own solution. Foundation is ***much*** more flexible when it comes to changing layout or building your own layout. There is a small learning curve but once you have the syntax down it becomes very easy to build most any layout you can imagine.


## LESS vs. Sass

This is a whole different hornet nest that I’ll save for a later date. It’s actually more contentious than any of the framework wars going on right now – so much drama. Anyway, to me, as a guy who is more interested in scripting and backend engineering, it doesn’t matter much. The major rub here is the preprocessor and your operating system. Sass runs on [ruby](https://www.ruby-lang.org/en/downloads/ "Get Ruby") and, since ruby is already installed on Mac OS, it lends itself to developers who use Mac’s and something like CodeKit to compile it. Not to say you cant use PrePros (or any other preprocessor) and Windows but you’ll have to go through the trouble of installing ruby (which honestly isn’t a big deal). On the opposite side, LESS is javascript based so it’s really platform agnostic. I don’t really think there is a winner here, it’s more based on taste.


## Responsiveness

Despite the technical and philosophical differences – this is where Foundation pulls away in my opinion. Bootstrap has a much more rigid set of breakpoints than Foundation. Bootstrap has three major breaks for large, medium, and small screens and things do not flow well between those breakpoints. Without getting overly technical this is because Foundation has a flexible grid system. You don’t need to know the specifics, but you should appreciate the fact that Foundation will fit a wider array of device screen sizes, better. This can be considered knit picking but with the huge, and ever growing, pool of screen sizes, I believe Foundation site’s will age better than Bootstrap sites.


## Bells and Also Some Whistles Too

When it comes to aesthetic goodies like buttons, tooltips, sliders, and modals I’d give the edge to Bootstrap. Bootstrap is just really beautifully done, there’s no getting around it. Again, Foundation gives you all of the above in a nice, easy to use, package that can be easily changed. But, out of the box, Bootstrap is your champ here.


## Community Support

As of this writing Bootstrap gets the edge but thing are beginning to tilt in favor of Foundation. Both frameworks are well supported and updated often. Bootstrap has the considerable contribution of Twitter backing it which probably will always lend it to a larger crowd. But, with that said, the growth of the Foundation community over the past year has been noteworthy. I’ve found the forums for Foundation to be OK, and the documentation to be fantastic.  On the flip side, Bootstrap does both well.


## Conclusion

You probably will have guessed by now that my choice is Foundation. But, I don’t think you’re going wrong if you use either. As with most arguments, it is all about what the project dictates. I love Bootstrap for being the pretty girl at the party who’s really easy to talk to – and I love Foundation for being the nerdy girl at the party who will teach you a thing or two. My suggestion – try both. The worse case scenario is you learn what you don’t like. Then you can come here and tell me all about it!


