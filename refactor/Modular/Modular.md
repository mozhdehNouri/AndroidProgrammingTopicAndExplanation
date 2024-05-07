
###### Modules
A *module* is a collection of source files and build settings that let you divide your project into discrete units of functionality. Your project can have one or many modules, and one module can use another module as a dependency. You can independently build, test, and debug each module.
Modularization is a means of structuring your codebase in a way that improves maintainability and helps avoid these problems.  Each module is independent and serves a clear purpose. By dividing a problem into smaller and easier to solve subproblems, you reduce the complexity of designing and maintaining a large system.
##### Modularization strategy:

- **Low coupling** - Modules should be as independent as possible from one another, so that changes to one module have zero or minimal impact on other modules. They should not possess knowledge of the inner workings of other modules.

- **High cohesion** - A module should comprise a collection of code that acts as a system. It should have clearly defined responsibilities and stay within boundaries of certain domain knowledge. For example, the `core:network` module in Now in Android is responsible for making network requests, handling responses from a remote data source, and supplying data to other modules.

#### Modularization by Layer :

Modularization by layer involves breaking down an application into separate modules based on the different layers of the application architecture. For example, an Android application typically has a presentation layer (UI), a domain layer (business logic), and a data layer (data access). Each of these layers can be modularized into separate modules that can be developed and tested independently.

- Fits for large projects/teams
- Highly modularized
- Features can be used across multiple apps easily
- Communication between feature modules may be struggling
- More strict rules

With this approach you will have too many modules. The **app** module in the second approach is divided into feature modules. And some shared codes between features are also moved to a separate module

#### Modularization by Feature:

Modularization by feature involves breaking down an application into separate modules based on the different features or use cases of the application. For example, an e-commerce application might have separate modules for product search, product details, shopping cart, and checkout.

Modularization by feature can help improve code organization and make it easier to add new features and functionality to an application. However, it can also lead to duplication of code and dependencies between different feature modules.

#### Hybrid Approaches :

Hybrid approaches combine modularization by layer and modularization by feature. For example, an application might have separate modules for the presentation layer, domain layer, and data layer, but also have separate modules for each feature or use case of the application.

Hybrid approaches can help balance the benefits of modularization by layer and modularization by feature, but can also lead to increased complexity and dependencies between modules.

##### Layer vs. Feature Separation

One of the most common ways of Modularization in the old days was to separate the presentation and data layer (aka domain-data-presentation). It took the developers a while to find out that the domain-data-presentation layer was no longer enough to apply Modularization. Separating only these layers will target the small projects, and you wouldn’t benefit from the reasons mentioned above. Because when you develop a Feature, you will touch all the layers, and guess what? You will turn a lot of heads, and all of your layers will recompile every time.

That’s why I want to demonstrate how the incremental build works in the next section. You may be surprised that this layering in your project (without Feature separation) wouldn’t help you have isolated Features.

By incremental build, the build process can run independently in a shorter time than when you only have three modules for your data, domain, and presentation of the entire app. Instead, by Feature separation, you will have more isolated modules, and then you can apply these layers on each Feature module if you are interested.

##### Mistake 1:

The first mistake you can do  it is you modular your project at first.   There is no need to make your project modular if it is not necessary. the most of time your project no need to be modular because your project is small and when you modular it you just add complexity in your projet.
##### Mistake 2 :
Never use of layer-base approach to module your project. Let's Comparison the modular approch benifit will pass in layer-base or not :

The first benefit is build time :
UI module depends on the domain module and the domain module depend on data module 
if any change happens data module domain and ui module rebuild 

The second reusability :
you can't separate the data module and put it in another project.

Third benefit delegation :
broke your code you cant use for example team A worked on Module A and Team B work on Module B 

The best practice is to use a hybrid approach for modularize :
First separate module by feature and in the module you have different modules and separate by layer.


Manage Dependecies in modular approch:

Comparison between buildSrc and build-logic :

1. Scope: The "buildSrc" folder is intended to be used for defining custom Gradle plugins, while the "build-logic" approach is a more general approach to organizing Gradle projects. Convention plugins defined in "build-logic" can be used to configure all aspects of a project, not just custom plugins.

2. Dependencies: Plugins defined in "buildSrc" are automatically available to all subprojects in a multi-project build, but they are not available to the root project or any other builds outside the current project. Convention plugins defined in "build-logic", on the other hand, are available to any project that includes the "build-logic" project as an included build.

3. Build time: Because the "buildSrc" folder is compiled separately from the main project, it can add significant time to the build process. Convention plugins defined in "build-logic" are included as part of the build process and can be compiled along with the rest of the project, potentially reducing build time.

4. Flexibility: Convention plugins defined in "build-logic" can be designed to be composable and additive, allowing for greater flexibility in configuring different aspects of a project. By contrast, plugins defined in "buildSrc" are typically more focused on specific tasks and may be less flexible in their application.

5. Code duplication: Convention plugins defined in "build-logic" can help to reduce code duplication by providing a single source of truth for common configurations. With "buildSrc", it's possible for multiple subprojects to define similar logic in their own build scripts, leading to duplicated code and potential inconsistencies.

##### What is Convention plugin :

Convention plugins in Android Gradle are a type of plugin that provides a set of conventions and default behaviors for building Android projects. Convention plugins are designed to simplify the build process by providing a predefined structure and set of configurations that developers can use as a starting point for their projects.
Convention plugins are typically included in the Gradle build file using the `apply plugin` syntax, like any other plugin. Once applied, the plugin adds a set of default configurations to the project that follow the conventions set by the plugin. For example, the `com.android.application` plugin is a convention plugin that provides a default configuration for building Android applications.

Some examples of convention plugins for Android Gradle include:
- `com.android.library`: This convention plugin provides a default configuration for building Android libraries.
- `kotlin-android`: This convention plugin provides a default configuration for building Android projects with Kotlin.
- `android-apt`: This convention plugin provides support for using the Android Annotation Processing Tool (APT).

Convention plugins can be very useful for simplifying the build process and reducing the amount of boilerplate code required to set up a new Android project. However, they may not always be suitable for complex projects that require more customization and flexibility in the build process.

What benefits convention plugin provide for our project :

1. Simplifying the build process: Convention plugins provide a set of conventions and default behaviors that simplify the build process for Android projects. By following the conventions set by the plugin, developers can avoid the need to write custom configurations for their projects.

2. Reducing boilerplate code: Convention plugins can help reduce the amount of boilerplate code required to set up a new Android project. This makes it easier and faster to get started with a new project.

3. Encouraging best practices: Convention plugins are designed to encourage best practices for building Android projects. By using a convention plugin, developers can be confident that their project follows established best practices and is set up in a consistent manner.

4. Supporting common tasks: Convention plugins often provide support for common tasks in Android projects, such as building libraries, working with Kotlin, or using annotation processing tools. This can save developers time and effort by providing built-in support for these tasks.

### Using Version catalog :

As projects start to be more complex and modularized, keeping all the dependencies in sync and updating them becomes a hard and annoying task. To solve this, we have a few alternatives and a new one was recently launched by Gradle to make our lives easier.


**How to have a modular project with a good build speed?**

For teams with big projects, waiting for new builds could eventually take up 10-15% of their workday. This not only wastes precious developer time — it also makes test-driven development extremely tedious, which hurts overall code quality.
Build a project has these five steps :

1. **Preparation of dependencies.** During this phase Gradle check that all libraries this module depends on are ready. If this module depends on another one, that module would be built as well.
2. **Merging resources and processing Manifest.** After this phase resources and Manifest are ready to be packaged in the result file.
3. **Compiling.** This phase started with Annotation Processors, in case you use them. Then source code is compiled into byte code. If you are using AspectJ, weaving also happens here.
4. **Postprocessing.** All Gradle tasks with a “transform” prefix are part of this phase. Most important ones are: `transformClassesWithMultidexlist` and `transformClassesWithDex`*.* They produce .DEX files.
5. **Packaging and publishing.** For libraries this stage means creating an .AAR file in the end, for applications — .APK.

It also doesn’t rebuild a module if it wasn’t changed. That leads to the following hypothesis:  “The Incremental build time for a project with multiple modules is faster than for a single module project, because only modified modules are recompiled.”

note : code more coupled, and increased compilation time So writing code is very important.
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

```gradle
dependencies {    compile project(path: ':app2')    compile project(path: ':app3')}
```
There’s an important problem in the code above: if a library is added like this, then the application always depends on the release variant, independent of it’s own build type. It also doesn’t matter which variant is chosen in Android Studio. Here’s what the Gradle Plugin User Guide says about all this:
By default a library only publishes its release variant. This variant will be used by all projects referencing the library, no matter which variant they build themselves. This is a temporary limitation due to Gradle limitations that we are working towards removing.

Luckily, it’s possible to change the variant our application depends on.
First, add the following code to libraries’ build.gradle file. It will allow library to publish debug variant as well:
```gradle
android {   
defaultConfig {      
defaultPublishConfig 'release'      
publishNonDefault true    }}
```
```gradle
dependencies {  
debugCompile project(path: ':app2', configuration: "debug") 
releaseCompile project(path: ':app2', configuration: "release")   
debugCompile project(path: ':app3', configuration: "debug")   
releaseCompile project(path: ':app3', configuration: "release")}
```

Now the debug variant of our application depends on the debug variant of the libraries, and its release depends on their release. So lets make some changes to the module app2 and rebuild it. After these changes, we can check our profile report again:

![[Pasted image 20240412205506.png]]

![[Pasted image 20240412205522.png]]

The most significant difference is the absence of :app2:packageReleaseJarArtifact, which saves us about 15 seconds. In addition, the time is reshuffled a bit between rest of the tasks, and we end up with 1m 32s. That is 18 seconds faster then before, but still 22 seconds slower then our initial configuration. For changes only in the application module, build time stays almost the same — 36 seconds vs 35 seconds in our previous configuration.

5 Modules
Obviously, build time depends on the amount of code. If you reduce amount of code by half then the build should also be roughly two times faster. If instead of 3 modules project is split in 5, then build time should be approximately 40% faster.

![[Pasted image 20240412205614.png]]
If changes are made only in application module then incremental build time is approximately 24s. For changes in a library module incremental build takes 50s. Compare to initial 1m 10s that is already  a win. But I have a few more tricks at my disposal.

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

