# Issues of a growing codebase

## It started well

Back when we started the app, we naively started with the simplest thing that worked. While working on the first few features of the app, we came up with the "typical" building blocks for our app, which we neatly organized in folders.

While working during the first months of the project, we came up overall with this sort of structure :

![initial folder structure](assets/00-initial-folder-structure.png)

You don't need to know much about it, but roughly, we ended up with :

- `bloc` - contains *BLoC*s , which are the classes through which we expose use cases (somethimes called _Application Services_)
- `design_system` - generic `Widget`s that we re-use across the app for a consistent UX
- `environments` - top level configuration to have slightly different behavior depending on the destination of the app (local build vs testing vs official Prod build)
- `models` - simple classes that we pass around
- `pages` - the actual top-level pages of the app (we navigate from one page to another through _routing_)
- `persistence` - all the necessary infrastructure to store user progress in the phone's database
- `program` - the contents of the program the patients go through
- `theme` - a centralized place for the app's look and feel (colors, fonts etc)
- `utils` - little helper functions

And a few top-level files

- `main.dart` - the entry point of the app
- `routes.dart` - the routing for the app, for navigation between pages

## An implicit layered architecture

In a way, what we created was a layered architecture, where we clearly and physically separated :

- Presentation
- Business Logic
- Data

TODO: a diagram with the layers , and folder mapping to the layers

A Layered architecture is quite a classical way of designing systems, and is not a bad thing ("Lasagna code is better than Spaghetti code"), and there are some benefits to be gained from using it :

- It enforces _"Separation of Concerns"_ : by having different folder, you are somehow forced to "split" your feature in UI vs Data vs Logic.
- This helps with testing (you can theoretically have automated tests for your feature without interacting with the UI), this can also help spreading the work if that's what you want
- The rules to follow are fairly simple and it's easy to learn

## A growing system

As we kept working on the app, the codebase grew, and it became harder to work efficiently in there.

### Cognitive load and friction

While initially we had less than 10 files in each of our main folder, this quickly grew.

TODO: How it started / How it was going (some items in a folder / many items in folder)

Just opening one of those folders became a little bit stressful.

They say the number of objects an average human can hold in short-term memory is 7 ± 2.
TODO : source

We went way past it.

By having more files in those folders, there's a lot of **friction** when navigating the codebase. Finding a file gets harder as it gets lost in a bucket full of other unrelated files.

### Feature spread

Typical work in the app is feature work, such as adding a new functionality, or enhancing an existing functionality. You rarely need to work on more than one functionality at once (and if you need to, then are they really distinct functionalities ?).

With our high level folders, this is what we get :

TODO : feature mapping - add colors to files that belong to the same feature

Here, I have painted in the same colour different files that belong to the same feature.

This quickly reveals 2 problems :

- code for a given feature is spread across folders. There is no obvious explicit connection between those files, but you'd end up discovering they are connected by following the `import` statements, or simply because their name somehow relate to the same concepts.
