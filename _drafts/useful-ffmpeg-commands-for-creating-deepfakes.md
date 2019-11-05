---
layout: post
title: Useful FFMPEG commands for creating deepfakes
date: 2019-11-05 00:00:00 -0800
excerpt: ''
categories:
- ffmpeg
- video editing
- deepfake
author: DFBlue
image: ''
highlight: false

---
When sourcing video content for deepfakes, we end up with videos from a variety of sources. These videos can be in different formats or sizes.

Preparing videos for frame and face extraction properly can improve our deepfakes.

Of course, we can use a video editor like Final Cut or Premiere Pro, but these tools will **re-encode** the video. This means we will loose quality. It might not be perceptible, but do it enough times and we can affect the quality of our deepfake.

Instead of video editors, we will use low level tools which require some time to understand. Once we gain a strong grasp with them however, we can accomplish simple editing tasks quickly.

## Cutting out unwanted parts

We at DFBlue use the excellent LosslessCut to quickly trim multiple unwanted parts in a video clip. This tool is extremely useful for cutting interviews, movies, and tv shows where your target face isn't always in the shot.

We can download LosslessCut from GitHub.

[https://github.com/mifi/lossless-cut/releases](https://github.com/mifi/lossless-cut/releases "https://github.com/mifi/lossless-cut/releases")

Direct link for our convenience:

* [Windows](https://github.com/mifi/lossless-cut/releases/download/v2.6.0/LosslessCut-2.6.0.exe)
* [MacOS](https://github.com/mifi/lossless-cut/releases/download/v2.6.0/LosslessCut-2.6.0.dmg)
* [Linux](https://github.com/mifi/lossless-cut/releases/download/v2.6.0/lossless-cut-2.6.0.tar.bz2)

We can use LosslessCut by following the [workflow](https://github.com/mifi/lossless-cut#typical-workflow) in the Readme.