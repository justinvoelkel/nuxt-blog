---
title: Site Optimization - Installing and Configuring mod_pagespeed with Cpanel
date: 2014-12-15T21:32:30.000Z
description: Site Optimization - Installing and Configuring mod_pagespeed with Cpanel
---

I’ve written a few posts here about my growing interest in site optimization. It seems as though that interest is coming at the right time as Google has begun to, and continues to, put more and more emphasis on page load times and performance metrics. Sites I maintain and develop for both freelance clients and my day job need to be optimized to squeeze every last ounce of performance out of them to show Google they mean business and to keep users happy. Apache’s mod_pagespeed is a great way to boost your site’s performance quickly with some basic configuration.

I do quite a bit of work on Digital Ocean droplets because they’re just so darn convenient. Installing mod_pagespeed on a basic install of Ubunutu is really straight forward – [find those instructions here](https://developers.google.com/speed/pagespeed/module/download 'Install mod pagespeed ubuntu'). However, I also have a few clients that have their own VPS packages complete with cpanel. Turns out cpanel muddies the waters a bit because the process outlined in that previous link will not work. In which case, the mod needs to be downloaded and unpacked via the command line and then installed through EasyApache in whm. That process can be intimidating so I thought I’d write a post and walk through it and outline some basic configuration.

## The Big Picture

We’ll attack a basic concept of site optimization – lighten up you page weights (payload) to lower your page load times. Think of a stock car being converted into a racing car – cut the weight, do some work under the hood, and you enhance the performance. Admittedly, there are other important performance metrics to consider like javascript performance, deferred loading, and non-blocking js/css but page weight is a good place to _start_ your optimization.

## Beginning at the End

Why do you want mod_pagespeed? Your website runs fine, right? Well, typically there are things tucked away that we can be doing better and there plenty of places to benchmark and get performance feedback on your site:

- [Google Pagespeed](https://developers.google.com/speed/pagespeed/insights/ 'Google Pagespeed')
- [GTmetrix](http://gtmetrix.com/ 'GT metrix')
- [Web Page Test](http://www.webpagetest.org/ 'Web Page Test')
- [Pingdom](http://tools.pingdom.com/fpt/ 'Pingdom site test')

You’ll probably see some variance in the scores of those sites but you’ll see, in general, that if you don’t perform well in one you don’t perform well in them all. You’ll see warnings and suggestions about several things but the main things we’ll focus on are compression and minification to lower our payload size and round trips. Google in particular will let you know when you have space that can be saved – either through minification or compression of one type or another.

## Install

A few assumptions I am making is that you have some sort of VPS hosting with a LAMP configuration and cpanel already up and working. If you don’t I can’t really help you too much but I will thank you for reading this all for no apparent reason. Also, that you have shell access to your server, if you’re on a shared host, regrettably,  none of this will work. You can find these exact same instructions [here](https://github.com/pagespeed/cpanel 'pagespeed for cpanel') – they’re pretty brief and this process can be intimidating the first time you do it so I’ll be throwing in some screen shots of my experience and adding any notes I think might help. Ok, let’s light this fire – ssh into your server (I’m being bad and logging in as root – don’t judge me)

1.Clone the installation scripts onto your CPanel server:

$ /usr/local/cpanel/3rdparty/bin/git clone https://github.com/pagespeed/cpanel.git /tmp/pagespeed/

![git-clone](http://justinvoelkel.me/wp-content/uploads/2014/12/git-clone.png)

2.Create Speed.pm.tar.gz

$> cd /tmp/pagespeed/Easy $> tar -zcvf Speed.pm.tar.gz pagespeed $> mkdir -p /var/cpanel/easy/apache/custom_opt_mods/Cpanel/Easy $> mv Speed.pm Speed.pm.tar.gz -t /var/cpanel/easy/apache/custom_opt_mods/Cpanel/Easy/ $> cd && rm -rf /tmp/pagespeed

<div class="wp-caption aligncenter" id="attachment_535" style="width: 810px">![unpack-pagespeed](http://justinvoelkel.me/wp-content/uploads/2014/12/unpack-pagespeed.png)Yeah I know…I didn’t add that pesky recursive flag to delete the directory – do as I say not as I do

</div>So, that is really *it* as far as cloning the mod source and positioning it to be used by cpanel. However, this is where some nail biting comes into play. Go ahead and login to whm on your host and search for EasyApache (It’s under software). It should bring up the following:

![easy apache step two](http://justinvoelkel.me/wp-content/uploads/2014/12/easy-apache-one-1200x506.png)

The **‘Previously Saved Config’** is what you’re running currently. This is what you’ll want to be editing – look to the right under **‘Actions’** and click the cog icon to edit settings. You can hit **‘next step’** for the ‘Apache Version’ and ‘PHP version’ unless you want to make any changes – we’re looking to get to the **‘Short Options List’**. If you’ve done everything right you should see mod pagespeed listed:

![mod pagespeed option](http://justinvoelkel.me/wp-content/uploads/2014/12/mod-pagespeed-option.png)

Check that checkbox and select **‘Save and Build’** and be prepared for a wait. A little back story as to what is going on here, if you don’t know, this process requires that apache and php be rebuilt and recompiled – **_fair warning, this process takes quite a long time_**. At this point, take a break, get a snack or a drink and come back in about 15 minutes and, if all went according to plan….

![easy apache done](http://justinvoelkel.me/wp-content/uploads/2014/12/easy-apache-done-1200x791.png)

At this point maybe take a minute or two to whiz around a few of your websites to make sure that everything is still up to snuff and working correctly. Odds are this went off without a hitch but it never hurts to be sure.

## Configuring

Assuming you’re still logged into your server find your way to **_/usr/local/apache/conf/_** and you’ll find your **pagespeed.conf** file just waiting for some sweet, sweet, configuration action. Let’s knock these out one by one based on my original score – first we’ll start with optimizing images.

<div class="alert alert-notice"><span class="alert-before"></span><span class="alert-after"></span><div class="alert-wrapper">**Notice this:** I am editing the server pagespeed.conf file – this will affect and be applied to all websites on the server. If you want to override that configuration or configure each site on it’s own you can put these filters into the .htaccess file.  
<span class="ico-st alert-close"></span></div><div class="clear"></div></div>Inside of your pagespeed.conf file you’ll want to find the section for enabling filters – we can put these filters anywhere but, in the interest of keeping things tidy you’ll want to put them here. Look for the line starting **‘#Explicitly enables specific filters…..’**.

### Images

To compress and resize images we’ll use the[ rewrite images](https://developers.google.com/speed/pagespeed/module/filter-image-optimize 'mod pagespeed rewrite images') filter –

**ModPagespeedEnableFilters rewrite_images**

### CSS

To minify our css we’ll be using the [css rewrite](https://developers.google.com/speed/pagespeed/module/filter-css-rewrite 'css rewrite') filter (we’ll just add a new line) –

**ModPagespeedEnableFilters rewrite_css**

But, we’re not doing the best we can do here. Why not combine and minify? Let’s add the[ css combine](https://developers.google.com/speed/pagespeed/module/filter-css-combine 'css combine filter') filter –

**ModPagespeedEnableFilters rewrite_css,combine_css**

Also, the [sprite images](https://developers.google.com/speed/pagespeed/module/filter-image-sprite 'pagespeed sprite images') filter has the potential for big performance gains and it’s worth an add. As of this writing this filter only works on background images that are gif or png format but this is scheduled to be expanded.

**ModPagespeedEnableFilters rewrite_css,combine_css,sprite_images**

### Javascript

Here, we’ll want to combine and minify the javascripts just like we did with the css –

**ModPagespeedEnableFilters rewrite_javascript,combine_javascript**

Seeing a pattern?

### HTML

Finally, let’s[ minify html](https://developers.google.com/speed/pagespeed/module/filter-whitespace-collapse 'pagespeed collapse whitespace') and [remove comments](https://developers.google.com/speed/pagespeed/module/filter-comment-remove 'pagespeed remove comments'). Both will help reduce the over all page weight by removing unnecessary characters.

**ModPagespeedEnableFilters collapse_whitespace,remove_comments**

## The Conclusion Part

The filters I added above cover a fairly basic setup,[ there are a ton of other filters to take advantage of](https://developers.google.com/speed/pagespeed/module/filters 'google mod pagespeed filters'). Also, you’ll want to enable things like deflate and gzip and setup your browser caching to take full advantage of mod pagespeed. That step of setting up browser caching will also help significantly in improving your performance scores if you haven’t already done it. Also, I’ve come across several blog posts and articles about configurations that will get your pagespeed score up to 100 out of the box – I’m not sure if those were dated or what but, while they did increased my score, it wasn’t to 100/100. Just know that your site(s) configuration is something you should test and retest until your satisfied with your page metrics and your scores.
