---
layout: post
title:  "DeepFaceLab Avatar tutorial"
author: "DeepFakeBlue"
image: https://i.imgur.com/O09rrlq.png
date:   2019-09-24
categories: [deepfake, deepfacelab, software, tutorial, example]
---

Iperov fixed and added back the Avatar model to DeepFaceLab on August 24th 2019. 

Avatar or puppet is a way to transform the facial expressions of the source person onto a destination person. 

This was great news for the deepfake community since we were missing easy to use tooling for it.

The source person is usually a voice actor and the destination person is usually a celebrity.

You’ve probably seen this method used in the [Jordan Peele / Obama video](https://www.youtube.com/watch?v=cQ54GDm1eL0){:target="_blank"}.

It’s the second category of face deepfaking, the first one being face swap. 

We have a separate [tutorial on the face swap models in DeepFaceLab](https://pub.dfblue.com/pub/2019-07-27-deepfacelab-tutorial){:target="_blank"}. 

**Read that first** if you don’t have familiarity with DeepFaceLab since it goes over setting up and configuring the software.

Great. But how do we use the Avatar model?

# Using the Avatar model on DeepFaceLab overview

- Source is the celebrity (10-20 mins)
- Destination is the actor (controls the celebrity face)
- Videos must be square aspect ratio (720x720) or (1080x1080)
- Extract src and dst frames at full fps
- Mark faces on src
- Extract unaligned faces on dst
- 2 stages of training (12-24hrs each)
    - 1st stage at batch size 48 for an 8GB GPU
    - 2nd stage at batch size 6 for an 8GB GPU
- Convert as usual

# Source and destination video requirements

The naming convention for source and destination is a bit confusing when it comes to the Avatar model, but just remember that the SOURCE is the CELEBRITY.

The Avatar model requires input videos in a square aspect ratio. This is different from the other models which have no such requirement. But don’t worry, it’s easy to crop our existing videos to square using `ffmpeg` or a video editing tool. See [ffmpeg.org](https://ffmpeg.org/){:target="_blank"} for more info.

### Cropping to square using `ffmpeg`

```
 ffmpeg -i input.mp4 -filter:v 'crop=ih/1:ih' -c:v libx264 -c:a copy output.mp4
```

## Extracting frames and faces

Once our source (data_src) and destination (data_dst) videos are in the workspace folder, we can start extraction. The extraction of frames and faces from the videos is also slightly different from the other face swap models.

Here are the steps we need to follow:

- `2) extract images from video data_src`
    - Extract at full fps
- `3.2) extract images from video data_dst FULL FPS`
- `4) data_src mark faces S3FD best GPU`
- `5) data_dst extract unaligned faces S3FD best GPU`

## Training the Avatar model

After extraction, we will train the Avatar model. Avatar has a 2 stage training process, but both stages are started by running `6) train AVATAR`.

### Stage 1

- Run `6) train AVATAR`
    - stage 1
    - batch size to max for our GPU (48 for 8GB Nvidia GTX 1080)
    - avatar type recommendation is `head`, but if the resulting video has a lot of movement, we can try again with `full_face`
    - run until we are happy with the clarity of columns 1 and 2 in the preview

Don’t worry that column 3 and 5 in the preview window stay grey, they will be trained during stage 2.

### Stage 2

- Run `6) train AVATAR`
    - stage 2
    - batch size to max for our GPU (6 for 8GB Nvidia GTX 1080)
    - run until we are happy with the clarity of columns 3 and 5 in the preview

## Converting the final video

Thankfully, converting is the same as the other models in DeepFaceLab.

- Run `7) convert AVATAR`
- Run `8) converted to mp4`

After running those two scripts we should see a `result.mp4` file in our workspace folder.

> 🎉 Congratulations, you just trained an Avatar/puppet model!

## Improving quality

The quality of Avatar model is not as good as the face swap models in DeepFaceLab, but we are working on putting together some recommendations for improving the quality. This will be released shortly.

-----

Follow us on [Twitter](https://twitter.com/dfblue){:target="_blank"} or [Reddit](https://reddit.com/u/deepfakeblue){:target="_blank"} to keep up with everything that is going on in the world of deepfakes. We aim to provide publications, tools, and services to further the ethical creation, detection, and awareness of deepfakes and digital forgery.

Think Blue.