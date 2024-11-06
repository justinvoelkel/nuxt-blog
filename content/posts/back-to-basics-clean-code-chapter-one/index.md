---
title: Back to Basics - Clean Code Chapters One and Two 
date: 2022-03-11T22:20:14.670Z 
description: Revisiting good practices reading Clean Code by Robert Martin
---

I've been working in development professionally for over a decade now. I've learned quite a bit from real world experience, peers and mentors, making lots of mistakes, and books. Sometimes when you've made a certain level in progress in something it pays to go back and revisit the basics. I think that's where I am now and I've decided to go back and re-visit the Uncle Bob line of books. I read Clean Coder last summer and loved it - until then I'd not read it and found it well written and it really motivated me to make some changes for the better. But, that book is a much higher level view of the profession and the career of a developer. I want to revisit the basics of what makes good code, good architecture, and, ultimately, a good developer. So I've settled on re-visiting Clean Code (to start). I believe I started reading this book years ago but never actually finished - I honestly don't even remember how far I got initially. I thought it would be a good excercise to document my notes here. So here we go - my thoughts on both chapters one and two (because one is a soft intro).

## Chapter One: Clean Code

This chapter mostly serves as a soft intro to the book, what it's about, and who it's for. However, there are some really significant points made on a few very important topics.

### The Total Cost of Owning a Mess

This subsection of the chapter is only a few paragraphs but manages to be one of the bigger take-aways for me. In short, it covers the long term implication (cost) of bad code. The book illustrates the asymptotic relationship of productivity over time given the relative tidyness of a codebase. Having worked primarily at small companies and start-ups I've seen this play out time and time again. I've also been someone who's contributed their fair share of poor code in the name of speed or "just getting it done". In Jeff Lawson's book _Ask Your Developer_ he encapsulates the business mindset at work here (paraphrasing) "to the business there's never time to do it the first time but always time to do it over and over again later". The point being the cost of slowing down and approaching a problem with intellect from the start is an investment - and by being cheap (rushing) we're incurring technical debt. If you're part of an organization that regularly pays down their technical debt that might be ok - you are trading quality for speed for some important reason but the understanding is that trade off comes with a price that eventually comes due. If you are part of an organization that _does not_ regularly pay down that technical debt...well...it compounds. The debt you incurred initially doesn't just sit in a vaccuum waiting to be addressed - it's part of a larger system that is interconnected - and as you add features to that system things only get worse and worse. This is how, over time, productivity drops exponentially. The cost of that is not only missing features and defects but potentially unhappy customers, unhappy developers, and unmotivated workers. Face it - no one wants to go back and work on the legacy turd application. 

### The Art of Clean Code / What is Clean Code

These subsections introduce you to the high level attributes of clean code as well as exposing you to personal definitions of "clean code" from prominent figures in software. It was interesting to be given the author's point of view and some unstructured feedback from these other figureheads. It seems pretty obvious that the question was left intentionally vague to invoke some thought from those who were asked to define "clean code". There is some over lap and some left field stuff but it's all helpful to give context to the conversation. I pulled out some of the keywords or adjectives I thought were most important (to me) from these responses:

- elegant
- efficient
- straightforward
- minimal
- simple
- direct
- tests
- explicit
- care
- duplication (avoidance)
- expressive
- expected

I love this list. I think after all this time I've spent working in code these descriptions represent the end goal of any system or small piece of code I'm tasked to take on. It's worth considering how much overlap there is here in terms of related words or synonyms (IE straightforward and simple). To me it all distills down to simplicity - code is simple because it is tidy, nicely arranged, digestable, modular, etc. The problem is _that is way harder to achieve than you think_. There is a quote I love that is weirdly attributed to several individuals that encapsulates this really well.

> If I had more time, I would have written a shorter letter.

That is to say - brevity, simplicity, and being concise are very difficult, especially when you are trying to convey something complex or context heavy.

## Chapter Two: Meaningful Names

Talk about going back to the basics - naming is one of the most fundamental concepts that we apply to our code every single day. It's deceptively important. One of my favorite resources for learning over the last several years was the Youtube channel _FunFunFunction_. The host (MPJ) was wonderful and so relateable - I use past tense because he has since moved on from the channel. But, a common line that would come up during his explanations and coding examples is "well - naming things is hard...". I remember thinking early on - _no it isn't - you just pick a name that isn't totally dumb and move on_. After having some of the projects I'd worked on over the years start to mature I started to see the error in my ways. Going back to code you wrote a week, a month, or a year ago can be very daunting if you haven't gone out of your way to name your classes, variables, and methods in a very explicit way.

I'm going to just throw a few of my thoughts/notes in on some of the subsections this chapter offers.

### Use Intention Revealing Names
- As a baseline we should choose names that mean something (single letter variables being the poster child of what not to do). But, the name chosen should also serve to reveal intent. When variables become part of a bigger piece of code they should serve to explain what is going on and not _just_ hold values.

### Avoid Disinformation
- Avoid using names that can be misinterpreted. Using the name `hp` to represent hypotenuse is a poor choice because it can represent so many different types of things (horsepower??).
- Don't refer to constructs as things that they aren't - meaning if you are creating a collection of `Foo` don't bother calling the variable to hold it `fooList` or `FooList`.
- Avoid using similar variations of names - especially true when names are long and differences are harder to spot.

### Make Meaningful Distinctions
- Avoid using indexes for things in a series (a1, a2, ...a[n])
- Avoid using noise words like data or info that contribute little to the understanding of what they represent (what's the difference between AccountData and AccountInfo?)

### Use Pronounceable Names
- This one is pretty self explanatory but - use real world names for the real world things they represent and try to avoid truncating or mixing word truncations together to form names

### Use Searchable Names
- Yes! yes! As someone who, maybe too often, uses grep to shortcut to finding things in the code base this is a must
- I've actually worked in code that had such ambiguous naming that grepping around was at best a misleading time sink and at worst a complete waste of time because someone left a variable or method named `foo` or `test`

### Class Names/Method Names
- Classes should have a noun or noun phrase as a name with out using noise words. Don't use verbs.
- Methods should be your verbs (because their acting on the noun).
- Accessors, mutators, and predicates should be prefixed with `get`, `set`, and `is`

### Don't Be Cute
- Seriously just don't.
- Code is fun but trying to determine the intent of something because someone was using inside jokes to name things is not fun.

### Add Meaningful Context
- But don't go overboard with it.
- For new eyes on code there's no real distinction between `state` as in _the condition of a thing_ and `state` a piece of territory organized under a government.
- Adding something like `stateAbbr` or `addressState` are just fine.
- The minimal amount is the right amount here - think in the context of someone who has not seen this code before. The surrounding code may hint that `state` is part of an address but that's mental overhead someone has to spend to understand.
