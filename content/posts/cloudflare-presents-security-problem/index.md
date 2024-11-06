---
title: CloudFlare Presents a Security Problem
date: 2014-01-30T21:40:00.000Z
description: CloudFlare Presents a Security Problem
---

<div class="wp-caption alignright" id="attachment_246" style="width: 310px">[![I has dumb](http://justinvoelkel.me/wp-content/uploads/2014/01/i-has-dumb-300x189.jpg)](http://justinvoelkel.me/wp-content/uploads/2014/01/i-has-dumb.jpg)Not related – but c’mon, it’s hilarious.

</div>Like most, I make it a priority to keep both my and my client’s WordPress sites and blog safe and secure. That means a standard set of plugins for backups and WordPress hardening. Amongst those plugins I use Better WP Security quite often for hardening. It’s kept up pretty well and it gives you a nice color coded list – Yoast style – on things you should improve to improve security on you WordPress install. Anyway, this isn’t a sales pitch for that plugin, but, some of the options it does give you is to log 404 activity and failed login attempts. The 404’s are particularly helpful because there are actually lists of vulnerable WordPress url’s out there and [Google dorking](http://blog.k3170makan.com/2012/01/science-of-google-dorking.html "Google Dorking Defined") has become a popular means of finding vulnerable sites – so it gives you a means of judging if people are being linked incorrectly or being malicious. But, ok, TO THE POINT! For me, the failed login log’s are a priceless feature. When someone fails a login attempt it will store the attempted username, IP it came from (you can only see this in the DB btw – I don’t get that), and timestamp.

The real golden nugget in there is the IP address – especially if you have the means to go in and blacklist IP’s in your server. Is that going to stop someone who’s really motivated? No – of course not. But, you can make it more of a headache for someone so it can be a helpful deterrent. So, according to my count, I’m like 250 words in and I’ve not mentioned CloudFlare and how it’s a problem. Well, a large client of mine runs their blog on WordPress and the entire domain runs through CloudFlare for acceleration and ‘safety’. This actually presented a problem…


## How CloudFlare Works

So, anyone unfamiliar with CloudFlare this is a brief explanation that may help:

> When you sign on to CloudFlare the way your logs are displayed will change because CloudFlare acts as a proxy. This means that your visitors are routed through the CloudFlare network. This allows CloudFlare to stop attacks before they reach your network.  
> [<cite>CloudFlare Support</cite>](https://support.cloudflare.com/hc/en-us/articles/200170786 "CloudFlare Proxy")

There you go. All you really need to know is that it acts as a proxy – a middle man – and as a result, the IP served to your CMS shows up as a CloudFlare IP.


## Why It’s a Problem

You’ve probably figured out where I’m going with this already. For one, when bad logins were being made the logs were recording a CloudFlare IP. That’s super ineffective if you’re trying to block someone trying to hack your blog/site by the IP address. Two, after a while I noticed a pretty disturbing trend – the login attacks intensified while CloudFlare was enabled!

While I was doing work on the site I needed to have CloudFlare disabled so I could see my changes being made. During those periods I realized that I didn’t log a single brute force attack against the blog. Voila! Light bulb! Now, mind you, I’m not a security expert, but, I seems pretty likely that someone was taking advantage of CloudFlare’s proxy anonymity to launch a brute force attack. I haven’t found much out there about this problem in relation to CloudFlare – then again all I’ve found is posts about how CloudFlare supposedly works as a first line of defense against this kind of stuff so maybe their trying to drown out any bad pub on the subject.


## Conclusion

In my case the solution was easy – I just setup a custom rule inside CloudFlare to ban it from the blog and, what do you know, the attacks cleared up. Digging in a bit deeper [CloudFlare offers their own solution on exposing these IP’s](https://support.cloudflare.com/hc/en-us/sections/200038166-How-do-I-restore-original-visitor-IP-to-my-server-logs- "CloudFlare Support") but it requires some lower level server stuff be downloaded and enabled in your apache config and, frankly, I just don’t want to go through the pain of doing it. After all, what good does it do to someone who doesn’t have shell access to a server? It’s entirely possible that I am missing something here, but, from my standpoint this seems like a pretty serious security gaffe on CloudFlares hands.


