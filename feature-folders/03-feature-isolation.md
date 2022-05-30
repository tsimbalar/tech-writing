# Going further with feature isolation

Once we we set with our basic folder structure, we started to dig a little bit deeper.

While our initial "division" felt about right, taking a closer look revealed a few suprises... We noticed in a few places something like this :

```dart
// contents of file 'lib/features/backup/backup_upload_bloc.dart'
import 'package:mindset_depression/features/auth/tenant_config_state.dart';
```

What you can see here is :

- a file under feature **_X_** (here `backup`) ...
- ... importing a file from feature **_Y_** (here `auth`)

This is not the end of the world, but this came as a surprise. When we moved files around to group them by feature, our assumption was that our features were properly isolated from each other ... and it turns out they weren't. Moving actually **made the dependency more apparent**.

## Why should I care about feature isolation ?

We want our feature code to be isolated from other features... but why ?

Feature code is the code that supports the use cases of a functionality. This can include for instance pages, individual widgets, `BLoC`s to support the possible use cases, interaction with the backend-counterpart of the functionlity, types that we pass around as part of the functionality.

What we'd like is to be able to move as quickly (or more!) on our 10th feature as we did on the very first one. We want to make it available as soon as possible so our users can try it and we can start gathering feedback. Once the codebase reaches a certain size, this is not so easy.

By more strongly 

low level of abstraction
Sandbox
Friction between features
High cohesion within a single feature / Low coupling between different features.

## Detecting feature coupling

When looking at our import statements in the previous example, we detected "feature coupling", and as a general rule of thumb, this is what we want to avoid.

We initially detected manually through exploration and inspection, but it would be nice to automate it so we can :

- fix it in the current state of our codebase
- prevent it from happening again during future developements

TODO: a bit about relying on Automated Test to do self inspection and detect some drift / infractions

This translates to "forbidding" importing code of `feature_a` from code belonging to `feature_b`.

![feature import rules](assets/feature-import-rules.png)

beyond the manual folder isolation
are they really isolated ?
why do we care so much about isolation ?
