For teams with big projects, waiting for new builds could eventually take up 10-15% of their workday. This not only wastes precious developer time — it also makes test-driven development extremely tedious, which hurts overall code quality.

Splitting an application into modules could be a solution to this problem. I was tempted to just split our codebase by feature, by layer, or in some other quick and obvious way. 



Build a project has these five steps :

1. **Preparation of dependencies.** During this phase Gradle check that all libraries this module depends on are ready. If this module depends on another one, that module would be built as well.
2. **Merging resources and processing Manifest.** After this phase resources and Manifest are ready to be packaged in the result file.
3. **Compiling.** This phase started with Annotation Processors, in case you use them. Then source code is compiled into byte code. If you are using AspectJ, weaving also happens here.
4. **Postprocessing.** All Gradle tasks with a “transform” prefix are part of this phase. Most important ones are: `transformClassesWithMultidexlist` and `transformClassesWithDex`*.* They produce .DEX files.
5. **Packaging and publishing.** For libraries this stage means creating an .AAR file in the end, for applications — .APK.





It also doesn’t rebuild a module if it wasn’t changed. That leads to the following hypothesis:  “The Incremental build time for a project with multiple modules is faster than for a single-module project, because only modified modules are recompiled.”





code more coupled, and increased compilation time So writing code is very important.

// check above text 

**Small losses here and there**

It’s no surprise that doing something in parallel does slow down the build. Even having a second project open in Android Studio can make build 5–10% slower. Listening to music, watching YouTube, or browsing the internet increases the build time a lot! I personally saw a speeding up of a build process by up to 30% after just closing everything except Android Studio.   


// start  checking :

**./gradlew assembleDebug --profile**
#### Initial State

As a starting point I took a project with a single module that contains all 15 000 classes. For this setup incremental build time is *1m 10s*.

3 Modules
First step is splitting one module into three: one application module and two library ones. Application module depends on libraries, but library modules are independent from one another. Each module has about 5 000 classes and 30 000 methods.

If changes are made only in application module, then build time is about 35 seconds. Almost 30 seconds win compare to initial state. But when one of library module is changed, even if application module is untouched, incremental build time grows to 1m 50s. 40 seconds longer!

Let’s take a look into profile report and see what took so long:

![[Pasted image 20240412205232.png]]

![[Pasted image 20240412205243.png]]


In the screenshots above, you can see that most of the time was spent on building the library module. You might also notice that for library modules, both debug and release tasks were executed. Gradle wasted time executing two sets of tasks instead of one! This is why it took an extra 40 seconds longer than the single-module project.

We can avoid this and make this build even faster than our initial 1m 10s by splitting our code into modules.

But that’s not the only problem. Let’s look into how our application module depends on library modules:

```kts
dependencies {    compile project(path: ':app2')    compile project(path: ':app3')}
```
There’s an important problem in the code above: if a library is added like this, then the application always depends on the release variant, independent of it’s own build type. It also doesn’t matter which variant is chosen in Android Studio. Here’s what the Gradle Plugin User Guide says about all this:
By default a library only publishes its release variant. This variant will be used by all projects referencing the library, no matter which variant they build themselves. This is a temporary limitation due to Gradle limitations that we are working towards removing.

Luckily, it’s possible to change the variant our application depends on.

First, add the following code to libraries’ build.gradle file. It will allow library to publish debug variant as well:

```kts
android {    defaultConfig {        defaultPublishConfig 'release'        publishNonDefault true    }}
```
Second, application module should depend on library ones like this:
```gradle
dependencies {    debugCompile project(path: ':app2', configuration: "debug")    releaseCompile project(path: ':app2', configuration: "release")    debugCompile project(path: ':app3', configuration: "debug")    releaseCompile project(path: ':app3', configuration: "release")}
```


Now the debug variant of our application depends on the debug variant of the libraries, and its release depends on their release. So lets make some changes to the module app2 and rebuild it. After these changes, we can check our profile report again:


![[Pasted image 20240412205506.png]]

![[Pasted image 20240412205522.png]]

The most significant difference is the absence of :app2:packageReleaseJarArtifact, which saves us about 15 seconds. In addition, the time is reshuffled a bit between rest of the tasks, and we end up with 1m 32s. That is 18 seconds faster then before, but still 22 seconds slower then our initial configuration. For changes only in the application module, build time stays almost the same — 36 seconds vs 35 seconds in our previous configuration.


Unfortunately I didn’t found a proper explanation for why it builds both flavors. I hope that once this Gradle limitation is resolved, then this problem will just go away as well. The same issue is discussed in AOSP Issue Tracker.

I’m also planning to spend some time experimenting with Gradle tasks to find a workaround for this problem. One possible way is to exclude all release tasks for debug builds. https://code.google.com/p/android/issues/detail?id=52962


5 Modules
Obviously, build time depends on the amount of code. If you reduce amount of code by half then the build should also be roughly two times faster. If instead of 3 modules project is split in 5, then build time should be approximately 40% faster.

It is almost true.
![[Pasted image 20240412205614.png]]

If changes are made only in application module then incremental build time is approximately 24s. For changes in a library module incremental build takes 50s. Compare to initial 1m 10s that is already 
a win. But I have a few more tricks at my disposal.

Reduce Size of The Application Module
Independently of what module is changed, application module will be recompiled every time. So reducing it’s size makes total sense. Ideally, it should just assemble whole application from a separate modules and might also provide a splash screen, because splash screen often depends on lots of features.

That is how idea about 3+1 and 5+1 configurations was born. In both cases project has a small application module that depends on 3 and 5 library modules accordingly. All library modules are independent from one another and has equal sizes. Let’s see what that gives us:


![[Pasted image 20240412205643.png]]

We can observe further drop in incremental build time. Even with changes in library module 5+1 configuration is build almost twice as fast as an initial single-module project. This is a decent progress.

Why does project actually depends on Butterknife?
This is a point where I have to make a confession. There was one very strong reason to add a dependency on Butterknife.

In the initial configuration incremental compilation takes 45s out of 1m 10s, but if Butterknife is removed then project is compiled in 15s only! Three times faster! Whole incremental build without Butterknife is 40s.

Is it a problem in the library?

As it appeared — no. Without Butterknife project compiles so fast because of actual Java incremental compilation, which is disabled for projects that use Annotation processors. You can find related issues in Gradle Jira, in AOSP Issue Tracker, it is also tracked in Gradle design docs. If you will take a closer look into the issue from ASOP Issue Tracker, one of the comment says:
“Annotation processors is not yet supported with incremental java compilation. It will depend on changes in Gradle.
We disabled it for project that apply com.neenbedankt.android-apt, so it's no longer a significant issue.”

That is why build just becomes slower without a notification.

Personally I won’t remove Annotation processors from the whole project. I find libraries like Dagger and Butterknife useful. But having a couple of modules without them could be a good idea, that would make their builds so much faster!

One More Trick — Rump Up API Level
Compilation is not the only thing that slows down build process. Producing .DEX files could also be time consuming. Especially if an application is above DEX Limit. Using multidex configuration increases build time, build system need to decided which classes go to which .DEX file. With introduction of Android Runtime the way how Android OS works with application that has multiple DEX files has changed. This is what Android Studio documentation says about it:https://developer.android.com/studio/build/multidex.html#mdex-on-l
Android 5.0 (API level 21) and higher uses a runtime called ART which natively supports loading multiple DEX files from APK files. ART performs pre-compilation at app install time which scans for classesN.dex files and compiles them into a single .oat file for execution by the Android device.”
That leads to decrease in build time. The reason is that each module produces its own DEX files, that are included into APK without modification. If you take a look into the tasks that run during build, you will notice that transformClassesWithMultidexlist is no longer executed. Also compilation itself became faster. You can find more information and instructions how to make a flavor that uses API 21 here. https://developer.android.com/studio/build/multidex.html#dev-build

Fastest Build Configuration Achieved.
Using API 21 for debug is an easy gain for every project. I tried it for 5+1 configuration and results were amazing:
![[Pasted image 20240412205854.png]]

Even for changes in library module incremental build time was only 17 seconds! But take into account that all modules are independent from one another. Once dependency between modules is introduced, build time increases from 17s to 42s (last row in the table above)!

Developing Library Module in a Test Driven Way
One of the main reasons why test-driven development (TDD) is difficult for single-module project is build time. TDD encourage to run test often. Running tests multiple times in a minute is a normal practice. But when build takes a minute or two, working in Test Driven way couldn’t be very productive.

With introduction of modules that problem is automagically resolved. Building a single module in the last configuration took only 9s! It make possible to run tests as often as needed.

**Conclusion**

First and most important, the hypothesis was correct, modularizing project can significantly speed up build process, but not for all configurations.

Second, if splitting is done in a wrong way, then build time will be drastically increased, because Gradle build both, release and debug version of library modules.

Third, working in test-driven way is much easier for a project with multiple modules, because building a small library module is way faster then the whole project.

Forth, doing many things in parallel slows down the build. So having more powerful hardware is a good idea.

Below you can find results of all experiments described in this article:

![[Pasted image 20240412205934.png]]







[How modularization can speed up your Android app’s built time](https://www.freecodecamp.org/news/how-modularisation-affects-build-time-of-an-android-application-43a984ce9968/)
