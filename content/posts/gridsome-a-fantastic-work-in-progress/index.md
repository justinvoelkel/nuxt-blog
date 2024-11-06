---
title: Gridsome - A Fantastic Work in Progress
date: 2021-07-01T01:30:07.396Z
description: My experience using gridsome the jamstack framework for vue
---
It turns out JAMstack frameworks and workflows have become pretty popular lately. What's not to like; you get the benefits of modern JS toolchains and the speed of a static website. So when it came time for my team and I to rewrite two branded websites that would need to hook into some sort of content management system it was a great opportunity to leverage this new stack to our advantage. Our team is comprised of only three contributors and two of have prior experience with Vue so Gridsome was the logical choice. We've released one site already and are progressing quickly on the second so I wanted to take a time out and review the pros and cons we've encountered up to this point. Let's get into it and start with the pros.

## Pros

### You get to work with Vue

Yeah yeah, I know, this is cheating, that's why I am getting it out of the way early. But, I really do love Vue. There's absolutely nothing wrong with React or any other JS framework you can pull off the pile (yes there are enough to create a literal pile at this point) but Vue has a lot going for it. The barrier to entry for less experienced devs is low and Vue 3 represents a major step forward in both performance and flexibility. I also believe that Evan You is an exceptional talent and his vision for the framework and involvement in the ecosystem has pushed it further than most _probably_ expected.

### Yes, it is fast

This is the primary, and expected, benefit of using any JAMstack framework or static site generator. Nothing beats good old static files served from a CDN in full gzipped glory. I guess the news here is that Gridsome lives up to that standard pretty well. However, if you're looking to really stretch the performance limits it _may_ not be the best choice. We've run up against some inefficiencies in our bundle creation that led to less performant output but ultimately what we've gotten out of it is still lightyears better than what we're replacing.

### You get some nice bells and whistles

I couldn't think of a clearer, less idiomatic, way of phrasing this. But, you get some out of the box functionality that really helps performance:
- Data sourcing (important for us because we're hooking into a headless CMS)
- GraphQL data layer (also important for us because our headless CMS choice exposes it's own GraphQL API)
- Automatic codesplitting
- Plugins (_kind of..._ the ecosystem here can leave a bit to be desired...I'll expand in the cons)
- Image optimization (_kind of..._I'll expand in the cons)
- Link prefetching makes everything feel nice and snappy

### Simple deploys

As with other offerings like Gatsby there are plugins and ample documentation around deploying to popular cloud platforms like Netlify, Azure, AWS etc. However, the fact that you're only deploying static files certainly lowers the difficulty of finding hosting that works.

### Preconfigured development environment

This is a standard offering with many modern frameworks. But, the Gridsome development environment, again, lives up to expectation with HMR and other preconfigured webpack goodies.

Ok, so those are most of the positives we've encountered in our time spent with Gridsome. It's been a very positive experience so far. Our most junior team member has picked up both the build, framework, and workflow very quickly. I think that speaks to how well put together Gridsome is and, coupled with Vue, make it very accessible. That being said there are some definite drawbacks we need to cover.

## Cons

### You get to work with Vue 2...not 3 _yet_

As of this writing Gridsome is still dependent on Vue 2. There is an open Github issue to make the transition to Vue 3 which seems to indicate it's being worked on but it doesn't seem like that update is all that close. This isn't a deal breaker by any means but the more time that passes without being up to the latest major version of the framework the less appealing Gridsome is bound to become.

### Image optimization is wonky

Images have been, by far, the biggest pain point for us. The provided directive _g-image_ works only with static local paths. This is expected as it is a webpack limitation, but, there are other issues outside of that known limitation. The dimension props do not seem to work all of the time - we often get images that do not have the provided dimensions applied correctly. Also, there is a quality prop that can be used to adjust image quality but the results are mixed at best. If you're dealing with dynamic paths like we are (having hooked into a headless CMS) the g-image component simply isn't that useful. And, if you follow some of the more prominent advice on how to use g-image with dynamic paths you're liable to bloat your bundles badly. Underneath it all g-image is using webpacks context require to sort out the appropriate path - if you implement this incorrectly you'll be importing large chunks of assets into your components unwillingly.

### The ecosystem is getting off the ground slowly

The maintainers provide a plugins registry where folks can highlight useful plugins they've produced for the framework. Some of these are up to date and really useful - namely the data sourcing plugins for popular cms options. But, many of the other offerings, if you look at the actual github profiles, are abandond or deprecated. This is nothing new in the JS development community at large but it does make the whole thing _feel_ a bit less sticky.

### Builds could stand to be further optimized

This is more of a neutral observation maybe. There are some plugins that can offer further optimization and you have access, via webpack chain, to further webpack configuration options; but out of the box our main js and css bundles were pretty heavy. I'm far from a pro when it comes to these types of optimizations so a more seasoned frontend developer skilled in webpack could probably squeeze more out; but isn't the point of using a framework like Gridsome or Gatsby to save that time you would otherwise spend on optimizing?

### Limited environment support

There is not yet support for buidling for multiple different environments. Namely we're talking about your staging environment. Using the gridsome cli to develop and build means you'll only ever be able to build for 'development' and 'production' respectively. There are ways to work around this - we're using our CI/CD pipeline to copy our staging.env file to .env so we get the appropriate configuration but it's kind of gross.

## Conclusion

Overally Gridsome is a great project with some obvious flaws. But, that's pretty much par for the course with many open source projects. As a manager with a very small team and tight development deadlines it's been a godsend. It's been easy for both myself and my team to pickup and use effectively without a lot of overhead time spent on things like configuration and deployment. I just hope that the maintainers are able to make the move to Vue 3 to help ensure the longer term viability of the project.
