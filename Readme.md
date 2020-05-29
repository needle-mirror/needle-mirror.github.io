## <span class="logo"><a href="https://needle.tools">!=</a></span>unity package mirror

#### [All Unity Packages on GitHub](https://github.com/needle-mirror/)

This project mirrors all packages available in the Unity Package Manager, across all versions. This allows for easier comparison of versions, checking actual changelogs, and forking to do customisations while keeping a relatively safe upgrade path.

#### What does it do?

The [Needle Mirror organization](https://github.com/needle-mirror/) contains all available Unity packages, with all their versions.
This allows for a couple of very nice things:

1. ⚖️ <b>Compare package versions</b> right on GitHub, without having to download first and then see whether you actually need the changes.  
[Example](https://github.com/needle-mirror/com.unity.xr.arfoundation/compare/2019.3/4.0.0-preview.3...2019.2/3.1.3)

1. 🥇 <b>Easily see the order in which packages are published</b>, including branching
(ever wondered why 3.0.1 and 4.1.2 work while 3.1.4 is broken in your project?)  
[Example](https://github.com/needle-mirror/com.unity.xr.arfoundation/network)

1. 📇 <b>Browse changelogs written by the devs</b>. While the technical writers do a great job, the dev changelog is usually more fine-grained and, at least to us, more helpful.  
[Example](https://github.com/needle-mirror/com.unity.cinemachine/blob/master/CHANGELOG.md)

1. 🗡️ <b>Easy forking.</b> You still can follow updates relatively easy by rebasing to a newer version occasionally.  
[Try it now for XR Interaction Toolkit]()

1. 📜 <b>Release overview</b>. The GitHub releases page makes it easy to skim through a lot of package versions and see what happened when.  
[Example](https://github.com/needle-mirror/com.unity.xr.arfoundation/releases)

1. 🔍 <b>Search across all package code</b> with ease, using GitHub's tools.  
[Example](https://github.com/search?q=org%3Aneedle-mirror+9999&type=Code)

#### Why though?!

UPM ([Unity Package Manager](https://docs.unity3d.com/Manual/Packages.html))</b> is great. It solves a lot of pain points of working with Unity — and, while at it, introduces some new ones!
Namely, since it's introduction, a common response to bugs and issues in Unity packages is "well, the source code is in your hands, go fix it yourself".

"Helpfully", package manager allows to embed a package for development, essentially cluttering your entire project / repository with all the files that were so tidily packaged away before.

Also, once you start going that "customization" route, you lose exactly what UPM is great for — updating packages to newer versions with fixed bugs.

For our own projects, we tried to minimize the impact of that in the following way:

1. Whenever changes to a package become necessary, create a new submodule
1. Put the latest version of that package into it as "base"
1. Do your modifications on top, in a branch
1. When a new version becomes available in UPM, go to the branch with the last version, delete everything and put the new version from UPM there
1. Rebase your custom changes onto the new version.

This seems to be the easiest and most comfortable way with the least headache to freely modify Unity-provided packages while still being able to upgrade*.
*: rebase can be easy or hard, depending on how many changes Unity did in the meantime.

(Note that this is easier for some packages where Unity freely provides the source, such as Graphics, Input System and some others - and some of them even accept PRs!)

Turns out we did the above process a couple of times too often over the last months.  
So, we decided to automate it; and make it available on GitHub for everyone.

#### Pain Points with Unity's approach to package releases

<b>No easy way to compare code between versions.</b> While the [Unity Engine C# Reference Code](https://github.com/Unity-Technologies/UnityCsReference) is fully available on GitHub the same is not true for most packages.

<b>No proper changelogs.</b> While all project changelogs say they "follow semantic versioning" the reality is that QA does not seem to be checking this and some changelogs are better than others, while unfortunately some don't make sense at all.
- ❌❌❌
- ❌❌
- [XR Interaction Toolkit](https://github.com/needle-mirror/com.unity.xr.interaction.toolkit/blob/master/CHANGELOG.md) ❌

Here are some notable exceptions:
- [Input System](https://github.com/needle-mirror/com.unity.inputsystem/blob/master/CHANGELOG.md) ⭐⭐⭐ (this changelog is a work of art)
- [Universal Rendering Pipeline](https://github.com/needle-mirror/com.unity.render-pipelines.universal/blob/master/CHANGELOG.md) ⭐⭐
- [Cinemachine](https://github.com/needle-mirror/com.unity.cinemachine/blob/master/CHANGELOG.md) ⭐

<i>Note that the first two are public repos — turns out having your Changelog user-facing and accessible with one click improves internal quality a lot! Who'd have thought!</i>

#### This section is for you, dear Unity QA people

While building this, we stumbled upon a couple of issues with package structure, especially around changelog consistency.  
To clarify: we're putting this here to hopefully improve quality in the future, not to shame anyone. Maintaining and QA'ing this amount of packages is hard.  

1. <b>Incorrect versioning in Changelog</b>(not adhering to SemVer specs)  
Package Version: `0.7.1-preview`  
Changelog Version: `0.7.1`  
[Example](https://github.com/needle-mirror/com.unity.barracuda/releases/tag/0.7.1-preview)  
Sometimes (rarely), it's even worse:
`[0.1.2-preview] - 9999-12-31`  
[Example](https://github.com/needle-mirror/com.havok.physics/commit/65862f557de2d864877fa482426c4a6fc8577b7e)

1. <b>Internal submodules submitted with package releases</b>  
Some packages contain `.gitmodules` and `.git` files.  
These show that internal submodules are used, e.g. for Testing:  
`com.unity.test-framework.performance` contains a .gitmodule with
```
[submodule "Runtime/Data"]
    path = Runtime/Data
    url = ../../../gintautas/unity.performancetesting.data.git
```
Of course it's totally ok that there are submodules for heavy test data, but embedding such a package in a project might cause some issues since the (Unity internal) submodule obviously can't be found.

1. <b>No/empty changelogs</b>  
[Example - very helpful changelog](https://github.com/needle-mirror/com.unity.mobile.buildreport/blob/master/CHANGELOG.md)  
[Example - Changelog TODO](https://github.com/needle-mirror/com.unity.reflect.runtime/blob/master/CHANGELOG.md)  
[Example - no Changelog](https://github.com/needle-mirror/com.unity.incrementalcompiler)

1. <b>Some teams might need more information about package structure</b>  
[Example - four releases to get ~Documentation folder right](https://github.com/needle-mirror/com.unity.google.resonance.audio/blob/master/CHANGELOG.md)  

Overall, quality of Changelogs definitely increases over time — seems more and more teams are aware of how important a good changelog is. Thank you!

#### Open questions

We haven't fully understood how Unity determines which packages to show in Package Manager. 
For example, URP 6.x is only visible in Unity 2019.2, but not in 2019.3 or 2020.1 — package.json only specifies a min version, not a max version.  

If someone knows, please tell us on Twitter!

#### Known Issues

Currently, it's not easy to see:  
- <b>which Unity version is supported by a specific package</b>  
<i>you can still see the minimal version from inside package.json, but not maximal version</i>

- <b>which package versions have been unpublished again.</b>  
<i>we might add a tag for that at a later point</i>

- <b>package keywords</b>  
<i>we'll have to add them manually as repository topics</i>

##### Packages aliases
<i>These have been renamed at some point</i>
```
com.unity.renderpipelines = com.unity.renderpipelines.core  
com.unity.spriteshape = com.unity.2d.spriteshape  
com.unity.oculus = com.unity.xr.oculus  
```

##### Too big for our current automation approach
<i>We will still update these, but less regular / manual</i>  

```
com.ptc.vuforia.engine  
com.unity.burst  
com.unity.xr.googlevr.ios  
```

##### Internal (can't access)
<i>These show as "searchable packages" but aren't actually accessible</i>  
```
com.unity.package-manager-doctools  
```

##### Deprecated
<i>Can't access anymore</i>
```
com.unity.facebook-platform (?)  
com.unity.barracuda.burst (merged into Barracuda package)  
```

#### Statistics (not yet)

We'd love to create some but haven't yet.

#### Code (not yet)

We're gonna release the source code of Needle Mirror soon (after some cleanup, removing credentials etc. etc.).

#### Who did this?!

<b>[needle — tools for unity](https://needle.tools)</b>
[@marcel_wiessler](https://twitter.com/marcel_wiessler)
[@hybridherbst](https://twitter.com/hybdridherbst)

#### Addendum

Oh, and @UnityTechnologies, if you feel this is wrong, please tell us and we'll figure something out. We'd love to take this down if you provide the same level of access, to data that is already publicly available.