---
layout: post
title: "android audio"
date: "2018-03-20 10:47:45 +0100"
---

Random notes on android audio development


### [Best Practices for Android Audio (Google I/O '17)](https://www.youtube.com/watch?v=C0BPXZIvG-Q)

- ```audio.pro``` hardware flag: means that path has latency < 20ms. Can be used to limit distribution to these. See [low latency flag (< 45 ms)](https://developer.android.com/reference/android/content/pm/PackageManager.html#FEATURE_AUDIO_LOW_LATENCY) and [pro < 20ms](https://developer.android.com/reference/android/content/pm/PackageManager.html#FEATURE_AUDIO_PRO)

```java
boolean hasLowLatencyFeature =
    getPackageManager().hasSystemFeature(PackageManager.FEATURE_AUDIO_LOW_LATENCY);

boolean hasProFeature =
    getPackageManager().hasSystemFeature(PackageManager.FEATURE_AUDIO_PRO);
```

- don't dos in audio callback:
  - logging (instead, use atrace and systrace)
  - memory allocation
  - don't wait on other threads
  - no file io

- core migration can incur time penalty
  -> [reserve core](https://youtu.be/C0BPXZIvG-Q?t=1137)

- stabilize load.

- AAudio: native C API
  - create stream, with stream builder
  - bursts (define frames per bursts)
  - [latency tuning](https://youtu.be/C0BPXZIvG-Q?t=1779)
  - oboe c++ wrapper: to decide dynamically whether to use AAudio or OpenSL ES, depending on platform.

### [High Performance audio](https://developer.android.com/ndk/guides/audio/index.html)

- [sample rate](https://developer.android.com/ndk/guides/audio/audio-latency.html)
> Don't assume that the actual sample rate exactly matches the nominal sample rate. For example, if the nominal sample rate is 48,000 Hz, it is normal for the audio clock to advance at a slightly different rate than the operating system CLOCK_MONOTONIC. This is because the audio and system clocks may derive from different crystals
