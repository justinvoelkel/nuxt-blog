---
title: Back to Basics - Clean Code Chapter Seven 
date: 2022-05-05T19:17:04.795Z
description: Revisiting good practices reading Clean Code by Robert Martin
---

Chapter seven deals with error handling. Another crucial concept that tends to not be utilized correctly. It still surprises me how many code bases I encounter that make little or no use of exception handling.

Right out of the gate we get a best practice reminder: _it can be hard to uncover the intent of code when it's obscured by error handling_. I think it was covered in a previous chapter as well but the concept of wrapping a _caller_ in try/catch/finally and having the _callee_ perform the logic that could end up in an exceptional state is very important.

## Exceptions Not Response Codes
- We should not be returning status/response codes from functions and acting on the codes in the caller
- Utilize exceptions to take action based on one or many exceptional states encountered


## Write Try/Catch First
- exceptions define scope within a program
- It is good practice to write your try/catch first
  - it helps to define what the user of the code can expect
- When it comes to testing try to write tests that force exceptions then add behavior to the handler to satisfy the test

## Use Unchecked Exceptions
- not really my domain

## Provide Context With Exceptions
- exceptions should include a message with some additional information about what happened
- a stack trace is helpful but does not reveal intent of code

## Define Exceptions Based on Callers Needs
- when defining exception classes the most important thing to keep in mind is ***how they will be caught***
- the example code given here illustrates how you can wrap third party code and translate exceptions.
  - this section provided my biggest takeway - that wrapping third party code is a matter of best practice
  - isolating dependencies and creating common wrappers makes the code easier to replace and easier to test

## Define Normal Flow
- wrapping calls and abstracting exception handling pushes error detection to the edges of your system
- the _Special Case Pattern_
  - create a class or configure an object so that it handles a special case for you
  - when done the client does not have to deal with exceptional behavior

## Do Not Return Or Pass Null
- yes yes yes
- when we return null we're just creating work for ourselves
- when tempted to return null consider throwing an exception or returning a special case object instead
- passing null as an argument is worse than returning it
- the conditionals required to validate what's passed in are unwieldly
- most languages do not provide a useful way to deal with null being passed to a function

My notes for this chapter were really short but the it provided a lot of value. I sort of wish it was a lot longer to be honest. The code examples were helpful but I would have found it beneficial to see a few more examples of the special case pattern to really solidify it's usecase.
