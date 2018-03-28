---
layout: post
title: "Juce audio + Android native UI"
date: "2018-03-20 16:46:17 +0100"
repo: "https://github.com/jbloit/JuceAudio_androidNativeUI"
---

Try and make the simplest app that mixes native Android UI with a Juce audio engine, as in this talk: [ADC'2015 video](https://www.youtube.com/watch?v=R2WhYU2JTfQ).

# Dev log
## Noise-making Juce audio app

- Generate project from the AudioApplication template in Projucer.
- Fill method bodies from code snippets in the [simple noise synth tutorial](https://docs.juce.com/master/tutorial_simple_synth_noise.html)
- Make sure to select the right *minimum SDK version* in the Projucer android exporters menu (has to correspond to the SDKs installed in Android Studio, see [Android studio setup](https://docs.juce.com/master/tutorial_android_studio.html)).

[>> tested commit](https://github.com/jbloit/JuceAudio_androidNativeUI/tree/feaff02caa6dc3b75893e982ecabc4b80e1390c5)

This runs on Android, but Juce (v5.3) takes control of the main activity. Looking at the generated java code, it looks structurally different to what is presented in Adamski's talk. I need to figure out C++ - java communication with JNI before I can make sense of this. I'm also starting to think that since I don't plan to use Juce for GUI in my current project, I should focus on embedding Juce as a C++ lib within the java main app... [well it looks like we'll need to wait for that](https://forum.juce.com/t/jnienv-not-initialized-when-building-an-android-library-aar-out-of-a-juce-project/22209/14) [yep](https://forum.juce.com/t/build-juce-shared-library-for-android-stuck/17005/6)


## Getting started with JNI & NDK

First, gather a few resources:
### JNI tutorials and resources
1. [J.Kemp 2012 JNI tutorial, OSX](http://mrjoelkemp.com/2012/01/getting-started-with-jni-and-c-on-osx-lion/)
2. [NTU (singapore univ) JNI Tutorial](https://www3.ntu.edu.sg/home/ehchua/programming/java/JavaNativeInterface.html)
3. [Getting Started with the NDK](https://developer.android.com/ndk/guides/index.html)
4. [NDK Samples](https://github.com/googlesamples/android-ndk/tree/master)

### General notes
- **NDK vs JNI**: compile C and C++ with NDK, use JNI for interfacing.
- **CMAKE** is used to compile native libs

### Hands on
#### NDK Samples / Hello-jni
[source](https://github.com/googlesamples/android-ndk/tree/master/hello-jni)

#### Codelab: making waves part 1
[source](https://codelabs.developers.google.com/codelabs/making-waves-1-synth/index.html?index=..%2F..%2Findex#0)

#### Codelab: making waves part 2
[source](https://codelabs.developers.google.com/codelabs/making-waves-2-sampler/#0)

## Juce cheat sheet

### Lifecycle
- audio system startup:
> the call to the AudioAppComponent::setAudioChannels() function triggers the audio system to start up. In particular, it will call the prepareToPlay() function.

- shut down audio:
> Calling the AudioAppComponent::shutdownAudio() function will, in turn, cause the AudioAppComponent::releaseResources() function to be called. This is a good place to dispose of resources, if we allocated any during the running of our audio process (for example, if we allocated memory or opened some files).


### resources
- juce tutorial: [simple noise synth](https://docs.juce.com/master/tutorial_simple_synth_noise.html)
- juce forum: [File browser on android](https://forum.juce.com/t/file-browser-on-android/26161)
- juce tutorial: [Looping audio using the AudioSampleBuffer class](https://docs.juce.com/master/tutorial_looping_audio_sample_buffer.html)
- juce tutorial: [Android studio setup](https://docs.juce.com/master/tutorial_android_studio.html)
