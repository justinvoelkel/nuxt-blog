---
title: Performance - Loading Javascripts
date: 2013-11-02T00:06:31.000Z
description: Performance - Loading Javascripts
---
Optimizing performance has become something of a healthy obsession lately. It started earlier this year when I first got myself the [page speed plugin for firefox](https://developers.google.com/speed/docs/insights/using_firefox "Google Page Speed for Firefox"). Knowing that Google has been putting increasing emphasis on page load times prompted me to dig a little deeper. My company also was knee deep into wire framing a total redesign for our website. So, for me to be able sit down and see the current, and not horribly well done, site’s limitations before moving to the next phase of development was priceless. It helped me to work on things like making sure keep alive was enabled in our new apache settings, optimization of images, setting caching and gzip compression, and a slew of other items that I wasn’t already aware of. It was a great guide to step through and fix the problems in our new templates before they hit production. I had increased our scores from the old site, which was in the 70/100 area, to 90+.  Watching those numbers go up is fiendishly  gratifying.

Then, out of the blue, I got an email from our SEO specialist a few weeks ago citing our Google Page Speed Insights score. I was feeling pretty confident in what I was going to see but checked out the link anyway. I was instantly disappointed in what I saw. Our scores, which now included mobile (MOBILE EVEN!!1!), were quite a bit lower than I had left them in the past. The biggest bug-a-boo right at the top of the list – Eliminate render blocking javascript and CSS with a big ol’ red exclamation point. Boy, I hate red. So where to go from here? Google, obviously.


## Javascript Google’s Way

You can check out their suggestions for solving this problem [here](https://developers.google.com/speed/docs/insights/BlockingJS "Google Render Blocking Js"). If you’re too lazy to read, in a nutshell, this is the code they offer as the solution:

```javascript
\\Add a script element as a child of the body function 
downloadJSAtOnload() { var element = document.createElement("script"); element.src = "deferredfunctions.js"; document.body.appendChild(element); } // Check for browser support of event handling capability if (window.addEventListener) window.addEventListener("load", downloadJSAtOnload, false); else if (window.attachEvent) window.attachEvent("onload", downloadJSAtOnload); else window.onload = downloadJSAtOnload;
```

The comments kind of explain it all but this works as a cross browser catchall for adding a window load event listener and then firing a function to append the body with script elements. This should be done keeping in mind that you should not defer loading anything that is required for elements above the fold. So, if you have some ajax to pull content etc. you should probably leave it be. As is, just about everything we were using could be pushed until after page load without much degradation in user experience.

I was well into testing Google’s solution when I came across some interesting reading from [Jake Archibald over at HTML5Rocks.com](http://www.html5rocks.com/en/tutorials/speed/script-loading/ "Script Loading Tutorial"). Brace yourself with that link it’s a pretty long read. But, it more fully describes the problem and several solutions of deferred script loading. He did a great job of outlining the pro’s and con’s of each solution including the fact that the inclusion of defer and HTML5 async aren’t smoking gun solutions to the problem. After the brain meltdown of reading through all these options I figured I’d divert from Google and try out Jake’s solution. Again, if you’ve skipped the article this was the code:
```javascript
 !function(e,t,r){function n(){for(;d[0]&&"loaded"==d[0][f];)c=d.shift(),c[o]=!i.parentNode.insertBefore(c,i)}for(var s,a,c,d=[],i=e.scripts[0],o="onreadystatechange",f="readyState";s=r.shift();)a=e.createElement(t),"async"in i?(a.async=!1,e.head.appendChild(a)):i[f]?(d.push(a),a[o]=n):e.write("<"+t+' src="'+s+'" defer></'+t+">"),a.src=s}(document,"script",[ "//other-domain.com/1.js", "2.js" ])
```
That’s all minified and shortcutted so if you want the full lowdown please [visit the article](http://www.html5rocks.com/en/tutorials/speed/script-loading/ "HTML5 defer js loading"). This actually appends the scripts to the head but can adapted for the body by changing e.head.appendChild to e.body.appendChild. I spent an afternoon offloading our current scripts and testing. This process alone was enough to bump our desktop score back to where we were originally and make our mobile score much more respectable.


## Conclusion

Like a lot of things in development, the premise is simple and the implementation is complicated. This is something anyone starting web development should know from the start. When I began developing sites I was always told to just move non-essential scripts to the footer and that was enough. It’s great to know that there is a step beyond that and, that it will pay dividends in both your page load times and search placement.

The one thing that really concerned me though, was thinking of all the wordpress site’s I’ve created over the years. Most of that Javascript is loaded in the header and not deferred. In fact, just plugging a few of my older sites into the [testing tool](https://developers.google.com/speed/pagespeed/insights/ "PageSpeed Insights testing tool") yielded pretty awful results. I think I’ll save that for a follow up post but it’s certainly something to keep an eye out for.


