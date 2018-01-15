---
layout: post
title: "iOS sync several videos to audio clock"
date: "2018-01-09 09:52:49 +0100"
repo: "https://github.com/jbloit/avplayer_mutipleVideos"
tags:
  - "iOS"
  - "video"
---


This is a toy app to try and understand how many videos I can play simultaneously on iOS. Some forums say it's 4. I seem to hit a limit at 16.

## Behaviors to try out:
- Can I quickly toggle between several videos within a same AVPlayerLayer?
- Can I sync video playback on an external clock (driven by the audio render callback, for instance)?

## Links
- [wwdc 517: Real-Time Media Effects and Processing during Playback](https://developer.apple.com/videos/play/wwdc2012/517/)
- [This guy also observes 16 vids as a max](https://stackoverflow.com/questions/40474480/how-many-avplayers-are-allowed-to-be-created-at-the-same-time)
- [The four streams limit seems to be unofficially admitted by Apple](https://stackoverflow.com/questions/8608570/avplayeritem-fails-with-avstatusfailed-and-error-code-cannot-decode/9933853#9933853)
- [Some dev hints from the Pinterest engineers](https://medium.com/@Pinterest_Engineering/building-native-video-pins-7ff89ad3ec33)
- [How about using OpenGL or Metal?](https://stackoverflow.com/questions/38130497/is-metal-a-good-framework-to-use-for-multiple-video-rendering)

## Dev log
### Sync videos to audio clock
[works](https://github.com/jbloit/avplayer_mutipleVideos/tree/80d1573fe9fc7a3a49cc05a3403e25c0297aed33)
Video playback start time and rate are set given the audio clock. The main things to grasp here, are that
- A player's Timebase can has a rate and sync time relative to a host clock
- A host clock can be either the host time or given from an audio clock. Both are instances of a CMClock
- The important method is setrate(rate, time, athosttime), which essentially defines a player's timebase relative to the host clock

See the beginning of this video for a good explanation of CMTime :
https://developer.apple.com/videos/play/wwdc2012/517/

### Toggling layers for a same avplayer
I'm trying to understand which is the exact upper limit: is it the number of AVPlayer instances I can keep alive?
[This SO post](https://stackoverflow.com/questions/8608570/avplayeritem-fails-with-avstatusfailed-and-error-code-cannot-decode/9933853#9933853) says what counts is the actual number of video rendering pipelines:

> It is not a limit on the number of instances of AVPlayer, or AVPlayerItem. Rather,it is the association of AVPlayerItem with an AVPlayer which creates a "render pipeline", and you are limited to 4 of these.

(Apparently though, 4 is not a hard limit, and is rather dependent on which hardware you are running)
