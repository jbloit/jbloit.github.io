---
layout: post
title: "Android cheat sheet"
date: "2018-03-27 11:11:44 +0200"
---

Random notes while developing my first Android app

### Gradle
the build system running under the hood. the app can actually be built from command line using gradle. [Better learn this stuff](https://guides.gradle.org/creating-new-gradle-builds/?_ga=2.163855231.199583704.1522232861-535293929.1522061178), since gradle errors come up all the time.

When syncing a project with Gradle, Android studio updates the IDE's project structure based on the Gradle files.

#### Notes from [Building Android Apps tutorial](https://guides.gradle.org/building-android-apps/)

Library modules:
> The file settings.gradle is used by Gradle to configure the multi-project build. It should consist of a single line:
```
include ':app'
```
This tells Gradle that the app sub-directory is also a Gradle project. If, at some later time, you were to add an Android Library to this project through the available wizard, another project sub-directory would be created and added to this file.
**--------------**

Debug build types
> Below this section is a block called buildTypes. By default, Android apps support two build types, debug and release. This section allows you to configure each however you like. The debug section is not shown here, implying that all the default settings for debug are being used.
**--------------**


### NDK: Static vs. shared Library
- static: .a, precompiled, bundled in app, NDK strips unused parts.
- shared (or dynamic): .so, compiled at runtime. can include static libs.

NDK objects can only be loaded dynamically via JNI, loaded at run time. So there is at least one shared lib loaded at runtime, that can load more libs, either static or dynamic(or shared).

[source](https://stackoverflow.com/questions/3213789/difference-between-static-and-shared-libraries-in-androids-ndk)

[Hello-libs: google NDK tutorial on libs](https://github.com/googlesamples/android-ndk/tree/master/hello-libs)




### CMake or Android.mk
- short answer: use one or the other. CMake has a life outside of Android projects.
- longer answer: [see this thread](https://stackoverflow.com/questions/39589427/difference-between-cmake-and-ndk-build-in-android-studio-project)

### logging
- [Android doc: Logcat](https://developer.android.com/studio/debug/am-logcat.html)
- Dont use prints, use the Log class. Its ouput is printed to the Logacat window.
- prefer debug Log level during development:
>You should never compile verbose logs into your app, except during development. Debug logs are compiled in but stripped at runtime, while error, warning, and info logs are always kept.

### memory load
Trying to get a sense of how android behaves under memory load. I have a simple C++ class for leaking memory. Running through JNI.

```c++
#include "MemoryLeakTest.h"
#include <iostream>

using namespace std;

void MemoryLeakTest::aquireMemoryOneMegabyte() {
    char* oneMegaByte = new char[1024*1024];
    oneMegaByte[0] = 0; // so compiler doesn't complain variable is unused
}

void MemoryLeakTest::leakMemory() {
    for (int i=0; i<500000; i++) {
        aquireMemoryOneMegabyte();
    }
}
```

I'm running it on an app that just plays a sine wave. Profiling on samsung s6, in android studio, with debugger on.

- at i<500, audio behaves fine, profiler reports 14,89 Mb from native process
- at i<2000, audio behaves fine, profiler reports 38,89 Mb from native process, 100mb total.
- at i<5000, app takes about 5 secs to load. audio can be interrupted. profiler reports 85,19 Mb usage from native.
- at i<50000, app takes about 7 secs to load. audio is interrupted. profiler reports 0.77GB from native.
- at i<500000, app stays blank, profilers shows spikes, going up to about 1GB, then back to 0 etc...
