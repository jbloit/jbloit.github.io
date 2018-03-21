---
layout: post
title: "Juce audio + Android native UI"
date: "2018-03-20 16:46:17 +0100"
---

Try and make the simplest app that mixes native Android UI with a Juce audio engine, as in this talk: [ADC'2015 video](https://www.youtube.com/watch?v=R2WhYU2JTfQ).

# Dev log
## Noise-making Juce audio app

- Generate project from the AudioApplication template in Projucer.
- Fill method bodies from code snippets in the [simple noise synth tutorial](https://docs.juce.com/master/tutorial_simple_synth_noise.html)
- Make sure to select the right *minimum SDK version* in the Projucer android exporters menu (has to correspond to the SDKs installed in Android Studio, see [Android studio setup](https://docs.juce.com/master/tutorial_android_studio.html)).

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
