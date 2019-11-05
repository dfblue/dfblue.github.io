---
layout: post
title: Tips & Tricks for Managing Deepfake Source Videos
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

Preparing videos for frame and face extraction properly can save us lots of time and improve our deepfakes.

Of course, we can use a video editor like Final Cut or Premiere Pro, but these tools will **re-encode** the video. This means we will loose quality. It might not be perceptible, but do it enough times and we can affect the quality of our deepfake.

Instead of video editors, we will use low level tools which require some time to understand. Once we gain a strong grasp with them however, we can accomplish simple editing tasks quickly.

## Cutting out unwanted parts

We at DFBlue use the excellent LosslessCut to quickly trim multiple unwanted parts in a video clip. This tool is extremely useful for cutting interviews, movies, and tv shows where your target face isn't always in the shot.

### Download LosslessCut

[https://github.com/mifi/lossless-cut/releases](https://github.com/mifi/lossless-cut "https://github.com/mifi/lossless-cut")

Direct links:

* [Windows](https://github.com/mifi/lossless-cut/releases/download/v2.6.0/LosslessCut-2.6.0.exe)
* [MacOS](https://github.com/mifi/lossless-cut/releases/download/v2.6.0/LosslessCut-2.6.0.dmg)
* [Linux](https://github.com/mifi/lossless-cut/releases/download/v2.6.0/lossless-cut-2.6.0.tar.bz2)

We can use LosslessCut by following the [workflow](https://github.com/mifi/lossless-cut#typical-workflow) in the Readme.

## Merging multiple videos together

If all the videos have the same codec and size, LosslessCut can be used to merge them together. This feature is under `Tools > Merge`.

If our videos aren't all the same size, we can upscale or downscale them.

## Upscaling or Downscaling videos

In order to merge multiple videos together they need to be the same size. Generally we want to scale all our videos to 1080p (1920x1080).

We can use `ffmpeg` to accomplish this. `ffmpeg` is a command line tool with tons of tutorials available online so we will skip the setup and go directly to the usage.

To scale a video to 1080p use the following command

    ffmpeg -i input.mp4 -vf scale=-1:1080 input_1080.mp4