##### kapt.use.worker.api=false  in Gradle.properties

"kapt.use.worker.api=false" is a setting in the Android Gradle build system that disables a feature called the Gradle Worker API for a tool called KAPT.
KAPT is used for processing annotations in Kotlin projects. By default, KAPT uses a feature called the Gradle Worker API that helps process annotations faster by doing some work in parallel (at the same time).
"kapt.use.worker.api=false" is a setting in the Android Gradle build system that disables a feature called the Gradle Worker API for a tool called KAPT.
KAPT is used for processing annotations in Kotlin projects. By default, KAPT uses a feature called the Gradle Worker API that helps process annotations faster by doing some work in parallel (at the same time).
But keep in mind that disabling the parallel processing feature may slow down the build process, especially for larger projects. So, it's usually best to leave it enabled unless you're experiencing specific problems.