---
title: ES6 - Not Your Dad's Javascript
date: 2016-12-20T22:00:00.000Z
description: ES6 - Not Your Dad's Javascript
---

##Preface
To be completely honest javascript is not my favorite language to work in. I find it difficult and temperamental. Of course that can probably be attributed to my background in more explicit server side languages and general inexperience than anything else. With that in mind I decided that my lunch and learn presentation I was set to give at work should be something javascript related. In the interest of giving the group something new to digest I decided on ECMAScript6 and the exciting changes that came along with it. I figured I would write something up to discuss the interesting changes I came across and highlight some of my favorite new features.

##Semantically Defined Classes
Holy moly. Ok, so being someone who came up with and spent a lot of time developing with server side languages the implicit and wide open nature of javascript has always been a struggle for me. I like what I know and what I know is explicit and rigidly defined classes. If you're familiar with object oriented javascript you know that it is a 'classless' prototype system. Creating an object and methods looked like the following:

```javascript
var User = function () {
  //annnnd so yeah, object here
};

User.prototype.setUsername = function () {
  //and well this is your method
};
```

So, this looks ok right? On a small scale it's fine but issues arise as you create larger and more complex applications. Long story short, it doesn't really scale well imho.

######In ES6 we can simply do the following:

```javascript
class User {
  constructor() {
    //do constructor things
  }

  setUsername(name) {
    //and here is your fancy method
  }
}
```

You may react a few different ways to this:

1. You're like me and are used to more explicit languages and this is great
2. You're used to the current methodology and this seismic shift angers you

Hopefully it's not the second because I love this. From what I've read about this it's really just syntactic sugar. This is treated the same as the example above it just looks like something more familiar and intuitive to those of us used to the `class` keyword.

This is definitely more of a traditional object oriented style. Inheritance also becomes easier with this syntax:

```javascript
class EventListener {
  constructor(element, ev, callback) {
    this.element = element;
    this.ev = ev;
    this.callback = callback;
  }

  execute() {
    this.element.addEventListener(this.ev, this.callback);
  }
}

class Load extends EventListener {
  constructor(element, callback) {
    super(element, 'load', callback);
  }

  listen() {
    super.execute();
  }
}
```

You can see this creates a nice `EventListener` base class that we can extend with all of the events we want to listen for in our application.

##Modularity
This is probably my single favorite addition. With native javascript you don't have the ability to write modular code like you do with other more traditional languages. You either have to use a task runner like Grunt or Gulp and combine everything in the correct order into some main.js file or use something like [require.js](http://requirejs.org/) or [curl.js](https://github.com/cujojs/curl). It's just sort of a pain to do and decoupling code is kind of a bedrock principle for writing good software.

ES6 introduces the `import` and `export` keywords that end up being incredibly powerful. Let's take a look:

```javascript
import { Generators } from './helpers/generators';
import { User } from './models/user';

class Task{...}

export { Task };
```

Now we can make values available to other modules natively. No more need for external libraries - just import the values you want (methods, variables, whatever is made available via `export`) and you'll be able to use it in your current module.

For my presentation I wrote an entire task list application (I know, yawn) pretty much the same way I would write any MVC application in PHP. To some people that sentence probably makes their skin crawl but it was really empowering for me to be able to easily apply what I already know and make something quickly.

##Variable Scoping
ES6 introduces the `let` and `const` keywords for scoped binding. These basically work like `var` for variable assignment but with two different scopes. Here's a quick explanation of of the different scopes:

- `const` is an immutable (read only) variable that is block scoped
- `let` is similar to `var` but is block scoped
- `var` is function scoped

Block scoping allows us to now isolate values to smaller blocks of code like conditionals without disturbing the functional scope (aka these values aren't hoisted).

##Parameter Handling
In case you're haven't noticed yet - we're basically covering all the things that ES6 adds that tradition languages already have. Default parameters are definitely one of those things but there is some cool sugar on top of that feature.

######Default Parameters
Now, we can tell our method to honor a default value within the method signature itself - like so:

```javascript
function foo(bar='baz'){...}
```

That's a nice feature but it's fairly standard to all other languages.

######Rest Parameters
That extra sugar that is added is the ability to use what are called `rest` parameters. These `rest` parameters can be set as the last argument of the function and must be prefixed with `...` - this allows for an indefinite amount of additional parameters. The `rest` parameter gets turned into a numerically indexed array of the values you pass to it:

```javascript
function foo(a, b, ...c) {
  console.log(a); //1
  console.log(b); //2
  console.log(c); //[3, 4, 5, 6]
}

foo(1, 2, 3, 4, 5, 6);
```

This is a really deceptively nice feature. It's not totally apparent right off the bat where it would definitely be useful in variable api calls where you're setting an uncertain amount of url parameters.

##Conclusion
This is just the tip of the iceberg when it comes to the changes ES6 is rolling out. If you want to view everything with some good supporting examples I'd suggest going on over to [ES6 Features](http://es6-features.org).

I guess my real take away here is that the standard seems to be moving away from a wide open and less structured language toward something more traditional and familiar to server side devs. I'm mostly wondering how this will affect the current javascript development community - is this something that they'll embrace or is it just too drastically different? I'm also wondering if the explosion in the popularity of node has anything to do with the trend towards more traditional languages. Personally this gets me genuinely interested in doing more work in node and gives some more legitimacy to javascript on the server.

The real downside to all of this is that ES6 is still sort of bleeding edge. That means as of this writing browser support is pretty sparse and is only available in nightly or very new versions. If you're interested in using ES6 now you can use [Babel](https://babeljs.io/) to compile your ES6 code back into compatible ES5 code but it can be a little temperamental.
