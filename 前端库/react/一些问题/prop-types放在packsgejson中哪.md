## porp-types 在react库中放哪



Question:

```
Hi Team, I've a question regarding the suggestion made in the readme on how to depend on prop-type:

https://github.com/reactjs/prop-types#how-to-depend-on-this-package

As a library maintainer both react and prop-type should be considered as external dependencies to my libraries as they should come from the environement using them and they should not be merged . So far I always thought that the peerDependency was the correct way of listing those dependencies (react and react-dom so far).
Now I'm a bit confused because you are suggesting to use prop-type as dependencies and not peerDependencies, what makes prop-types different to react ?
```



one Answer:

```
--- gaearon
I think the main difference is the user tends to know which version of React they need, but might not care about prop-types. So letting the user choose with react seems more important. Since peer dependencies are generally a pain, it would be great to reduce the reliance on them, and this seems like a case where it could work.

But I agree the distinction is fuzzy.

--- benwiley4000 
Thanks for the explanation; perhaps some version could make it to the README. I think the key thing to understand is that prop-types will still be bundled during the user app's compile step, not the library's; though unlike with react it's unnecessary that the user app explicitly depend on prop-types.
```



