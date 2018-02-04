---
layout: post
title: Fixing Memory Issues - resources
date: "2018-02-04 17:26:40 +0100"
---

A couple good resources I watched on this topic.

# [WWDC_2013 Fixing Memory Issues](https://developer.apple.com/videos/play/wwdc2013/410/)

Clarifies a few ideas on the heap.
Quite some detailed info on virtual memory (resident, dirty, shared, private...)

Gauge provides the footprint number, but that's only a big picture.
Profiling with instruments allows to refine it.


# [Memory 2 - Finding and Fixing Memory Leaks iOS, Xcode 9, Swift 4](https://www.youtube.com/watch?v=1LnipXiSrSM)

Nice one, shows how to use the *Debug Memory Graph*. Nice pace, with code examples all the way. Retain cycles. Distinction between strong, weak and unowned references.
