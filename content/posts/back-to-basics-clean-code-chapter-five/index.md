---
title: Back to Basics - Clean Code Chapter Five
date: 2022-04-12T19:17:04.795Z
description: Revisiting good practices reading Clean Code by Robert Martin
---

Egh well - chapter five is about formatting. This one's a bit if a layup with the automated tools and standards at your fingertips now but still worth a read I guess. The biggest takeaways I had from the chapter were around the flow of code and ordering methods. I do think that the logical spacing of things is an important topic to have covered.

## Purpose

- "It [formatting] is too important to ignore and it is too important to treat religiously"
  - I spent a lot of time ignoring formatting but in my defense I spent a bunch of years being the only dev a one small company
  - I've _definitely_ experience the latter (treat religiously) part
    - The value in formatting is standardization - not winning an argument about tabs vs spaces or whether to force requiring semi-colons in your javascript
    - I've been on teams where these discussions have raged on far longer than they should and they feel pointless but also important. Weird dichotomy.

## Vertical Formatting

- The stats provided here amongst projects land at a lower limit or average of 200 lines and an upper limit of 500 lines.
  - It's noted this shouldn't be treated as a concrete rule but I'd argue your team should set some sort of upper limit and stick to it.
  - As modules or components go - I think if you've dragged something out past that _somewhat_ arbitrary number there is probably an opportunity to break things out.
  - It's certainly going to be very hard for someone to understand your components/modules if they're 500+ lines long
  - I've had this argument before about Vue components that feel like run-on's. Most folks just feel like things aren't worth abstracting until you have a reason for re-use. I don't really believe that - I think large run-on components are difficult to understand and set a bad precident about what's acceptable.
  - The newspaper metaphor
    - This is a surprisingly effective metaphar - probably because I've worked in web and some of the concepts hold true there too.
    - A source file should be like an article in a news paper
      - The name by itself should convey if we are in the right module or not (again - naming is super fundamental given prior chapters but also super hard)
    - News papers are composed of many articles in varying sizes.
      - Most are small, some bigger, but few every command a full page
      - This is what makes the newspaper usable
        - I guess this is true. I didn't grow up in a time where I made much use of them but it makes sense if it was just one big article people wouldn't bother.
  - Vertical openness between concepts
    - each _line_ represents an expression or clause
    - each _group of lines_ represents a complete thought
    - these thoughts get delineated by spaces or breaks (white space)
  - The sort of before/after examples given are really illuminating - I guess I never really realized how awful this stuff is to read if you don't space it out somewhat.
    - I guess I've looked at minified code and that's a similar headache but....yeah don't minify your pre-minified code with bad formatting.
  - Vertical Density
    - implies close association
    - comments serve to break density association so should be avoided
  - Vertical Distance
    - Concepts closely related should be kept vertically close to each other (in same file)
    - The note about how frustrating it is to sort out what a system does by constantly scrolling up/down or moving files is something I can totally relate to.

## Variable Declarations

- Should be declared as close to their usage as possible.
  - I've broken this before by shoving all my variable declarations at the top of functions. Don't really do it anymore but...yeah.
- However, instance variables should appear at the top of the class (yep - that's a no brainer).

## Depedent Functions

This section is where I found most of the value in this chapter.

- If one function calls another they should be vertically close with the caller being above the callee if possible.
- All too often I would write and refactor till I was happy with what I've produced but would skip this additional step of making sure things _flow_.
- Calling back to previous chapters - putting methods or functions in this order helps the code read nicely and one concept flows into another related concept.
- avoids having to ping-pong up and down a class/file
- Adds the benefit of then being able to skim files for what we're looking for - if things are named well that is.

## Horizontal Formatting

- Not much to see here - lots of standards out there already and IDEs will typically align with what is mostly accepted.
- Horizontal openness and density
  - operators should get spaces, function args should be spaced after commas.
  - most style linters can pick up on this stuff - you team just needs to agree on what's best.
- Horizontal alignment
  - just don't do it - I hate running across code that trys to align everything into columns. It's not easier to look at and it's harder to maintain.
- Indentation
  - again, not much to report here. There are alot of language standards that can be applied to linters - you and your team just need to agree on which standards to use.
  - inlining things as a short cut is usually a bad idea anyway - it's fine to break things out with extra lines and parens.

That pretty much covers chapter five. Like I noted, too often, a lot of this can be/is taken care of by modern tooling. The real trick is getting your team to agree on which standards to follow and what extra inclusions or exceptions are reasonable. The final subsection of the chapter covers the team agreement. The most important part of _all_ of that is that someone coming into the system has the confidence that the standards are being enforced and can therefore read according to them.

On to chapter six!
