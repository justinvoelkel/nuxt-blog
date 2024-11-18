---
title: Back to Basics - Clean Code Chapter Three
date: 2022-03-24T18:23:46.782
description: Revisiting good practices reading Clean Code by Robert Martin
---

Continuing my journey back into Uncle Bob's _Clean Code_ I've completed chapter three on functions. There is a lot of gold in the points covered here. I found the use of before and after code examples with refactors to be extremely helpful to really draw the distinction between good and bad implementation. Also, in that regard, I really appreciate being given a real world code example as part of the text. One of my biggest issues with these types of technical books is that often times the examples are simple by design to only convey a concept; that is sometimes useful but I also find it difficult to then find practical examples I would apply that concept to. Overall I learned quite a bit and so I'm just going to jump into my notes.

### The Rules:

- The first rule is that _functions should be small_
  - This is such an easy thing to over look. Seriously - it is _very_ easy to write something that accomplishes your goal that is verbose and simply say "well, I don't think this can be broken down so...moving on".
  - Keeping the goal of _small_ functions top of mind can force you to parse out your task a bit different ahead of time.
  - He mentions at the end of the chapter that he often just writes a mess to get something working then applies this concepts after - but I'd argue that keeping a goal of small functions from the start helps inform your approach even if your writing a big ol monolith to start.
- Measurements - he gives some concrete lengths to not surpass
  - should not be over 150 chars long
  - should not be 100 lines long
  - should hardly ever be 20 lines long
- I think the last bullet is the one to really consider. If you're surpassing 20 lines you've probably got at least one opportunity to split things out if not more. If you don't you need to consider what is preventing you from doing so as there is probably a larger issue at play.
- As an aside; I always kind of thought that splitting things out into smaller functions just contributed to overhead required to understand how something worked. If I'm having to continually bounce from a public method down to a handful of private methods that do smaller things - that's just confusing. But when you couple smaller functions with the naming rules covered in chapter two things start to be more explicit and you should only be dipping into those private methods if they're not behaving correctly.
- Indents
  - blocks with `if`, `else`, and `while` statements should be one line long.
  - really, our functions should only be one indentation level deep
  - your functions _should not be large enough to hold nested structures_
  - any more probably signals that you have a potential function call

### Do One Thing:

I've always tried to follow this as a very basic principle. But, honestly - oftentimes it's really hard to draw the line one what _one_ thing you are accomplishing with your function. If you are parsing a string in a function and searching for a given value and returning true if found; is that two distinct things? Even if the parsing and searching can be chained together or they're very short snippets of code in-and-of themselves? We're given some direction on how to deduce if we're doing _more_ than one thing in this subsection.

- FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL. THEY SHOULD DO IT ONLY. - I want a giant print out of this for my wall as a constant reminder.
- Using _TO_ paragraphs
  - Here's the direction on sorting out if you're doing too much. Use the format `TO [function name]...` and follow with plain english implementation.
    - If the function does only the steps one level below the given function name then the function is doing one thing.
      - So, for instance, the string parsing example. If we called the function `searchParsedStringForTerm` we could sort out that it is doing one thing by writing out `TO searchParsedStringForTerm we parse the string by spaces into an array and search that array for the given term. If we find the given term we return true else we return false`.
      - I think an appropriate example of doing more than one thing in the string parsing example would be if we were also seeing if the term that is found is inside of some other data structure. In that instance we'd have to add another condition after searching the parsed string to check if the other data structure included that value. At that point you probably have one function for parsing and searching and one for checking if that value is in the other structure.
  - Another approach is - if you can extract another function from your current function without merely restating it's implementation - you probably are doing more than one thing.
  - One level of abstraction per function
    - We should aim to keep the level of abstraction consistent within a function. The example code given illustrates this well showing high, medium, and very low level details all grouped within one function.
  - I need to also briefly mention the 'broken windows' analogy referenced in this chapter. It's a very real thing that most all of the teams I've worked on face. I think it all comes back to something discussed in chapter 1 - caring. You have to care enough about the code and your future selves to put in the time _now_ to ensure refactors are made and best practices are followed.

### Switch Statements

Here's my honest thoughts - don't use them. As noted it's hard to make a short/small switch statement because they are, by default, verbose. Personally, I also find them hard to read and once you've surpassed a hand full of cases they're even harder to comprehend. That falls in line with most of what's in this subsection but there are good points made for switch statement use cases.

- `switch` statements can be tolerated when they only appear once, are used to create polymorphic objects, and are positioned behind an interface.
  - I've honestly never seen this set up. I've always just seen switch statements thrown in where you need branching logic and a bunch of `if` statements doesn't feel good.
  - After seeing the code though I can see how this can make for a reasonable implementation. At least if this baloons with different cases it's relegated to one spot.

### Use Descriptive Names

Here we are back at naming - not much to add here but it really drives home the concepts delivered in chapter two.

- A long descriptive name is better than a short enigmatic one
- A long descriptive name is better than a long descriptive comment
- I've always thought long names were bad practice. When I first started the Zend Framework naming conventions were basically a punchline. But at this point in my career I can really appreciate clarity over brevity.

### Function Arguments

This is a big one for me. While the concept(s) here may be sort of obvious at first glance the examples given of when it's _ok_ or _reasonable_ to use a diadic or triadic helps to add some contrast and remind us that these aren't concrete rules - don't try to write cartesian points that only take one argument _unless you really want to I guess_.

- Monadic, diadic, and triadic function arguments
  - We should try to limit the number of function arguments always
  - Ideally we should have one but there are cases where two or three may be required and make sense
    - diadics and triadics can be problematic when context is required to remember the order of the arguments
  - When possible utilize members/methods of a class and utilize that classes properties to limit the number of arguments a function requires
  - If a function absolutely requires many arguments it could be a sign you need to abstract that information into it's own class
    - so, instead of `someFunction(foo, bar, baz)` you abstract those values to a class/object and pass like `someFunction(FooBarBaz $foo)`; not horribly well named but you get it.

### A Few More Naming Things

There are small subjections in the chapter that cover a bit more on naming conventions. Specifically, encoding verbs and keywords into function names.

- It could be more explicit to name a diadic function with a verb/keywords encoded into the name directly.
  - for example re-naming `assertEquals` to `assertExpectedEqualsActual` naturally tells the developer that the order of the arguments are `expected` then `actual`

### Side Effects, Separation, and Exceptions

This post is pretty lengthy so I'm going to lump the last few subsections together. However, that is not to imply there is less value here. I would argue that these next few subsections cover some of the most important topics in the chapter.

- Functions should not have side effects
  - You see this everywhere now as a matter of best practice.
  - Avoiding side effects helps the predictability and orthogonality of your code
  - Side effects also create 'temporal coupling' - limiting when and how they are called based on context
- State change
  - If your function needs to change the state of something it should change the state of it's owning object.
- Command query separation
  - functions should either _do something_ or _answer something_ but _not both_.
    - an example is given of a setter method that both sets a value and then returns true - when used in the context of a conditional `if(set('username', 'foo'))` the intent becomes unclear and can be interpreted in many ways.
    - the remedy to this is to extract the command `set` from the query for the value - meaning we should have a function for `attributeExists` (query) and `setAttribute` (command).
- Prefer Exceptions to Returning Error Codes
  - This was something I learned the hard way - when I initially started writing code I didn't really utilize exceptions heavily. After learning the benefits of bubbling exceptions to appropriate levels of the app and effective exception throwing and handling methods I learned the error in my ways.
  - The points presented here about codes/statuses are spot on - going back to some of the previous points; your devs are going to either need an ever growing switch statement to react to different statuses or you'll end up with many (possibly nested) conditionals.
  - Also - if the language you are working in supports multiple catch blocks you get a nice way to specify how to handle certain exceptions if you need to react differently.
    - That sort of logic can even be abstracted to a global handler which decides how to react to exceptions (to avoid growing your catch blocks for too many different exceptions)
- Error handling is one thing
  - I honestly never thought of this before. Generally I would just handle exceptions inside of a function or method with some other business logic.
  - Wrapping function calls in another function that explicitly handles exceptions helps to clean up and streamline things. That way downstream we don't have to actually worry about exception handling - it's propagated up to the appropriate caller who can decide how to act.

The remainder of the chapter covers the concept of DRY principles (which I think most are familiar with) and structured programming which may be considered a bit out of date based on the language limitations it was meant to overcome.

I did find it particularly note-worth that the pentultimate subsection covers _how you write functions like this_. It's somewhat reassuring to know that someone like Uncle Bob doesn't just produce magic in his first pass - he writes what's required to achieve a goal and then begins to refine what he's done. Every time this concept of just writing till it works and refining comes up I'm reminded of the Hemingway quote:

> "The first draft of anything is shit."
>
> -- <cite>Ernest Hemingway</cite>

Also Uncle Bob goes out of his way to tell us it's ok to take this approach:

> "I don't write them [functions] that way [laid out in this chapter] to start. I don't think anyone could"

I found this chapter to be an excellent resource and I am really excited to move on to chapter four on commenting our code!
