---
title: Back to Basics - Clean Code Chapter Four 
date: 2022-03-29T16:53:05.644Z
description: Revisiting good practices reading Clean Code by Robert Martin
---

Chapter four of _Clean Code_ by Mr. Uncle Bob (as I've taken to calling him in my head) - covers commenting our code. This is a bit of controversial topic in my eyes. I fully understand and acknowledge the sentiment that 'good code is self documenting' - which is mostly the opinion expressed in chapter four; but I think some people make that statement too binary. Should we never comment things then? Is the addition of a comment ackowledging that our code is _bad_? Personally, I think no - comments are a good addition sometimes and they can exist in even good and expressive code. 

In the interest of acting with humility - I've been so guilty of a lot of these bad practices in the past. I think time has taught me when and when not to use comments and how to make them useful but let's see and get into the notes:

## Comments
- Mr. Uncle Bob hits us right out of the gate with _"comments are, at best, a necessary evil"_.
  - I think that goes a long way setting the expectation and tone for the rest of the chapter - _try to not write them_
  - I don't fully disagree with this
- Also - _"The proper use of comments is to compensate for our failure to express ourself in code"_.
  - I will take a bit of an exception to this statement.
    - I suppose if you are wrapping a lot of language logic in expressive function names this is expected to be the default state of things.
    - But, some times things just don't shake out this way. Maybe we're dealing with old code that violates a lot of the good practices we have discussed prior to this chapter. Maybe we are very limited on time or our team is adamant about how things should be structured. Point being - if you aren't in a green field situation of small functions and solid naming conventions then comments play a crucial role in productivity and understanding.
    - That being said - I think the remainder of the chapter does a _great_ job in laying out situations where comments can be useful and where they become pitfalls.
- Inaccurate comments are worse than no comments
  - Yep, I suppose that is a valid point. I can't argue that if comments are krufty and out of date they'll mislead very easily.
- Truth is always in code.
  - Yes, this is also true. And, some code is very very bad and hard to decipher. Splitting the difference - if there were some slightly out of date comments in bad code that help illuminate _some_ path or intent - is that not helpful? Is it _always_ better to rely on just the code - even when it requires copious amounts of mental overhead to understand?


## Explain Yourself in Code
- Code can make a poor vehicle for explanation.
  - Agreed, and I think that's my argument for the relevance of comments in the right context.
  - The example given resonates with me (checking several statements in-line inside of a conditional vs. obj.checkConditions())
    - I see the former a lot. I did the former a lot.
    - I think I did this a lot because it felt like the latter was an unneccessary abstraction...till you go back to that code six months later and need to remind yourself what all of those in-line conditions are again.

### Good Comments
- Legal comments - makes sense if you need them
- Informative comments
  - a little vague - I don't think anyone is leaving a comment for malicious purposes
  - 'better to use the name of the function to convey information'
    - this is probably the best approach and aligns most with what we've learned so far
- Explanation of intent
  - This is where I argue comments are most valid. There are so many implementation approaches people are bound to have different opinions on how to solve a problem. These types of comments clue a developer in on your mindset and choices when solving a problem and can help them better understand and even learn if they've encountered this code and had a different approach in their head on how it _should_ be done.
- Clarification
  - I think these fall in line with explanation of intent in terms of usefulness.
  - These types of comments (IMO) are those that help us surmount the limited expressiveness of framework/language APIs. Modern APIs have come a long way in terms of being declarative but they're not perfect and these comments help to fill those gaps.
    - As a very basic example I just think of javascript's `Array.prototype.filter`. Even though it is expressive and clear about what it is doing I often can't remember if it is filtering elements that evaluate to true or false - that seems to be a common pitfall. Leaving a small comment on the callback logic can help.
- Warning of consequences
  - I don't know - these feel like a bit of a smell to me.
  - If you have dangerous or _highly conseqential_ code maybe time is better spent trying to lessen the consequences.
  - the example given is for a long running test - which is fair. I guess the consequences are time based not risk based.
- TODO comments
  - Ah yes - they make me feel warm and fuzzy. As if I've been proactive in identifying an improvement or change needed.
  - Rarely ever acted on or see the light of day again.
  - Sorry for being so pessimistic - failure to act on these is probably a team or process error more than anything. If you have them you should have time built in to review and pay down that tech debt.
- Amplification
  - Feels similar to warning of consequences or explanation of intent - but very valid.
- Docblocks (altered from Javadocs - because I don't java)
  - Skipping ahead a bit; it never dawned on me that docblocks are a waste of time for a private API.
  - Like TODO comments these tend to make me feel warm and fuzzy because they just make the code look and feel organized.
  - But, the overhead in maintaining them over time and keeping them accurate just plain sucks.
    - TIL the cost of this maintenance is worth it for a public API but not so much for private.

### Bad Comments
I'm going to say right off the bat that some of these feel like those monochromatic segments of an infomercial. Meaning they feel like an overly clumsy and wild representation of what a developer would actually do. Still valid points are delivered.

- Mumbling
  - I think the point is that your first pass at a comment can make sense in _your_ head; but you need to re-read and refine from a contextless point of view (aka someone who happens upon this code later and hasn't been working on it)
  - "Any comment that forces you to look in another module for the meaning of that comment has failed to communicate to you and is not worth the bits it consumes"
- Redundant comments
  - I've been guilty of this in the past and, if I'm being honest, probably continue to do it to some extent.
  - You need to re-read your code including the function name and logic and decide if it is expressive enough
  - If you've already got a comment and it's just re-iterating the implementation almost in psuedocode it's not valuable at all.
  - Just don't re-state implementation details in a comment.
- Misleading comments
  - The number one reason not to use them.
  - What was originally meant to help begins to hinder
  - Can lead to major productivity hits if you just "take their word for it".
  - Things as subtle as word choice (subjective things) can lead to misinterpretation and still serve as misinformation.
- Mandated comments
  - As stated 'just silly'
  - We don't need docblocks on everything by mandate for reasons outlined above
- Journal comments
  - Sounds like maybe there was a time and place for these but definitely not in modern software development with source control logs.
- Noise comments
  - Count me guilty having done this as well.
  - Sometimes trying to be helpful and explicit becomes counter intuitive and a waste of time.
  - These seem like they would be an artifact of mandated comments.
  - Don't include useless emotional comments - yes you might be frustrated but that act is in no way helpful or constructive.
- Scary noise
  - Ah yes - the old copy pasta problem. Again an artifact of mandating maybe?
- Just use a variable or function name
  - This is something I, and I think others, have failed to do at times for fear of adding more code than is required.
  - It all points back to the benefit(s) of being clear and explicit. That additional code may not serve a practical function outside of giving you easier to comprehend code - that in and of itself makes it worth it.
- Positions, Closing Brace, Atrribution
  - I've not run into these types of comments in modern projects thankfully.
- Commented Code
  - "Others who see that commented-out code won't have the courage to delete it."
  - ^ this times 90872394720
  - This is a practice that relates back to 'care'. We need to make sure we're tidying up after ourselves.
  - Given you work somewhere that does peer review commented code should never get into your mainline branch.
  - If you have commented code you should exercise the boy scout rule and clean it up as you encounter it if possible
    - If the person that wrote that code is not longer around and there are no tests to check for regression this is _difficult_ to say the least.
- HTML in comments
  - can't say I've seen this fortunately.
- Nonlocal, TMI, Inobvious Connection
  - these are little bit tricky becuase they feel like something you need to catch yourself in the act of doing
  - In the case of inobvious connection it is just as much to do with the code being written as the comment supporting it.
    - this would probably be a good spot to use variable names. In the example the bits of code in question might be able to be pulled out into variables with expressive names or clarifying comments of their own.
- Function Headers
  - "A well-chosen name for a small function that does one thing is usually better than a comment header"

  And that is pretty much it. As an added bonus I learned the meaning of the word _anathema_. In reading this chapter I felt like I would be really light on notes but surprisingly that has not been the case.

  Moving ahead to chapter five on formatting!


