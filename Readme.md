Unity Package Mirror
==

Why though?!
-

UPM (Unity Package Manager) is great. It solves a lot of painpoints of working with Unity - and, while at it, introduces some new ones!
Namely, since it's introduction, a common response to bugs and issues in Unity packages is "well, the source code is in your hands, go fix it yourself".

"Helpfully", package manager allows to embed a package for development, essentially cluttering your entire project / repository with all the files that were so tidily packaged away before.

Also, once you start going that "customization" route, you lose exactly what UPM is great for - updating packages to newer versions with fixed bugs.

For our own projects, we tried to minimize the impact of that in the following way:
1. whenever changes to a package become necessary, create a new submodule
1. put the latest version of that package into it as "base"
1. do your modifications on top, in a branch
1. when a new version becomes available in UPM, go to the branch with the last version, delete everything and put the new version from UPM there
1. rebase your custom changes onto the new version.

This seems to be the easiest and most comfortable way with the least headache to freely modify Unity-provided packages while still being able to upgrade*.
*: rebase can be easy or hard, depending on how many changes Unity did in the meantime.

(Note that this is easier for some packages where Unity freely provides the source, such as Graphics, Input System and some others - and some of them even accept PRs!)

Pain Points with Unity's approach to packages
-
No proper changelogs. While all project changelogs say they "follow semantic versioning" the reality is that QA is not checking this and many, many changelogs don't make sense at all.

There's some notable exceptions:


Our solution
-

Turns out we did the above process a couple of times too often over the last months.  
So, we decided to automate it; and make it available on GitHub for everyone.

This account contains all available Unity packages, with all their versions.
This allows for a couple of very nice things:

1. Easily see the order in which packages are published, including branching
(ever wondered why 3.0.1 and 4.1.2 work while 3.1.4 is broken in your project?)

1. Browse actual changelogs written by the devs and not the stuff the technical writers make out of it (sometimes it's better, sometimes it's worse, but the dev changelog is definitely more helpful to us).

Caveats
-

Currently, it's not easy to see:
- tags of packages
- which Unity version is supported by a specific package
- which package versions have been unpublished again

Prior approaches
-

Contact
-

Addendum
-

Oh, and @UnityTechnologies, if you feel this is wrong, please tell us and we'll figure something out. We'd love to take this down if you provide the same access to data that is already publicly available.