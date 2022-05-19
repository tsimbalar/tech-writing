Our journey through the evolution of the architecture of one of our apps : Mindset

**TBD**: links to separate articles

A bit of context about Mindset

What it is for, some screenshots to show the different sections

# Introducing Feature folders in Mindset

About Mindset

- Goal of the app, types of contents etc
- Flutter ?
- General structure, Bloc pattern

How we initially started

- Initial “basic” folder structure
- Clear separation of layers
- This worked ok up to some extent

Defining “pages” vs blocs vs core other stuff

Layers are good

- Very clear where pieces fit
- Easy rules to follow
- Lasagna is better than Spaghetti

But …

Codebase is getting harder to work on

- New features just add files into growing folders
- Navigation gets harder (higher noise vs signal)
- Some features don’t match exactly the typical pattern … where do we put them ? for-fit them ?
- Cognitive load 7±2 items
- Working on a feature means jumping through folders
- Harder for onboarding
- How do I know some code I’m changing is not breaking another feature
- The folder structure doesn’t seem to reflect the structure of the app
  o Screen shot with tab / screens

Currently “grouping but type of things” looks clean, but is impractical
Forces to limit to “types of things that exist” , needs upfront definition of “what types of things exist”

Let’s try something else …

“grouping by purpose” / “feature folders”

Instead of layers, think more about slices

- Inside slices you’ll still find the layers

So we started “grouping things” that worked closely together
… and it started to look nicer

Some things that appeared from there :

- Not all features are born equal … and that’s fine

Some rules of thumb : what is inside a feature folder is specific to this feature … a feature should not reference anything from another feature folder …. If we want to re-use let’s make it explicit, and put that “shared stuff” outside …. This makes it explicit that this is shared (i.e. handle with care, may have impacts in multiple spots of the app) vs feature specific : no need to over-engineer, no need to make something too generic …

This is about COHESION !

Feature folders help with “innovation” , give more confidence / safety … new features won’t break existing features …

# Beyond feature folders - coupling etc

There’s more to life than features

Features don’t exist in the vacuum

Not all code is feature code
Shared stuff
Need to be assembled together at some point

Trying to control the direction of dependencies / directed acyclical graph

App
Infra “shared” stuff
Shared widgets
Core services that features can rely on
Core things that we are fine sharing “extending the dart base library”

Those are not “hard rules” , practicality beats purity ….

# A small architecture lesson

A lesson in architecture
Cohesion
Controlling coupling
Look at afferent coupling
Avoid over-generalization
Start simple / dumb
Only generalize once you have several use cases

# Controlling / enforcing architecture guidelines in our app

Once you have some idea about how you expect to organize your app into features, shared things etc … you can start moving files around …
But with many team members working on the team working hard to ship features, it’s very possible that you’d have to re-do that work in a month or 2…

Wouldn’t it be nice if we could somehow enforce the rules for our folder hierarchy so that we don’t accidentally introduce dependencies between parts that shouldn’t have them?

Going back to Afferent coupling etc …. What we could do is look at all the `import`s in all the files of our codebase, and detect “suspicious” imports …

AFAIK there is no out of the box support for this, but we can probably build our own. After all, we already have (right?) a test suite that validates the behavior of your app, and this test suite is executed often (dev machine, CI pipelines etc) …. Could we add tests that validate the “structure” of our app ?

Blah blah blah small library to do just this

Explaining the concept of rules

- Allow rules
- Vs forbid rules
  “package” imports vs “dart” vs “relative” imports

# Automating architecture

The process of enforcing architecture rules in the codebase

2 approaches :

- Every allowed by default … except some things you don’t want
- Nothing allowed by default, unless explicitly stated

EVERYTHING RED ….
Need a human to decide :

- This import is legit -> add a allow rule (maybe figure out a general pattern rather than import for a particular file)
- This import is wrong -> fix the code
- This import is wrong … but it’s ok -> ad a (temporary?) allow rule

Here is the type of rules we ended up with :

- Some things can be used anywhere (clear abstractions / simple data types / explicitly shared stuff)
- Some things are expected to be used by no one “except X”

Once the baseline is in place, what is happening over the life of the project is :

- Somehow those tests break … and the following happens :
  o Maybe someone asks what those tests mean ? (newer joiner) … and awesome, a learning opportunity to talk about architecture … also maybe an opportunity to document more clearly your rules ?
  o Maybe there’s an agreement that indeed this is legit, and we just add a rule … congrats you have just “documented” + “enforced” a decision
  o Or maybe there’s an opportunity to refactor a little bit to make the test pass

## Some other benefits from those changes

- automation
- looking at test coverage ... we get reports per features ... telling us where we have possibly less coverage ... to balance with how critical / risk / cost of the feature
-
