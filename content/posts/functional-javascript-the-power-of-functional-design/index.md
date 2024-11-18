---
title: Functional Javascript - The Power of Functional Design
date: 1970-01-01T00:00:00.000Z
description: Functional Javascript - The Power of Functional Design
---

Functional programming seems to be a growing, let's call it 'ideology', within the javascript community recently. It is by no means a new concept, but it is one that is particularly powerful when applied to a weakly typed and more 'lax' language like javascript.

###Context Is King

In the past few years I've really tried to put an emphasis on learning to become a better javascript developer because, if I'm being honest, it is was a language I had struggled with and did not appreciate. The benefits of Javascript were lost on me - anonymous functions, prototypal inheritance _(vs traditional object inheritance)_, callbacks, lexical scope....all of this seemed like strange and chaotic alchemy.

As I've spent more time writing data manipulating, functional code, I've come to really appreciate the power that javascript gives you to solve problems. But, like most things, with that power comes great responsibility. It's easy to let things get out of hand within your javascript code if you're not proficient in thinking about abstractions. It's easy enough to write anonymous callbacks and closures and be done with it because 'it works'.

###Higher Level Thinking
Bridging the gap from more traditional OOP concepts in PHP to javascript was a bit of a bumpy road for me. Trying to write javascript like PHP produced some mediocre results. Exploring ways to improve led me to the concept of **higher-order functions**. It finally clicked that functions are just values and one of the great benefits of javascript is that those functions/values can be easily passed around as arguments and composed into other larger and more complex logic.
