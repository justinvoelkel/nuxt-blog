---
title: Performance - The Battle of the Buldge
date: 2013-11-27T10:07:39.000Z
description: Performance - The Battle of the Buldge
---

<div class="wp-caption alignright" id="attachment_164" style="width: 310px">[![Homer Simpson](http://justinvoelkel.me/wp-content/uploads/2013/11/homer-fat-300x225.jpg)](http://justinvoelkel.me/wp-content/uploads/2013/11/homer-fat.jpg)I wonder how many kb a pink frosted donut is worth?

</div>As I mentioned in my previous post on [javascript loading](http://justinvoelkel.me/performance-loading-scripts/ "Performance: Loading Scripts") – page optimization has become a healthy obsession for me lately. In case you didn’t read that fascinating article it’s all about optimizing loading of javascripts and it’s effect on google page insight scores. After deferring those scripts till after document.ready I saw significant jumps in both the desktop and, especially, mobile scores. However, I knew there were some areas left for significant gains in that score. I read up quite a bit on page optimization and the growing concern over page weight, and, well, considering Thanksgiving is Thursday, what could be a better topic to write about?!

Chris Coyier conducted a poll over at [CSS Tricks](http://css-tricks.com/new-poll-ideal-page-weight/ 'CSS Tricks - Page Weight') about this very subject. The results were an absolute eye opener for me. The winning result of that poll was 500kb (KB!!1) with a more modest 1mb in second. With knowledge in tow and keyboard in hand, I headed over to our ‘widget’ site to weigh in. Well folks, I wouldn’t be writing a post about it if I was satisfied with the results. It was very much like that first trip back to the scales after a delicious two month food binge we like to call ‘the holidays’. It was enough to drive me to salad for lunch people! Ok ok – so problem identified, what is the solution? Luckily, there are several easy things you can do to lighten up and optimize delivery to those slower devices and connections.

## Image Optimization

For me, this was the area of largest gains. I had never dug this deep into optimization before so, when slicing my psd’s, I never considered image weight. Turns out high res png files are like really costly. I started by converting several images from png to gif format. This can be especially helpful for images that are greyscale or are super small like social icons. Reducing the color pallet and running them through [RIOT](http://luci.criosweb.ro/riot/ 'RIOT Image Optimization') (which is a fantastic tool by the way) helped me cut the footprint of my images by nearly half right out of the gate. Also worth noting is the practice of combining your images into sprites and using background position in your stylesheets. I’ve not gotten this far yet so I can’t really report on the gains but this technique is widely used.

## Condense and Minify CSS and JS

Ok, I know, right now your face is in your palm because this is so obvious. Well, this one was simply me being lazy. I knew minification was a best practice but I really didn’t think it was worth the time and hassle before this. Reducing the amount and size of http requests is absolutely huge in this process. Moving forward I’d like to start working with CSS preprocessors like SASS to make this easier but for now I just took all my scripts and ran them through [jscompress.com](http://jscompress.com/ 'Javascript Minification'). Make sure your loading as few files as possible – namely, one or two css files at most.

## Lighten Up Your Libraries

I didn’t even have to research this one, I knew right off the bat my jquery UI library was mostly unused. Our site uses some jquery UI items like auto complete and tooltips but aside from that the remainder of the library was expendable. If you head over to [jQuery UI ](http://jqueryui.com/download/ 'Jquery UI custom downloads')you can build your own custom download and cut out some significant weight. Also, again, make sure it’s minified and condensed.

## Optimize Delivery of Fonts

Using [WebPageTest.org](http://www.webpagetest.org/ 'Web Page Test') will clue you into some significant areas of optimization as well. In my case gzip compression was something that I had address during development but I had apparently missed for my opentype and truetype MIME types. By adding those MIME types and DEFLATE filters I delivered those costly font files optimally. This is a must add to an htaccess file for anyone planning to use @font-face which we all know is so wildly popular amongst the teenagers nowadays. See Below-

<IfModule mod_mime.c> AddType application/vnd.ms-fontobject eot AddType application/x-font-ttf ttf ttc AddType font/opentype otf AddType application/x-font-woff woff AddType image/svg+xml svg svgz </Ifmodule> <IfModule mod_deflate.c> AddOutputFilterByType DEFLATE image/x-icon image/svg+xml application/vnd.ms-fontobject application/x-font-ttf font/opentype </Ifmodule>

## Conclusion

After running through the list of optimizations above I managed to get my homepage weight down to about 1.1mb from around 1.9mb. Along with that came a ten point bump in our mobile page speed insights score from 75 to 85 and a score of 97 on desktops. It is worth noting that these took me a few days to implement, mostly because re-sizing and fiddling with the image files took forever. I learned a valuable lesson moving forward – always optimize during development or at least as a last step before launch. With the insane burst in mobile traffic and the growing popularity of heavy frameworks and libraries it’s extremely important for developers to remain cognizant of their page weights.

##
