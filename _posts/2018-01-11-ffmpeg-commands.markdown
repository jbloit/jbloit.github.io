---
layout: post
title: "FFmpeg recipes"
date: "2018-01-11 14:46:19 +0100"
---

Useful recipes for when using FFmpeg. Save some googling time.

### Thumb videos
Shrink down all mov videos in a folder. Rescale to given width (320), preserve ratio, and apply compression.
```bash
for fff in `ls *.mov`; do ffmpeg -i $fff -vf scale=320:-1 -vcodec libx264 -crf 30 lores/$fff; done
```

## Links
[scaling](https://trac.ffmpeg.org/wiki/Scaling)
