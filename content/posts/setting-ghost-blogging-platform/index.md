---
title: Getting Started on The Ghost Blogging Platform
date: 2014-01-05T10:29:24.000Z
description: Getting Started on The Ghost Blogging Platform
---

I run this blog on WordPress. No big deal, I’ve done hundreds of WordPress installs, I’ve created custom post types and themes – I’ve done plenty with WordPress. So, In the course of redesigning my freelance website ([cinpleweb.com](http://cinpleweb.com "Cinple Web Solutions") – depending on when you see this you probably won’t see anything cool) I decided to take a different approach. There has been some good pub going around the interwebs about [Ghost](http://ghost.org "Ghost Blogging Platform") so I decided to give it a go.


## What Is Ghost?

Ghost is a blogging platform, pure and simple. While platforms like WordPress and Drupal are striving to be fully fledged CMS solutions Ghost’s creators (including former WordPress.org folk) have pledged to create a dedicated blogging platform. Ghost can run on top of sqllite, mysql, and postgres – if there are more options forgive my omission. Unlike wordpress, Ghost requries NodeJS to run. This makes things a bit more complicated from an installation perspective but that will be addressed later. Lastly, I’ll just say that Ghost looks to be beautifully done and the marketplace for themes is starting to pick up some momentum. It appears as if the community is growing fairly quickly.


## What Can’t Ghost Do?

I figured I would throw this in just as a primer on what to expect moving forward. There are a few things that Ghost, apparently, cannot do quite yet. First, Ghost cannot be installed and ‘mapped’ to a sub directory. So, any dreams you had of dropping it into yourdomain.com/blog/ just went up in smoke. This is being worked on for a later release but there is no telling when that will come. Next, Ghost is not meant to be another CMS option. Ghost is strictly meant for blogging so if you’re interesting in a WordPress or Drupal replacement for a small website you are barking up the wrong tree. Last, there is not a plugin repository like there is for WordPress, not at this moment at least. That can be both a good and bad thing considering that a poorly written WordPress plugin can gum things up pretty badly – still it would be nice to have some options, especially for those not capable of doing full on edits to add functionality to their blog.


## How To Install Ghost

First off, don’t let anyone tell you installation of Ghost is just as simple as installing WordPress – it just isn’t. This can’t and shouldn’t be attempted unless you’ve got shell access to your hosting account which is typically rare if you’re on shared hosting. There are a few places like [Digital Ocean](http://digitalocean.com "Digital Ocean") that are now offering Ghost as a one click install option; that’s probably ideal for the power blogger but not so much for folks like myself. You can also get a hosted blog through Ghost themselves now as well. With that said, you’ll need a few things installed before you get going:

<div class="alert alert-info"><span class="alert-before"></span><span class="alert-after"></span><div class="alert-wrapper">**Heads up!** Note that I’m installing on my CentOS 5 VPS. These instructions may vary based on your environment.  
<span class="ico-st alert-close"></span></div><div class="clear"></div></div>- <span class="tooltip" title="It's important to know Node requires python 2.6 or higher. In your shell type python -V to see your version.">NodeJS and npm</span> – they come together so either just install Node from source (wget) or, depending on your flavor of Linux, you may be able to use a repository.[ Detailed instructions here.](https://github.com/joyent/node/wiki/installation "Install NodeJS")
- Download and install Ghost:

```
<br></br>
cd /directory/to/your/blog<br></br>
curl -O https://ghost.org/zip/ghost-latest.zip<br></br>
unzip -d ghost [Name-of-Ghost-zip].zip<br></br>
cd ghost<br></br>
sudo npm install --production<br></br>```

- Configure Ghost – You can do this by editing the config.js file much like wordpress. You should be able to keep mostly default values unless you have a preference on sql db (see options above). You’ll notice that when you open that file there is a development, testing, and production section all with their own settings. This is a huge win in my book for Ghost. You can actually enter different values there and have a dev and testing environment separate from your production area. Once Ghost is configured you’ll be able to switch between them by, first stopping Ghost, then, specifying npm start –[development/testing/production]. Super cool!
- Start Ghost – Just type `npm start --production` (or testing…or development).


## Setting Up a Reverse Proxy

If you visit your blog you will probably see raw javascript code. This confused the heck out of me at first.  It turns out that a reverse proxy needs to be set up in your virtual hosts.

<div class="alert alert-warning"><span class="alert-before"></span><span class="alert-after"></span><div class="alert-wrapper">**Warning!** Again, this needs to be done by someone with shell experience as you can nuke your hosting by messing with apache config file or vhosts file.  
<span class="ico-st alert-close"></span></div><div class="clear"></div></div>It should look similar to the following:

 <VirtualHost *:80> ServerName [optionalsubdomain.]yourdomain.com ProxyRequests Off ProxyPreserveHost On <Proxy *> AddDefaultCharset Off Order deny,allow Allow from all </Proxy> ProxyPass / http://127.0.0.1:2368/ ProxyPassReverse / http://127.0.0.1:2368/ </VirtualHost>


## Restart Ghost

Once you’ve setup the reverse proxy you need to restart ghost. Once you’ve done this you should see your blog up and running! You can setup your account and login by navigating to yourdomain.com/ghost.


## Conclusion

After spending a few days with Ghost from a user’s standpoint I am loving it. Since it’s running on Node it’s quite fast and, while the interface on the back end is simplistic, it serves it’s purpose extremely well. The real downside to Ghost is the setup. I’m fairly knowledgeable on these types of things and it was a struggle for me to get things up and running. If you’ve already got Node up and running or at the very least had an updated version of python (I didn’t) it’s a much less significant process. If you have the tools to set up a Ghost installation I’d certainly recommend giving it a try if only to try something new. Happy blogging!


