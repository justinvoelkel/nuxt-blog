---
title: Create a Glowing Button With CSS3
date: 2013-12-07T22:29:11.000Z
description: Create a Glowing Button With CSS3
---

I mentioned previously that I’ve been putting together an iOS app with [PhoneGap](http://justinvoelkel.me/app-development-getting-started-phonegap/ 'App Development: Getting started with PhoneGap') for my ‘day job’. The design for that application was a bit out of date since it had been done several months ago. We were sidetracked by Apples ridiculous developer application program and Duns and Bradstreet’s inability to change a few items in their database for us – BUT that’s a story for another day. Anyways – that new design included a new feature (sick features!!!!) – the ability to search for pricing on our – erm, widgets – based on your location. On our website it’s preferred that you search by zip code or city, since PhoneGap can tap into the phone’s GPS for lat and lon location, it’s actually easier to just add a button to search by your current location (no surprise there right?). Since that really is the preferred search method I wanted to add a very subtle hint that – ‘hey, bub….you should use dis right here’.

## First Attempt – Don’t Blink

My first thought was to just blink the opacity using css3 keyframes. Since this was included with the PhoneGap default stylesheet it just required a couple of quick adjustments. The default started at 100% opacity – went to something like 70% halfway through the animation and back to 100% at the end of the animation. I just adjusted this to go from 100% to like 10% to make the blink a bit more noticeable. See below:

<div class="alert alert-info"><span class="alert-before"></span><span class="alert-after"></span><div class="alert-wrapper">**Heads up!** Note: You should follow standard procedure as far as vendor prefixing in a phone gap app if you plan to deploy on several devices. For the sake of time and since this is an iOS specific app – I’ve only included the webkit prefix.  
<span class="ico-st alert-close"></span></div><div class="clear"></div></div> 
    @-webkit-keyframes fade { 
         from { opacity: 1.0; } 50% { opacity: 0.1; } to { opacity: 1.0; } 
     } 
    .blink { -webkit-animation:fade 2000ms infinite;

This ended up looking OK but it definitely was not as subtle as I wanted it to be. I remembered that one of those ‘daily deal’ sites that I visit on occasion used a really nice background pulse to create a nice gentle pulsing glow on important calls to action. I had checked it out in the past and knew they were doing it using javascript – I knew there had to be a way to do this ins CSS3 – which would help decrease the footprint of the app and be better for performance in the long run.

## Second Attempt – Keyframes and Animating Box Shadow in CSS3

I hadn’t done much with css3 keyframes and animations in the past so this was uncharted water for me. Luckily I found a nice write up over at [CSS](http://css-tricks.com/snippets/css/keyframe-animation-syntax/ 'CSS Tricks - Keyframe Animation') Tricks about the basics. That Chris Coyier sure knows a lot about front end development. It turns out my desired effect was as simple as adjusting the css above to just fade the blur of the box shadow – easy peasy. Check it out:

      @-webkit-keyframes bgBlink{
     from{ -webkit-box-shadow: 0px 0px 0px rgba(244, 136, 4, 0.95); } 50%{ -webkit-box-shadow: 0px 0px 25px rgba(244, 136, 4, 0.95); } to{ -webkit-box-shadow: 0px 0px 0px rgba(244, 136, 4, 0.95); } }
    .bg-blink{ -webkit-animation: bgBlink 2s 20; }

I also limited the number of blinks. Since I assumed it would get annoying if for some reason you happened to stare at the screen for more than 30 seconds – plus, when it was running the animation on infinite I noticed my phone was getting a bit warm (aka working a little too hard). So there you have it – you can see a demo over at code pen I made for a few of my button designs.

[<span> Demo </span>](http://codepen.io/stin4u/pen/BrtiF)

## Conclusion

Like I said, I hadn’t ventured into CSS3 animations too heavily so this was a nice intro with a nice subtle effect. It really shows the amazing power of CSS and where we’re headed. It really is great to be able to move away from heavy libraries like jQuery (it pains me to say this because of my unabashed love for it). As mobile performance and optimization becomes more and more important it’s reassuring to know that we’ll be able to keep a lot of eye candy without compromising performance and application weight.
