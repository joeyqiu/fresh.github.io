# Idiomatic Redux: Redux Toolkit 1.0

*A look at the motivations, design, and goals for Redux Toolkit*



**Today I am \*extremely\* excited to announce that:**

**[Redux Toolkit 1.0 is now available!](https://github.com/reduxjs/redux-toolkit/releases/tag/v1.0.0)**

> Note: Redux Toolkit was formerly known as "Redux Starter Kit", but [we renamed it to clarify the intended usage and audience](https://github.com/reduxjs/redux-toolkit/releases/tag/v1.0.4).

As part of that, I'd like to look back at how Redux Toolkit came to be, talk about my goals and vision for the project, and look at how we've designed the API to fulfill those goals.

As a TL;DR, **Redux Toolkit is designed to:**

- **Make it easier to get started with Redux**
- **Simplify common Redux tasks and code**
- **Use opinionated defaults guiding towards "best practices"**
- **Provide solutions to reduce or eliminate the "boilerplate" concerns with using Redux**

I'm proud to say that we've accomplished all those goals!

#### Table of Contents

- Origins
  - [The Rise of Redux](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#the-rise-of-redux)
  - [Facing the Hype Cycle](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#facing-the-hype-cycle)
  - [Dealing with Complexity](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#dealing-with-complexity)
  - [In Search of a Solution](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#in-search-of-a-solution)
  - [Jumpstarting the Process](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#jumpstarting-the-process)
- Designing the Toolset
  - [Early Implementation and Naming](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#early-implementation-and-naming)
  - [Filling Out Functionality](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#filling-out-functionality)
  - [Defining a Roadmap and Clarifying the Vision](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#defining-a-roadmap-and-clarifying-the-vision)
  - [Honing the API](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#honing-the-api)
- The Future of Redux Toolkit
  - [Review](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#review)
  - [Reception](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#reception)
  - [Next Steps](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#next-steps)
- [Further Information](https://blog.isquaredsoftware.com/2019/10/redux-starter-kit-1.0/#further-information)

## Origins

### The Rise of Redux

Redux came out in the summer of 2015, at the tail end of the "Flux Wars". Within a year, it had swept aside all other Flux implementations, and became the de-facto standard state management tool for React applications.

Redux was [deliberately designed to be extensible](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-1/#redux-should-be-as-extensible-as-possible), and it succeeded brilliantly. The design led to [a thriving ecosystem of addons and related libraries](https://blog.isquaredsoftware.com/2017/09/presentation-might-need-redux-ecosystem/), ranging from [action/reducer generation utilities](https://github.com/markerikson/redux-ecosystem-links/blob/master/action-reducer-generators.md) to [store persistence](https://github.com/markerikson/redux-ecosystem-links/blob/master/store-persistence.md) to [dozens of immutable update utilities](https://github.com/markerikson/redux-ecosystem-links/blob/master/immutable-data.md) to [middleware for manipulating dispatched actions](https://github.com/markerikson/redux-ecosystem-links/blob/master/middleware.md). In particular, since Redux deliberately did not include a built-in solution for managing async behavior and side effects, [dozens of side effect addons](https://github.com/markerikson/redux-ecosystem-links/blob/master/side-effects.md) appeared.

In short, if you needed to do something with Redux, odds were an addon already existed for your use case, and it was possible to build something yourself otherwise.

### Facing the Hype Cycle

Most major technologies go through [the hype cycle](https://en.wikipedia.org/wiki/Hype_cycle). A new product comes out, and early adopters fall in love with it and tout it as the solution to every problem. As more people use it, they begin to run into limitations, and the negative feedback becomes noticeable. Eventually, enough people figure out the best ways to use it, and it settles down as a viable tool with known tradeoffs.

Redux is no exception to this. It quickly became associated with React, and many tutorials and discussions assumed that "if you're using React, you must be using Redux too". This became something of a self-fulfilling prophecy, as new learners would read those tutorials and assume they *had* to use Redux with React. Or, in many cases, senior devs might *tell* other devs that they had to.

As a result, **many people began to learn Redux without understanding the context of why it was originally created, what problems it was meant to solve, and what the tradeoffs were. Blindly using \*any\* tool without understanding is a recipe for trouble.** While many people were happy using Redux, many others began to complain about flaws, both actual and perceived.

### Dealing with Complexity

There are several factors that combine to make it relatively harder to use Redux in comparison to other options:

- Redux deliberately adds a level of indirection to the idea of updating state, in the form of actions. [As Dan said early on](https://twitter.com/dan_abramov/status/733742952657342464):

> [Redux] is not designed to be the most performant, or the most concise way of writing mutations. Its focus is on making the code predictable.

- [Common conventions around Redux add additional levels of indirection and code that "needs" to be written](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-2/) (action creators, action type constants, selectors, thunks, switch statements, immutable updates).
- There's also a lot of "rules" around Redux that are described in written form, but not obviously enforced by Redux itself, particularly immutability, serializability, and avoiding side effects.
- JS is a mutable language, and [writing correct immutable code is hard](https://redux.js.org/recipes/structuring-reducers/immutable-update-patterns), both in terms of avoiding accidental mutations and the amount of code that has to be written to handle updates immutably
- Redux was always meant to be a small core supporting a larger ecosystem of addons. This means that the core is deliberately tiny, and it's up to the user to choose how to configure the store. This is great for flexibility, but means that even the most common tasks require multiple semi-complex lines (such as adding thunk middleware for async logic, and also setting up the DevTools Extension). It also means the user has to make decisions early on about which addons they want to use, even in a simple app.
- Functional programming concepts are foreign to most people and hard to grok, especially if you're coming from an OOP background

Like most early adopters, many early Redux users were "power users". Configuration, customization, and "implementing things by hand so I know exactly what's happening" were key selling points. However, Redux took off across a larger userbase, those early selling points became weaknesses. Many people saw the patterns and practices as simply "boilerplate" that was a pain to deal with, and reasons to look for other options.

In addition to the minimum *required* complexity of dispatching actions and writing reducers, the *incidental* complexity became a problem. For example, the original Redux docs divided code up into files and folders by "actions", "constants", "reducers", and "containers". [There are valid reasons to split your code into files by type](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-2/#defining-actions-action-creators-and-reducers-in-separate-files). But, at the same time, [there's no *requirement* that you must write actions, reducers, and constants in separate folders/files](https://redux.js.org/faq/code-structure#what-should-my-file-structure-look-like-how-should-i-group-my-action-creators-and-reducers-in-my-project-where-should-my-selectors-go). Because it was in the docs, most people copied what they saw, and this did become the standard pattern.

Similarly, writing action types as constants like `const ADD_TODO = "ADD_TODO"`, or the idea of having action creator functions for every single action type, are not *required* either. But, [those were the *recommended* patterns](https://redux.js.org/faq/actions#why-should-type-be-a-string-or-at-least-serializable-why-should-my-action-types-be-constants), and users followed the examples in the docs.

On top of that, the need to make decisions about what addons to use has frequently resulted in decision fatigue. The plethora of side effects libs (each with their own terminology) has been a particular pain point. Even within a year of Redux's release, [it was evident that the rapidly growing list of side effects libs was confusing](https://medium.com/javascript-and-opinions/redux-side-effects-and-you-66f2e0842fc3).

While the ecosystem has [largely settled on thunks, sagas, and observables](https://blog.isquaredsoftware.com/2017/09/presentation-might-need-redux-ecosystem/) as the standard approaches, the situation has made ["how do I choose a side effects middleware?"](https://redux.js.org/faq/actions#what-async-middleware-should-i-use-how-do-you-decide-between-thunks-sagas-observables-or-something-else) one of the most frequently asked questions. (Ironically, [thunks were originally built into the core, but removed to make things flexible and unopinionated](https://github.com/reduxjs/redux/pull/63#issuecomment-110575809)). Clearly, there's a need for an official blessing on how to handle side effects.

### In Search of a Solution

I got involved with Redux when I volunteered to write [the Redux FAQ](https://redux.js.org/faq) in early 2016. Dan handed over the maintainer keys to myself and Tim Dorr that summer, and I found myself answering Redux questions everywhere across the internet.

By spring 2017, I realized that the programming landscape had changed enough that most new Redux users didn't understand the context behind its creation or why the typical usage patterns existed. I attempted to remedy that by writing a pair of comprehensive blog posts: **[The Tao of Redux, Part 1: Implementation and Intent](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-1/)**, and **[The Tao of Redux, Part 2: Practice and Philosophy](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-2/)**, which covered how Redux was originally designed and how it was meant to be used.

Around the same time, I filed **[Redux issue #2295: Request for Discussion: Redux "boilerplate", learning curve, abstraction, and opinionatedness ](https://github.com/reduxjs/redux/issues/2295)**. In that issue, I asked:

> - What would idiomatic Redux usage with "less boilerplate" look like? How can we provide solutions to those complaints?
> - What possible abstractions could be created that simplify the process of learning and usage, but without actually hiding Redux (and would hopefully provide a migration/learning path to "base" Redux)?

The very first reply was from [Justin Falcone (@modernserf)](https://github.com/reduxjs/redux/issues/2295#issuecomment-287625366), who suggested:

> - official redux-preset package containing redux, react-redux, redux-thunk already wired together
> - built-in jumpstate-style reducer creators, e.g. createReducer({[actionType]: (state, payload) => state}, initState)

**That suggestion was the genesis for what would eventually become Redux Toolkit.**

The thread continued on for over a hundred comments, eventually devolving into users just pasting links and advertisements for their own Redux abstraction libs. In the middle of the thread, Dan Abramov dropped by, and [laid out his own wishlist for an opinionated layer on top of Redux](https://github.com/reduxjs/redux/issues/2295#issuecomment-287960516):

> If it were up to me I’d like to see this:
>
> - A Flow/TypeScript friendly subset that is easier to type than vanilla Redux.
> - Not emphasizing constants that much (just make using string literals safer).
> - Maintaining independence from React or other view libraries but making it easier to use existing bindings (e.g. providing mapStateToProps implementations).
> - Preserving Redux principles (serializable actions, time travel, and hot reloading should work, action log should make sense).
> - Supporting code splitting in a more straightforward way out of the box.
> - Encouraging colocation of reducers with selectors, and making them less awkward to write together (think reducer-selector bundles that are easy to write and compose).
> - Instead of colocating action creators with reducers, getting rid of action creators completely, and make many-to-many mapping of actions-to-reducers natural (rather than shy away from it like most libraries do).
> - Making sensible performance defaults so memoization via Reselect “just works” for common use cases without users writing that code.
> - Containing built-in helpers for indexing, normalization, collections, and optimistic updates.
> - Having built-in testable async flow support.
>
> On top of core, of course, although it’s fine to brand it in as an official Redux thing.

The thread eventually fizzled out, but the discussion stayed in the back of my mind.

### Jumpstarting the Process

In February 2018, [Nick McCurdy filed an issue suggestion adding freezing of state in dev to avoid accidental mutations](https://github.com/reduxjs/redux/issues/2858). In that thread, I commented:

> This goes back to my comments in #2295 , about wanting to provide a "starter preset" of packages that would provide a Redux-team-recommended simplified store setup. I've seen dozens of variations on the theme, as well as "CRA + Redux" pieces out there already, but we haven't "blessed" any of them.
>
> The real issue here is that the Redux core comes completely unconfigured out of the box. We make no assumptions about what middleware you might add in, or even if you're going to use middleware at all. So, we can't just add a dev middleware out of the box, because nothing is configured out of the box.

In response, Tim Dorr opened **[Redux issue #2859: RFC: Redux Starter Kit](https://github.com/reduxjs/redux/issues/2859)**, and said:

> Based on comments over in #2858 and earlier in #2295, it sounds like a preconfigured starter kit of Redux core + some common middleware + store enhancers + other tooling might be helpful to get started with the library.
>
> As for what goes in a starter package, the short list in my mind includes one of the boilerplate reduction libraries (or something we build), popular middleware like thunks and sagas, and helpful dev tooling. We could have a subset package for React-specific users. In fact, I started a Twitter poll to find out if that's even necessary (please RT!).
>
> I don't think we need to build a Create React App-like experience with a CLI tool and all that jazz. But something out of the box that gives a great dev experience with better debugging and less code.

Seeing the issue got me incredibly excited, and [that led to a *very* long discussion on Reactiflux](https://discordapp.com/channels/102860784329052160/103538784460615680/418590808992645133) with Nick McCurdy and Harry Wolff, as we tossed around ideas for what this "starter kit" thing might look like. Afterwards, [Harry summarized the ideas](https://github.com/reduxjs/redux/issues/2859#issuecomment-369467568):

> - wrapper around createStore with better defaults, i.e. easier way to add middleware, redux dev tools
> - built in immutable helper (immer?)
> - createReducer built in (the standard "reducer lookup table" utility)
> - reselect built-in
> - action creator that adheres to FSA
> - async action support / side effect support (flux, saga, what?)

The discussion continued in the issue. I had some time free the next day, and [put together a notional first sample implementation of `configureStore` and `createReducer`](https://github.com/reduxjs/redux/issues/2859#issuecomment-369816441).

Two days later, [I published `@acemarke/redux-starter-kit` as an experimental package](https://github.com/reduxjs/redux/issues/2859#issuecomment-370182309), and Redux Starter Kit (the original name) was born!

## Designing the Toolset

### Early Implementation and Naming

Work on RSK advanced intermittently over the next few months. Nick filled out the build setup and initial unit tests, added CI integration, and enabled the Redux DevTools in `configureStore`. I added re-exports from Redux and the `selectorator` library, and oversaw discussion and improvements to middleware setup. But, after the initial burst of activity in March and April 2018, RSK just sort of sat around for the next few months.

I came close to burning out in the summer of 2018 due to too many self-assigned responsibilities, such as maintaining my links lists. After taking some time to evaluate, I decided I really needed to focus on "maintainer" responsibilities. As part of that, I decided it was time to start pushing RSK forward seriously.

The initial name of "Redux Starter Kit" was notional, and part of why I'd originally published it as a personal scoped package. In late August, I opened up [an issue to bikeshed over what the final name should be](https://github.com/reduxjs/redux-toolkit/issues/36). We debated various options, including using a `@redux/` scoped prefix.

The discussion trailed off until early October, when someone suggested we just go with plain `redux-starter-kit`. The problem was that the unscoped `redux-toolkit` package name was already taken on NPM.Just for kicks, we tried pinging the original owner (@shotak) and asked if they would be willing to donate the `redux-toolkit` package name. To my surprise, they replied within a few hours and said yes. They transferred ownership of the NPM package, I moved the repo over to the `reduxjs` org, and published the first official version as v0.1.0.

### Filling Out Functionality

The `createReducer` and `createAction` functions we'd already added were useful, but I wanted to find more ways to simplify things.

We'd already been discussing [possibly bringing in helper functions from packages like `redux-utilities`](https://github.com/reduxjs/redux-toolkit/issues/17).

I'd seen dozens of various similar libs. One that had stuck out to me was [Eric Elliott's `autodux` package](https://github.com/ericelliott/autodux). In particular, `autodux` allowed users to auto-generate action creators and action types based on an object full of reducer functions and a "slice name" prefix.

I'd met Shawn Wang at ReactRally in August and told him about my ideas for RSK, and [he wrote up an assessment of `autodux` and several other packages](https://github.com/reduxjs/redux-toolkit/issues/17#issuecomment-414091942), concluding:

> using autodux creates a fairly powerful set of creators and selectors and reducers out of the box, and you can further write actions for business logic easily. i like autodux.

Ultimately, `autodux` served as the direct inspiration for the most useful part of Redux Toolkit: [the `createSlice` function](https://redux-toolkit.js.org/api/createSlice).

Eric Bower (@neurosnap) was participating in those discussions, and made [an initial attempt to port `autodux` into our own `createSlice`](https://github.com/reduxjs/redux-toolkit/pull/37). The PR stalled a bit. Shortly thereafter, [I saw someone on Reddit say they'd written a library called `robodux` that used Immer](https://www.reddit.com/r/javascript/comments/9fu82r/the_rise_of_immer_as_immutability_library_in_react/e5zmo3q/), and after some confusion, I realized that Eric had opted to try putting together his own version as a separate lib written in TypeScript.

After some further discussion, [Eric agreed to contribute his code back to RSK in whatever form worked best for us](https://github.com/reduxjs/redux-toolkit/issues/17#issuecomment-428420190). I ended up compiled his TS code to plain JS and porting that back over to RSK.

### Defining a Roadmap and Clarifying the Vision

In early December, we [set up a Docusaurus-based docs page for RSK](https://github.com/reduxjs/redux-toolkit/issues/48) as a testbed for converting the Redux docs.

Meanwhile, I added [default dev middleware for detecting mutations and non-serializable values](https://github.com/reduxjs/redux-toolkit/pull/63), and enabled [the new Redux DevTools Extension "action stack trace" feature](https://github.com/reduxjs/redux-toolkit/pull/64) that I'd helped to implement.

In late January, I filed **[RSK issue #82: Roadmap to 1.0](https://github.com/reduxjs/redux-toolkit/issues/82)**, and began [adding the ability for slices to listen to other actions](https://github.com/reduxjs/redux-toolkit/pull/83). In that PR, Denis Washington (@denisw) left [a comment suggesting that `createSlice` be dropped entirely](https://github.com/reduxjs/redux-toolkit/pull/83#pullrequestreview-194450952).

In response, I put together a very long comment, laying out **[My Vision for Redux Starter Kit](https://github.com/reduxjs/redux-toolkit/issues/82#issuecomment-456261368)**. I'll skip quoting the entire thing (which is worth reading), but here's the key section:

> #### Objectives
>
> Per the README, other specific inspirations are Create-React-App and Apollo-Boost. Both are packages that offer opinionated defaults and abstractions over a more complex and configurable set of tools, with the goal of providing an easy way to get started and be meaningfully productive without needing to know all the details or make complicated decisions up front. At the same time, they don't lock you in to only using the "default" options. They're useful for new learners who don't know all the options available or what the "right" choices are, but also for experienced devs who just want a simpler tool without going through all the setup every time.
>
> **I want Redux Starter Kit to be the same kind of thing, for Redux.**
>
> #### **Speed Up Getting Started**
>
> ... I want to give people a way to get working Redux running as fast as possible.
>
> #### **Simplify Common Use Cases and Remove Boilerplate**
>
> I want to remove as many potential pain points from the equation as possible:
>
> - Reduce the amount of actual physical characters that need to be typed wherever possible
> - Provide functions that do some of the most obvious things you'd normally do by hand
> - Look at how people are using Redux and want to use Redux, and offer "official" solutions for what they're doing already
>
> #### **Opinionated Defaults for Best Practices**
>
> Redux Toolkit should set things up in ways that guide the user towards the right approach, and automatically prevent or detect+warn about common mistakes wherever possible.
>
> #### **No Lock-In / Add It Incrementally**
>
> If someone starts an app using Redux Toolkit, I don't want to prevent them from choosing to do some parts of the code by hand, now or later down the road.
>
> On the flip side, someone should be able to add Redux Toolkit to an existing Redux application, and begin using it incrementally, either by adding new logic or replacing existing logic incrementally.
>
> #### **Become the "Obvious Default" Way to Use Redux**
>
> Long-term, I'd like to see RSK become the "obvious default" way for most people to use Redux, in the same way that Create-React-App is the "obvious default" way to start a React project.
>
> #### **Don't Solve Every Use Case, and Stay Visibly "Typical Redux"**
>
> Redux is used in lots of different ways. Because of that, and because the core is so minimal, there's thousands of different Redux addon libraries for a wide spectrum of use cases. We can't solve all those ourselves, and we won't solve all those ourselves.
>
> #### **Reasonable TypeScript Support**
>
> I want to support people using TypeScript, same as I want to support anyone else using Redux in specific ways. So, RSK should be reasonably well typed. At the same time, I don't want "perfect typing" to become the enemy of "good functionality". I'm fine with trying to shape things so they work well in TS, but I'm very willing to ship a feature that has less-than-ideal typing if that's what it takes to get it out the door.

### Honing the API

Simultaneous with this discussion, Denis was busy [converting the RSK codebase to TypeScript](https://github.com/reduxjs/redux-toolkit/pull/73), based on a fork from @Dudeonyx. @Jessidhia also chimed in with several good pieces of TS advice.

The [React-Redux v7 rewrite](https://github.com/reduxjs/react-redux/issues/1177) completely occupied my attention for the next couple months. The [hooks API design process](https://github.com/reduxjs/react-redux/issues/1179) kept me mostly occupied after that.

Meanwhile, we had ongoing discussions on things like [possible additions to `createSlice`](https://github.com/reduxjs/redux-toolkit/issues/91), like maybe a special `combineSlices()` function, or whether the auto-generated selector functionality was really useful.

By summer 2019, the React-Redux work was finally done, and I was ready to turn my attention back to RSK. I began [writing an actual set of tutorial docs pages](https://github.com/reduxjs/redux-toolkit/pull/164), finishing in early September.

Meanwhile, Lenz Weber (@phryneas) had started contributing a number of key improvements, [adding a `prepare` callback to `createAction`](https://github.com/reduxjs/redux-toolkit/pull/149), and [rewriting the TS type definitions for readability](https://github.com/reduxjs/redux-toolkit/pull/168).

From there, I began nailing down the final API. This included new functionality like [allowing customization of the default middleware](https://github.com/reduxjs/redux-toolkit/pull/192) and [exporting the case reducers](https://github.com/reduxjs/redux-toolkit/pull/209), and breaking changes such as [removing selectors from `createSlice`](https://github.com/reduxjs/redux-toolkit/pull/193) and [renaming the `slice` field to `name`](https://github.com/reduxjs/redux-toolkit/pull/197).

There were still plenty of other ideas for more additions, but [I concluded that anything else could be added post-1.0](https://github.com/reduxjs/redux-toolkit/issues/82#issuecomment-542773046).

And that leads us up to **the official release of Redux Toolkit 1.0!**

### Changing the Name

I've been going back and forth between the names "Redux Starter Kit" and "Redux Toolkit" in this post. We did call it "Redux Starter Kit" up through the actual 1.0 release. But, after the release, we began getting feedback that the word "Starter" was confusing people. They assumed that it was a CLI project creation tool, a cloneable boilerplate project, something that was only suitable for beginners, or something that you'd outgrow later in a project.

I didn't want to rename the package, but it was clear that this confusion was going to continue.

I opened up [another thread to discuss naming and marketing](https://github.com/reduxjs/redux-toolkit/issues/246). After some additional discussion, we concluded that **"Redux Toolkit"** was the best option, and that we should publish it as a scoped **`@reduxjs/toolkit`** package name.

With that decision made, I was able to push through the rename process fairly quickly, and published the renamed package as **[Redux Toolkit v1.0.4](https://github.com/reduxjs/redux-toolkit/releases/tag/v1.0.4)**.

## The Future of Redux Toolkit

### Review

Looking back at the original ideas and goals that came up in those early discussion issues, I think we've pretty much nailed a majority of the desired capabilities.

RTK is:

- an official opinionated package, with our recommended best practices built in
- that drastically simplifies Redux apps and reduces "boilerplate" for the most common use cases
- while still remaining "visibly Redux" in the process
- that can all be added to a new project on day 1, or used for incremental migration of an existing project
- is useful for both new learners and experienced developers
- and provides solid TypeScript support without requiring a painful amount of type declarations

I'd say we've hit the sweet spot with that list!

### Reception

I've been actively advertising Redux Toolkit for the past year. It's linked on the front page of the Redux docs, it's recommended in the ["Configuring Your Store" docs page](https://redux.js.org/recipes/configuring-your-store), and I've been telling almost literally everyone I talk to that they should try out RTK :)

I've been excited to see that as a direct result of my efforts, [Redux Toolkit has been picking up some actual adoption](https://npm-stat.com/charts.html?package=redux-toolkit&from=2018-07-01&to=2019-10-23). Starting at effectively 0 a year ago, it's now at 100K NPM downloads a month and climbing at a solid pace. This is even more impressive given that it's been pre-1.0 until now.

I've also been thrilled at the reception for RTK. I've received numerous comments telling me things like:

> - [Honestly your starter thing should have been the advertised api all along. It’s 💯](https://twitter.com/AdamRackis/status/1104434428661678080)
> - [hijacking this to confirm how the redux-toolkit is literally indispensable, from a beginner to an advanced level, the combination of createSlice and immer is just so good](https://twitter.com/AlaaZork/status/1153124968856788994)
> - [I am in the business of building things and having less code to maintain, RTK checks all my boxes.🙏 thank you for making it](https://twitter.com/ryan_castner/status/1170056095458635776)

I'll admit to a certain amount of pride, in that RSK has basically been my idea, I've driven it, and I've written a very large portion of the code. So, yeah, it feels great to hear all the compliments.

But, even more than that, **I'm excited for how much this is going to help people learning and using Redux**.

### Next Steps

We're currently planning [a major revamp of the Redux docs](https://github.com/reduxjs/redux/issues/3313#issuecomment-450601554), and I'm hoping to finally begin working on that now that RTK 1.0 is out.

As part of that docs revamp, **we plan to actually teach using RTK as part of the tutorial sequence, and emphasize that we recommend its use throughout the rest of the docs**. We're also going to be reworking the tutorials and other docs sections to emphasize simpler patterns wherever possible, like avoiding use of action constants, dropping the emphasis on writing React "container components", and recommending [the "ducks pattern" for organizing Redux logic](https://github.com/erikras/ducks-modular-redux).

Feature-wise, there's also plenty more we could consider adding, such as [maybe having a built-in way to define async side effects in slices](https://github.com/reduxjs/redux-toolkit/issues/76).

**If you've got any feedback, I'm always interested. Ping me at [@acemarke on Twitter](https://twitter.com/acemarke/) and in the Reactiflux chat channels.**

## Further Information

- [Redux Toolkit docs](https://redux-toolkit.js.org/)
- [The Tao of Redux, Part 1: Implementation and Intent](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-1/)
- [The Tao of Redux, Part 2: Practice and Philosophy](https://blog.isquaredsoftware.com/2017/05/idiomatic-redux-tao-of-redux-part-2/)
- [Redux issue #2295: Request for Discussion: Redux "boilerplate", learning curve, abstraction, and opinionatedness](https://github.com/reduxjs/redux/issues/2295)
- [Redux issue #2859: RFC: Redux Starter Kit](https://github.com/reduxjs/redux/issues/2859)
- [RTK issue #17: Consider using packages from redux-utilities](https://github.com/reduxjs/redux-toolkit/issues/17)
- [RTK issue #82: Roadmap to 1.0](https://github.com/reduxjs/redux-toolkit/issues/82)
- [Comment: My Vision for Redux Starter Kit](https://github.com/reduxjs/redux-toolkit/issues/82#issuecomment-456261368)