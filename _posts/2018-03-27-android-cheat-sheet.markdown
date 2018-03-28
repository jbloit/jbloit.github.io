---
layout: post
title: "Android cheat sheet"
date: "2018-03-27 11:11:44 +0200"
---

Random notes while developing my first Android app

### Gradle
the build system running under the hood. the app can actually be built from command line using gradle. [Better learn this stuff](https://guides.gradle.org/creating-new-gradle-builds/?_ga=2.163855231.199583704.1522232861-535293929.1522061178), since gradle errors come up all the time.

When syncing a project with Gradle, Android studio updates the IDE's project structure based on the Gradle files.

[Building Android Apps tutorial](https://guides.gradle.org/building-android-apps/)


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
