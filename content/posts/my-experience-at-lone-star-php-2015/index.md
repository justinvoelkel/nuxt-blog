---
title: My Experience at Lone Star PHP 2015
date: 2015-04-21T19:33:45.000Z
description: My Experience at Lone Star PHP 2015
---

It’s that time of year — spring is in full bloom, the ugly brown motif of Cleveland is slowly giving way to greener pastures, and several development conferences are holding their yearly gatherings. This year I was fortunate enough to be able to attend Lone Star PHP in Dallas, Texas. Many thanks to my gracious and generous employer [BudgetDumpster.com](http://www.budgetdumpster.com 'Budget Dumpster') for putting me up and allowing me to spend a few days with some world class developers and even better people. I thought it made sense to put my thoughts down in a post to help out an perspective attendees (for future Lone Star events and any conference these presenters may speak at in the future) and to highlight a talk form each day that I found helpful. I was in attendance for the conference and the training day so lets start there.

## Training Day

### Getting Started with PHPUnit

#### _Presented by Matt Frost _[@<span class="u-linkComplex-target">shrtwhitebldguy</span>](https://twitter.com/shrtwhitebldguy)

##### Why I Went

<div class="wp-caption alignright" id="attachment_648" style="width: 410px">[![Matt Frost Training Day](http://justinvoelkel.me/wp-content/uploads/2015/04/mf-training-day-300x121.png)](http://justinvoelkel.me/wp-content/uploads/2015/04/mf-training-day.png)Note: This didn’t *actually happen* if you’re concerned.

</div>TDD (test driven development) is something I’ve aspired to move into for a little while now. As developers, we move from basic functional programming to more object oriented development and ultimately more complex applications, testing should become a regular part of our workflow. Along with assuring your code works, it helps your write more concise and testable code.

After recognizing testing is like, a thing, I found out pretty quickly that good resources on _how_ and _when_ to test weren’t all that easy to find. Most everything I came across consisted of walking through the most basic assertions in a calculator class with some methods that perform various mathematical functions. Nothing against those resources, they did a fantastic job showing you the basics of how to write tests, but what about more substantial real world examples? Often times modern PHP requires us to interact directly with a database or some API end point that provides useful information. Matt addressed all of the above in his training day track.

##### How It Went

Matt provided us with github repo with everything we needed to get started – essentially we just needed composer installed and an internet connection to pull in dependencies and we were off. We did start out with the traditional calculator model testing simple mathematical assertions but that was mostly to illustrate [PHPunit](https://phpunit.de/ 'PHPunit')‘s functionality and syntax. However, once those basics were out of the way we moved into a better real world example of setting up a call to several [Slack API](https://api.slack.com/ "Slack's API") end points with [guzzle ](http://guzzle.readthedocs.org/en/latest/ 'guzzle http client')and mocking the data that was returned. For me, that was the biggest take away – learning how to set up and use the mock data to effectively test a method. The only downside was that we ran out of time before we could cover the database mocking but, again, he provided all that info upfront so it was there to complete later on. Matt made himself super available to anyone who needed any kind of help and did a great job of keeping the group together through the various examples.

##### Other Highlights

We were able to attend a morning and an afternoon training session. In the afternoon I attended Yitzchok Willroth’s([@coderabbi](http://twitter.com/coderabbi 'Yitzchok Willroth twitter')) session on code katas. The session was particularly helpful in presenting ways to maximize what you take away from these coding challenges and learning how to practice with a purpose.

---

## Day One

### Talmudic Maxims to Maximize Your Growth as a Developer

#### _Presented by Yitzchok Willroth [@<span class="u-linkComplex-target">coderabbi</span>](https://twitter.com/coderabbi)_

##### Why I Went

<div class="wp-caption alignright" id="attachment_654" style="width: 160px">[![code rabbi](http://justinvoelkel.me/wp-content/uploads/2015/04/code-rabbi-150x150.png)](http://justinvoelkel.me/wp-content/uploads/2015/04/code-rabbi.png)seriously how great is this for a logo though?

</div>I attended Yitz’s training day session the previous day and I found him to be a knowledgeable and genuinely friendly guy. The way he presented the material and interacted with the group made a stressful experience that much easier. I had heard from a few people on twitter that his talks were very good and, let’s be honest, the whole reason to attend a conference is to *grow as a developer* — and also get crazy amounts of stickers but that’s a whole different story.

##### How It Went

Turns out Twitter was spot on about Yitz’s talks. This may have been the highlight of the conference for me. He spoke to the importance of the PHP development community as a whole and about the importance of the mentor/apprentice relationship. Specifically, that participating in mentoring or blogging or podcasting provides significant benefit to all parties involved. That helping others achieve and understand the things that we have already achieved helps reinforce those things, and that act of re-teaching leads to an even deeper understanding for the teacher. As someone who is trying to help in some small fashion by running this blog, his talk really hit home with me. His talk was not heavy on any sort of technical information but the subject matter, to me, is arguably just as important. I was particularly taken by the enthusiasm with which he delivered his talk.

Again, I can’t say it enough — I thought this was, for me, the best talk at the conference. It allowed me an hour to digest all of the technical information I’d taken in during the day and really focus on a soft skill that’s near and dear to my heart. Thanks, Yitz.

#####  Other Highlights

Other talks I attended included Chris Tankersley’s([@dragonmantank](http://twitter.com/dragonmantank 'chris tankersly twitter')) talk on OOP, Composer best practices by Jordi Boggiano([@seldaek](http://twitter.com/seldaek 'jordi borgianno twitter')), Introduction to OAuth clients by Matt Frost([@shrtwhitebldguy](http://twitter.com/shrtwhitebldguy 'matt frost twitter')), and API Pain Points by Phil Sturgeon([@philsturgeon](http://twitter.com/philsturgeon 'phil sturgeon twitter')). All of those talks were really good but, in particular, I took a lot away from Chris’s, and Phil’s talks. Chris expanded basic OOP principles that are typically taught when you learn to program (animals, cars, etc) to cover some _gotcha_ type use cases that can be addressed by using PHP interfaces and traits — yes, dogs can have wheels too. Phil, co-host of the [PHP Town Hall](http://phptownhall.com/ 'PHP town hall podcast') podcast presented on common issues with API builds and how to address them.



---

## Day Two

### A Gentle Walk Towards SOA

#### Presented by Jeff Carouth [<span class="screen-name">@jcarouth</span>](https://twitter.com/jcarouth)

##### Why I Went

<div class="wp-caption alignright" id="attachment_666" style="width: 310px">![evolution of PHP architecture](http://justinvoelkel.me/wp-content/uploads/2015/04/evolution-of-arch-in-php-300x160.png)Essentially The March of Progress for PHP developers. If you laid all of those images on top of each other and lit it on fire it’d probably more accurately depict my code style.

</div>I’ve been hearing plenty about service oriented architecture over the past year. How breaking your app out into individual micro services can help scaling and maintenance of your application. I just literally had no idea where to start. Looking at overly complicated design layouts was getting me no where quickly but, the thought of individual micro services written in the language that best suits it’s purpose was and is really intriguing. So, when I saw this on the schedule it was a no brainer to attend.

##### How It Went

Of all the talks I attended during the conference I think this may have been the most helpful. Jeff is a really engaging guy so, in general, the whole talk was really entertaining. It demystified SOA for me and presented it in a way that was understandable for someone getting their feet wet with these concepts. Jeff did a great job of balancing code examples with higher level design principles and concepts. Too often these presentations can get too heavy in technical information and code examples and they become too hard to follow, or, they’re just chock full of abstract principles with too little code to put it into context. Along with said concepts and code he followed up with the admission that this all is a ‘perfect world scenario’ — in the real world, shifting your code base from a ball of mud to nicely distributed services is time consuming and fairly unrealistic to the business stakeholders/clients. He provided an example of how the company he is currently working for began shifting their current application in more of a hybrid approach. It helped to drive home the fact that there really isn’t ever a _‘one size fits all’_ solution to every problem or scenario.

##### Other Highlights

Other talks I attended on day two were Deploying Web Applications with Capistrano with Andrew Turner([@galenandrew](https://twitter.com/galenandrew 'andrew turner twitter')), and Testing the hard stuff: writing tests for things you “can’t test” by Matt Land([@blitzcat](http://twitter.com/blitzcat 'matt land twitter')). Again, both really good talks but Andrew’s talk on deployments hit close to home as it’s something I’ve been forced to address a lot lately. He walked through the basic configuration and some use cases for using Capistrano for your deployments.

## Conclusion

This was my first PHP exclusive conference and it couldn’t have gone any better. What really struck me was how welcoming and friendly everyone was. Often times in this industry ego and elitism take precedence and help to alienate those of use who are just looking to improve. This experience was the polar opposite of that. The presenters themselves and the attendees I spoke with could not have been more friendly,open, or supportive. I had a blast and I cannot wait till next year — I’ll be back!
