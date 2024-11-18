---
title: AngularJS - Some Simple Yet Effective Directives
date: 2017-04-09T07:20:20.000Z
description: AngularJS - Some Simple Yet Effective Directives
---

Apologies for the corny alliteration - but the title actually works and it was only half intentional. I've been working with AngularJS for roughly three years at this point and anyone who works with it that long is bound to find the need to write a directive or two. It's one of the bigger selling points of the framework - being able to augment your html templates with custom functionality, filters, and basically whatever else you can imagine is not only helpful, it's awesome. So, maybe you're new to Angular, let's quickly go over what directives are and why they're important/cool.

##Directives; Powerful and Cool
If you're new to all of this I'd recommend that you go [checkout the docs](https://docs.angularjs.org/guide/directive" target="\_blank) on all this but I'll try to give a general overview. Directives give you the ability to extend custom functionality into your Angular templates by adding custom elements, attributes, class names, and comments. So you know all of those cool `ng-whatevers` you do inside of your templates? Well, you can write your own and that's powerful and really cool.

That's really the long and short of it. Sometimes they're large and complex sometimes they're short and sweet. We'll talk about the latter. These are just some really simple directives I've found useful in my day to day work that I wanted to share.

###Numeric Only AngularJS Directive
This happens pretty frequently - you need to validate a form and the contents should strictly be numeric. In my case, working on our proprietary CMS, there are a lot of times I need to restrict user inputs for practical and security reasons. This numeric directive is great for making sure that data comes in as expected. It's worth noting that you could easily change this to accept phone numbers or some other type of formatted numeric pattern for your application or site. Let's look at the directive and walk through it:

```javascript
app.directive('numeric', [
  'Flash',
  '$parse',
  function (Flash, $parse) {
    return {
      require: 'ngModel',
      restrict: 'A',
      link: function (scope, element, attrs) {
        scope.$watch(attrs.ngModel, function (val) {
          if (val && isNaN(val)) {
            Flash.show('Value Must Be a Number', 'danger');
            var model = $parse(attrs.ngModel);
            model.assign(scope, '');
          }
        });
      },
    };
  },
]);
```

Ok, so to note - our main application here would be stored in `app` and we're using the `directive` API. The format will look familiar if you've written modules and controllers previously.

Moving left to right the first argument `numeric` is the reference - in this case the attribute's (we'll cover that in a minute) name. We're injecting the `Flash` and `$parse` dependencies - in this case, `Flash` would reference a mythical service that serves up messages to users of the application. Also, `$parse` is going to be used to reference the model on the element we're using the `numeric` attribute on. We want to trip the numeric directive if a letter (or other non-numeric character) is entered, and we want to make sure that we inform the user as well as clearing out the model so they can't submit that bad data.

Alright, so after our dependencies we have our callback. We're defining a `require` option to tell the directive we want `ngModel` in the scope of the directive. This is exactly what we need to clear out the model if the data the user enters is bad. You can use this `require` option for other attributes or data options to pull things from your template as needed. In this case, when we get `ngModel` from the template it's going to be a string which doesn't do us much good - so that's why we're using `$parse` to use that string to reference the original function and clear it's value.

From there, the `restrict` option let's Angular know we only want `numeric` to be invoked as an attribute. You can set this to say `E` if you wanted to reference something as an element like `<numeric></numeric>`.

`link` is the function we want to link to the directive to determine the behavior. Sort of like a callback I guess, it's just an anonymous function that does the work we outline for it. The common signature for that callback is what you see `scope, element, args`. Angular should populate these with the appropriate data from your template.

In our current case we're watching for changes in the current attribute's `ngModel`. When that value is updated, we want to make sure it's numeric, and, if it's not, send the appropriate flash message to the user. The rest you may be able to decipher for yourself. We're doing just that - we're looking at the value, if it's not numeric we're going call the show method on `Flash` to tell the user that they're doing something wrong, we then `$parse` the value of `ngModel` which could be something like `form.foo` and we make sure to set it to an empty string `''`. We just call the assign method to make sure Angular is fully aware that something went wrong.

###Going the Other Way: Alpha Only AngularJS Directive
Continuing the theme of form validation things you may do often - making sure that something is alpha only (be it a name or some other field) pops up a lot. So let's put it in and take a look (spoiler alert - it's going to look pretty similar)

```javascript
app.directive('alpha', [
  '$parse',
  function ($parse) {
    return {
      require: 'ngModel',
      restrict: 'A',
      link: function (scope, element, attrs) {
        scope.$watch(attrs.ngModel, function (val) {
          var reg = /[^-A-Za-z\s]/;
          if (val && reg.test(val)) {
            var model = $parse(attrs.ngModel);
            model.assign(scope, val.replace(reg, ''));
            scope.error = 'Special characters and numbers are not accepted';
          }
        });
      },
    };
  },
]);
```

So, this is going to look super similar to the other example. We're injecting one of our same dependencies `$parse` for the same reasons. We're, again, restricting the directive to only and `A` attribute. And, finally, our callback - we're using a regular expression to test the model value to make sure it's alpha only. If it isn't, we're following a similar path as the previous example and setting the model to an empty string `''`. In this case, I'm setting a `$scope` variable `error` in the template to let the user know that the data is not valid. That's really it - it's very similar to the previous example but I think it illustrates how you can easily re-purpose a script to do something similar.

###Conclusion
Angular directives are awesome. They're extremely helpful in terms of data validation but they can be used to do many more complex things inside of your application. I'd love to do more of these and maybe provide some common recipes to use. If I missed something or got something wrong please let me know down in the comments. As always, thank you for reading!
