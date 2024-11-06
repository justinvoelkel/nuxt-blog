---
title: Performance - WP-Options Table Bloat and It's Performance Effects
date: 2013-12-26T11:28:23.000Z
description: Performance - WP-Options Table Bloat and It's Performance Effects
---

Tis the season to stay up late and fix critical server problems – well, for me at least. I’m going to preface this post by saying I know enough about SSH and command line environments on a server to make myself dangerous but in 90% of cases I’m useless. In my free time away from the day job I run several blogs and do some freelancing for a few clients. For this purpose I have a manged VPS with the folks at [Wiredtree](http://www.wiredtree.com/ "Wiredtree"). And, i have to tell you, these guys are fantastic. I rarely have problems and when I do they generally respond to support tickets within an hour if not immediately. They’re always really courteous and extremely knowledgeable. OK OK enough of the sales pitch right? MOVING ON!


## The Problem: MySQL Using Too Much Memory and Causing Server Timeouts Within WordPress

While working on a client’s WordPress website last night I noticed that, either my ISP had reverted back to 14.4 modems, or my server was running really slow to do simple things like update a picture or a piece of meta data. Since I’m running the site on a VPS I had root level access so I logged in via SSH and ran a -top command to see what exactly was going on. I was more than a tiny bit surprised to see that MySQL was reportedly using 20% of the available memory and my load averages (typically 1 or lower) were spiking anywhere from 5 to 10+ – in short, not good.

So I went to the kind folks at Wiredtree and opened up a ticket. Just a week prior to this I had a similar issue that had ended up being some resource abuse on my node, so I figured this was likely the same thing. As usual they were johnny on the spot jumping into my issue but after nearly an hour of server timeout’s I was starting to get worried. They reported back the node was find and that the problem was indeed MySQL hogging resources. After running a repair and optimization on the wordpress database they reported back there was a small boost in speed but the site was still having major load issues. They also informed me that my database was 420MB (wow) and that could be part of the issue.

Both myself and the staffer assigned to my ticket continued working on this into the night. After trying the requisite ‘turn everything off’ method to solving WordPress issues (ie. revert to the stock theme and turn all the third party plugins off..) I had determined that it was unlikely this was caused by the recent update to wordpress 3.8 or any of my current plugins. After that I tried dropping some dead weight from my wp-options table as it accounted for almost 300MB of my database (don’t try this if you don’t know what you’re doing). I noticed when I did this there were an absurd amount of entries from NGG (Next Gen Gallery plugin) – interesting, but they didn’t look like they were doing much of anything so I moved on.  So, that left MySQL and the server. I started, most logically, by restarting MySQL. No dice, things were ok for a few minutes but MySQL, again, climbed to the top of the processes list. By this time it was the wee hours of the morning and I was beginning to think this was never going to be resolved. The staffer reported back that they narrowed it down to a query on the site that was requesting data over and over. He dumped the output of the watch command he had used to spot the faulty query. See below –

 SELECT option_name, option_value FROM wp_options WHERE autoload = 'yes'

Well, *I was* right about the wp_options table (*pat on the back, keyboard maestro*). A quick cut, paste, and Google search [revealed the answer](http://wordpress.stackexchange.com/questions/71691/slow-query-for-the-wp-options-table "Stack Exchange WordPress").


## The Solution: Indexes Speed Up Searches

That’s it, plain and simple. It turns out that those thousands and thousands of entries by the Next Gen Gallery plugin had bloated the wp_options table so badly it could no longer perform a routine query without consuming massive memory. The idea here is that the wp_options table should never grow to be 300+MB because plugins should not be abusing it as Next Gen was. Consequently, there were no indexes on the table and the query being performed was continually stacking up on itself because it had to traverse every record in the table when the query was invoked. Obviously, this chewed through memory really quickly. The solution was to add an index to the table and/or remove the superfluous entries in the wp_options table. I actually did both, and I got rid of the Next Gen Gallery plugin. The performance difference was like night and day! This site that had been barely operational for several hours was now a joy to zip around. You can read the full, and more expert, version of the answer I found [here](http://wordpress.stackexchange.com/questions/71691/slow-query-for-the-wp-options-table "Stack Overflow").


## Conclusion

Lots of people say this but it really is important advice – don’t simply trust every plugin in the WordPress codex. I don’t doubt that the folks who develop Next Gen did NOT expect such a problem to arise but non the less, it did. A quick Google search, check of the date on the plugins last update, and rating should give you a relatively good idea if the plugin is dependable or not. Also, using a plugin like [WP Cleanup](http://wordpress.org/plugins/wp-clean-up/ "WP Clean Up plugin") to delete revisions and keep your database lean and mean is a must.


