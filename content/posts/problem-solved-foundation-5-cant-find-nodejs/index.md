---
title: Problem Solved - Foundation 5 Can't Find NodeJS
date: 2013-12-17T11:09:14.000Z
description: Problem Solved - Foundation 5 Can't Find NodeJS
---

It’s late here. Like ‘whoa crazy crazy’ late for me – aka anything past 8pm – but, I digress…

<div class="wp-caption alignright" id="attachment_192" style="width: 310px">[![Zurb's foundation Yeti](http://justinvoelkel.me/wp-content/uploads/2013/12/yeti-business-300x166.png)](http://justinvoelkel.me/wp-content/uploads/2013/12/yeti-business.png)Seriously, this little dude is awesome. Who doesn’t love Yeti’s?

</div>Tonight I got the sudden urge to finally try out Compass (Sass) with my favorite front end framework, Foundation 5. I actually have a LAMP stack running at home with Ubuntu (KDE ‘cuz im weird like that) 13.10. If you check out Foundation’s documentation on the subject it says there are a few requirements to make this work:

1. Git
2. Ruby 1.9+
3. NodeJS
4. Bower

NBD right? Fo’ sho….Yeah it’s late. So I had ruby and git installed – we can take those off the list. Everything else you can apt-get install. Everything was running pretty smoothly until I entered my www directory all primed and ready for some Foundation 5 awesomeness. According to the good people at Zurb I merely needed to:  

<code>
$ foundation new yourprojectname
</code>

 This should spit out a new directory with the name of <project name> (insert your own cool project name – like Tina or Operation Cuddlefluff…whatever you fancy). My problem was, well, it didn’t do that. In fact it was all up in my face telling me it couldn’t find NodeJS. Which is weird because I literally typed ‘sudo apt-get install nodejs’ like fourteen-ish seconds before that. As usual, when in trouble, google is my best friend. After a lengthy search I realized I wasn’t finding much of anything besides someone else with the [same problem as me](http://foundation.zurb.com/forum/posts/571-foundation-can't-find-nodejs "Foundation can't find NodeJS"). The actual error is ‘Can’t find NodeJS. You can install it by going here: http://nodejs.org’.

After checking versions of things I noticed something – my path variable was ‘nodejs’ and not ‘node’. When running or checking version I could not access node by typing ‘node’ but had to in fact type ‘nodejs’. As one of the dependencies it’s my guess that foundation is referencing node by ‘node’ and not ‘nodejs’. Following through on my hunch I found [this on stack overflow](http://stackoverflow.com/questions/18130164/nodejs-vs-node-on-ubuntu-12-04 "Symlink nodejs to node"). I had to create a symlink to be able to use node to access nodejs:

<code>
$ sudo ln -s /usr/bin/nodejs /usr/bin/node
</code>

It turned out my hunch was correct and this did work. I’m guessing this may be a niche problem now but as wider adoption of Foundation 5 occurs it might become more of a problem. I’ve got no idea if this an issue on other versions/flavors of Linux or MacOS but I’m guessing that, regardless, this would work as a solution on other platforms as well. But, remember, use at your own risk.


