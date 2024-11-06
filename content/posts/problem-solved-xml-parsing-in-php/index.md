---
title: Problem Solved - Parsing Large XML Files in PHP
date: 2013-10-10T04:28:35.000Z
description: Problem Solved - Parsing Large XML Files in PHP
---

My first problem solved post! Let me just quickly outline how I’m going to do this and will be doing it from henceforth. I’m going to try my best to break down the problem into three digestible pieces – The Situation, The Problem, and The Solution. I feel like these are fairly self explanatory; so lets jump right in, shall we?


## The Situation – Let’s Track Phone Calls:

<div class="wp-caption alignright" id="attachment_32" style="width: 212px">[![Dom DeLuise](http://justinvoelkel.me/wp-content/uploads/2013/10/dom-202x300.jpg)](http://justinvoelkel.me/wp-content/uploads/2013/10/dom.jpg)This Dom is clearly better than THE DOM. Ok maybe that’s harsh…

</div>The company I currently work for hosts a call center for taking phone orders for, well, lets say widgets. All of our phones are VoIP hosted over at a company called [Virtual PBX](http://www.virtualpbx.com/ "Virtual PBX"). These guys do a pretty bang-up job if your in the market by the way. Anyway, as part of their administrator dashboard they offer generated reports for specified date periods. There was a popular consensus amongst the owners that it would be really cool to track call volume by hour and by location (areacode / city). Luckily, VPBX offers all of these reports as an XML download. These reports included the incoming number and the date and time of the call, pretty easy to parse right? What could possibly go wrong?


## The Problem – Growing Pains:

Full disclosure, so that you don’t immediately utter ‘ugh…Noob’, I was still fairly fresh to even intermediate PHP and dealing with parsing any kind of XML.  With that said, it’s a fairly tidy process in PHP. In researching exactly how I was going to go about doing this I came across several posts outlining how this can be done by simply creating a new DOMDocument and loading the XML file. I could then easily loop through each ‘call’ node, pull the necessary tag node values, and push them to an array for examination. This worked great for about a year, but as business (and calls) picked up and we grew, I noticed an alarming trend – things were getting slow, L I K E  R E A L L Y  S L O W. I didn’t have time to loop back around to look into it so the problem persisted for a few months since it was just slow and not broken. Well, wouldn’t you know it, going into our busiest month of calls, in our busiest year so far, this method finally started throwing timeouts. Even with set_time_limit() increased it was painfully obvious how inefficient this method was. A quick SSH – ps aux or login to WHM showed a massive and prolonged CPU spike whenever this script was running. It was time for a change.

<div class="wp-caption alignright" id="attachment_40" style="width: 190px">[![Terminator](http://justinvoelkel.me/wp-content/uploads/2013/10/skynet-300x225.jpg)](http://justinvoelkel.me/wp-content/uploads/2013/10/skynet.jpg)Do your part to avoid the uprising – treat your servers right.

</div>
## The Solution –   Keeping it Simple:

With my inefficiencies front and center, I picked myself up and took to google to find my answer. Turns out this is a fairly common problem so I didn’t feel too awful. The suggested solution was a simple one – do NOT create a new DOMDocument. The problem with the DOM solution is that at the time the XML file is loaded it loads the entire XML file into memory on the server. So, this is actually fine to do if your working on a small document, like when I started out. By the time this thing ground to a halt our call volume had increase from a couple thousand calls in a month to over 20,000(!). This would then result in the server attempting to load and traverse an XML file with 160,000 plus lines in it.

Not fun times, dude. Needless to say I was instantly heart broken for our poor, overworked VPS server; I DIDN’T KNOW MAN – * I DIDN’T KNOWWW*! It turns out that the path to efficiency here was to use [XMLReader](http://www.php.net/manual/en/intro.xmlreader.php "XMLReader") in conjunction with  [SimpleXML](http://php.net/manual/en/book.simplexml.php "SimpleXML"). Unlike loading the document into the DOM or just using SimpleXML on it’s own – XMLReader does NOT read the entire file at once. It will load the file one element at a time as you iterate over the file.

Now, I believe this can be accomplished using XMLReader by itself – but, I chose to use a combination of XMLReader (to iterate over the file) and SimpleXML (to parse and assign the individual node values). This may end up causing the performance to dip a hair but I found SimpleXML so much easier to work with. So, to the code!

***Originally, this was my approach (cliff notes version):***

 $xmlFile = $_POST['xmlFile']; $dom = new DOMDocument(); //load the selected XML file to the DOM $dom->load( 'saved/'.$xmlFile ); $calls = $dom->getElementsByTagName('call'); foreach($calls as $call): //this section would essentially do the following //for the select few bits of info needed like phone and date //Iterate and pull the appropriate info $info = getElementByTagName('neededTagInfo'); //push those values to an array for evaluation array_push($info,$callStack); endforeach;

*** After things went wonky, that transformed into this (again cliffnoted for your pleasure):***

 $xmlFile = $_POST['xmlFile']; $reader = new XMLReader(); //load the selected XML file to the DOM $reader->open('saved/'.$xmlFile); while ($reader->read()): if ($reader->nodeType == XMLReader::ELEMENT && $reader->name == 'call'){ $node = new SimpleXMLElement($reader->readOuterXML()); $info = $node->elementName; //push those values to an array for evaluation array_push($info,$callStack); } endwhile;


## 


## Conclusion:

As a result of swapping out the DOM for XMLReader this script, that could take upwards of two minutes to complete, now completes is task in right around twenty seconds – that’s with an extremely large file. With older/smaller files the parsing is nearly instant.

For me this really drives just how important research is as part of our job. The difference between just solving a problem and solving it in a ‘more’ correct way can just be a matter of a few more hours spent browsing examples, but in the end it’s worth it. In this case however, it would have been hard to identify the problem before it actually happened.


