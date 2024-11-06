---
title: Services - CodeGuard - Gonna Back That SASS Up....
date: 2013-10-15T07:01:24.000Z
description: Services - CodeGuard - Gonna Back That SASS Up....
---

I thought I’d like to eventually get around to suggesting and/or reviewing some of the services that I use on a regular basis. There is a huge catalog of different services that solve any number of problems you run into in your daily work so sometimes posts like this can be helpful in picking out the right solution.

I’m pretty paranoid about my work, I mean who wouldn’t be, you spend hours on something that you care about – you don’t want to lose it. Data redundancy is extremely important both locally and remote. In my opinion, having a local copy of your site paired with your remote copy just wont cut it anymore. Most hosts do a good job of keeping nightly backups of their servers but sometimes that backup and restore is an extra (cough cough more money) option. Plus, you never know when a meteor may hit the data center and wipe out all the servers – ask the dinosaurs – oh wait. If you’re running wordpress there are some pretty good free options available as far as backup plugins. I’ve typically stuck with [BackWPup](http://wordpress.org/plugins/backwpup/ "BackWPup Plugin for WordPress") but there are several other options available. The ability to send those backups to Rackspace or Amazon S3 is pretty awesome.

[![CodeGuard Logo](http://justinvoelkel.me/wp-content/uploads/2013/10/codeguard-300x204.png)](http://justinvoelkel.me/wp-content/uploads/2013/10/codeguard-300x204.png)So, what if you don’t run a CMS? My company has a large and rapidly growing website that we recently did a full ground up redesign on and, for the sake of scalability, decided to continue on with a static website instead of transitioning to a CMS solution. During this time we also decided to move over to a managed VPS for hosting as well. So with no backups set in place, I wanted to make sure we had something on top of whatever our provider offered as far as nightly or weekly imaging. In steps [CodeGuard](https://codeguard.com/ "CodeGuard"). CodeGuard does a phenomenal job of not only backing up your physical files but also your database(s). I know it’s totally anti-nerd to use stuff that is simple but I have to say it was crazy easy to set everything up. As you would expect it’s simply a matter of entering SFTP or FTP information as far a connecting to your server for remote files and for the database you can connect directly through an SSH tunnel.

Where CodeGuard really excels, for me, is peace of mind. Yeah, it’s nice to know your data is being backed up but they take it to the next level. I get daily reports telling me how many files were added, changed, and removed. If you’re not OCD about data redundancy this can get annoying pretty quickly but can be adjusted. The really great thing about that feature is that, suppose you walk into a situation where that update email says ‘Oh hey you had 3000 files and they were all deleted last night by some guy in Bulgaria’ (paraphrasing of course) – It’s a-ok hombre. All your restore capabilities are at your finger tips in the CodeGuard back end. Knowing I have that restore at the drop of a hat is what’s really important to me. I don’t want my backup running quietly, I want it to phone home frequently so, in case of emergency, I know exactly where were at – that’s where CodeGuard really shines. Along with all of that they also give you some analytic information as it tracks the ongoing changes on your site. And, it’s also worth noting that the UI on the back end is really simple and really, really, intuitive.

<div class="wp-caption aligncenter" id="attachment_59" style="width: 699px">![CodeGuard UI](http://justinvoelkel.me/wp-content/uploads/2013/10/codeguard.jpg)The UI is really well done. It’s polished but it avoids the pitfalls of too much eye candy. Everything has it’s place….

</div>As the cost of space has dropped like a rock of the past several years, the options offered through CodeGuard are expectedly reasonable. Services start from just five dollars a month for five gigs of space and unlimited databases.


## Conclusions

So far, so good. Backing up can be a hassle, especially if your manually dumping your databases and managing your own file backups (not sure if anyone actually does this but who knows really). I’d highly recommend CodeGuard’s service to anyone looking for a simple and intuitive backup solution for a static or CMS based website. They do an awesome job of tracking changes which can be truly helpful on large and extremely active websites. Their service is where good web design meets helpful and important functionality; you can’t really argue with that.


