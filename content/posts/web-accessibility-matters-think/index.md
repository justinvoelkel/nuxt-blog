---
title: Web Accessibility - It Matters More Than You Think
date: 2014-02-19T01:27:42.000Z
description: Web Accessibility - It Matters More Than You Think
---

Lately I’ve made more of an effort to attend local development meetups in the Cleveland area. It’s been great meeting members of the local development community and network a bit – it’s important in any profession. The topic of the last event I attended, [WebSigCleveland](http://websigcleveland.org "WebSigCleveland"),  was web accessibility. It’s a subject I’ve heard kicked around by Chris Coyier and Dave Rupert on Shop Talk show several times but, personally, I hadn’t dug into it at all. That talk was given by Tom Babinszki (@TBabinszki) who consults IBM on their accessibility and he’s an expert, not only in theory, but also in practice – he’s blind. For me, it reminded me how much I take my vision for granted, but, maybe more importantly, how much more I NEED to be doing on my projects to make them more accessible.

<div class="alert alert-info"><span class="alert-before"></span><span class="alert-after"></span><div class="alert-wrapper">**Heads up!** If you’ve got a moment, take a minute and head over to Tom’s website [http://blindcoincollector.com/](http://blindcoincollector.com/)– there are few better case studies for accessibility than a site *for* the vision impaired that is *built and edited* by a man who is also visually impaired. His post on [blindness and technology](http://blindcoincollector.com/2013/10/03/blindness-and-technology/ "Blind Coin Collector - Blindness and Technology") is well worth the read.  
<span class="ico-st alert-close"></span></div><div class="clear"></div></div>
## Just Do It – Everybody Wins

During the talk Tom hit on something that flipped the light switch in my head – he mentioned that, as steps have been taken to improve handicap accessibility to buildings, it’s added value for the non-handicapped – think of ramps, and someone on crutches, a mother with a stroller, or the elderly. It was easy for me to draw the parallels from this to web accessibility, especially considering that [ insert your favorite SERP ]’s crawlers aren’t living, breathing, *seeing*, people. You probably understand what I’m getting at by now – making a few simple modifications to your markup makes it more accessible for folks that need it, but it adds additional value in areas like SEO, user experience, and markup readability.


## You’re Doing It Right

So, what can you do to make your website more accessible? Odds are your probably already doing some of it right.

- Add title and alt attributes to your images and links. Leave these blank if the image is layout related.
- Make sure links have a focus state.
- Set an appropriate/logical form layout.
- Provide form labels for your fields.
- Make sure form errors appear relative to the appropriate field.
- Use semantic HTML5 markup.
- If you’re not using HTML5 markup it’s ok! Use [ARIA landmarks](http://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page "ARIA Landmarks") to define roles.
- Avoid using inline javascript.
- <del>Provide a javascript alternative. </del>
- [Progressive Enhancement](http://alistapart.com/article/progressiveenhancementwithjavascript "Progressive Enhancement with Javascript") (thanks to Dennis Lembrée in the comments!).

Just by glancing at the list you can tell a few things. One, just about everything you need to do to make your site more accessible is markup related. That means your not going to be dealing with any front-end headaches. That’s a plus right? Two, the added value of a few of those list items are pretty powerful. Anyone who’s started using semantic HTML5 tags for their markup will know how insanely readable it becomes – it makes infinitely more sense than just id’ing divs. Along with that adding alt and title attributes is a standard SEO practice for markup. If it isn’t part of your workflow – MAKE IT PART! They truly make a difference to the SERP’s as well as our friends using screen readers. And, finally, all of those form related items are a standard from a user experience standpoint. If you’re using something like jQuery to show/hide form errors on form validation just realize a few things – that will remove your error text visually but it won’t be hidden from a screen reader, those error messages should be close to the field they’re validating (don’t hack a modal or something, but if you do…), make those errors as descriptive as possible. That last one is especially important for accessibility and, believe it or not, mobile. Say your awesome responsive design breaks on the latest and greatest devices – you’re error might be popping up two fields down or above/below the form – if that error isn’t descriptive people are going to bounce (heyo – conversion and SEO!!1!).


## Conclusion

The biggest thing I took away from Tom’s talk and my subsequent research is that accessibility isn’t hard and it provides so much value aside from that specifically, it just makes sense to do. On top of the added value with the SERP’s it make sense to keep your site available to the widest audience as possible. Just like browser testing, local development, and a million other dev things – we shouldn’t take for granted the fact that, although it’s working right for me, there is a chance somewhere out there in the world it’s not working correctly for someone else – minimizing those cases is our job. The list above is just the tip of the iceberg, if you’re interested in learning more about what you can do checkout [The Accessibility Project](http://a11yproject.com/ "The Accessibility Project") and [The MDN](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA "ARIA - The Mozilla Developer Network").


## [<span class="screen-name">@sos_jr</span> ](https://twitter.com/sos_jr)


