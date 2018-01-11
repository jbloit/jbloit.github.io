---
layout: post
title: "FFmpeg recipes"
date: "2018-01-11 14:46:19 +0100"
---

Useful recipes for when using FFmpeg. Just for saving some googling time.

### Shrink down all mov videos in a folder
Make smaller in size (width = 320) and apply compression.
```bash
for fff in `ls *.mov`; do ffmpeg -i $fff -vf scale=320:-1 -vcodec libx264 -crf 30 lores/$fff; done
```

## Links
[scaling](https://trac.ffmpeg.org/wiki/Scaling)
