---
layout: post
title:  "The DeepFaceLab Tutorial (always up-to-date)"
author: "DFBlue"
image: https://imgur.com/GKfH1jT.jpg
date:   2019-10-25
redirect_from: /pub/2019-07-27-deepfacelab-tutorial/
categories: [tutorial, guide, how to, deepfacelab, deepfakes]
highlight: true
excerpt: "We will use DeepFaceLab to create the deepfakes. Another software, FaceSwap is also available, and will have a separate tutorial."
---

> 👀 Update 10/25/19: Added instructions for the new [SAEHD model](#training) (amazing)

> 👽 Update 9/24/19: New post for [AVATAR mode]({% post_url 2019-09-24-deepfacelab-avatar-tutorial %}){:target="_blank"}

-----

## Source and destination videos requirements

* High resolution (4k webm is best, lower than 1080p is not recommended)
* Faces not too far from camera and unobstructed
* Multiple angles, facial expressions
* Brightly and evenly lit
* Faces should somewhat match (beard, hat, hair, skin color, shape, glasses)
* Need at least 2 mins of good quality video, interview videos work well

## Downloading the software
> We will use DeepFaceLab to create the deepfakes. Another software, FaceSwap is also available, and will have a separate tutorial.

* [Download DeepFaceLab](https://github.com/iperov/DeepFaceLab/blob/master/doc/doc_prebuilt_windows_app.md){:target="_blank"}
    * Make sure to pick the right build for your GPU. If you don't have a GPU, use the CLSSE build
    * Here's the [direct link](https://drive.google.com/drive/folders/17a9b9zmLdnAlItifcGSE9ixDIDAT3YxP){:target="_blank"}
    * In that folder, you will find some pre-compiled face-sets. Go ahead and download one of them to get started quickly (otherwise you will have to build your own face-set from videos / images)
* The downloaded .exe will extract and install the program to the location of your choosing. 
    * A `workspace` folder will be created. This is the folder where all the action will happen.

## Extracting faces from source video

* Name the source video `data_src` and place it in the `\workspace` folder.
    * Most formats that `ffmpeg` supports will work
* Run `2) extract images from video data_src`
    * Use PNG (better quality)
    * FPS <= 10 that gets you *at least* 2000 images (4k-6k is ideal)
* Run `4) data_src extract faces S3FD best GPU`
    * Extracted faces saved to `data_src\aligned`.
* Run `4.2.2) data_src sort by similar histogram` 
    * Groups similar detected faces together 
* Run `4.1) data_src check result` 
    * Delete faces that are not the right person, super blurry, cut off, upside down or sideways, or obstructed
* Run `4.2.other) data_src util add landmarks debug images`
    * New images with `_debug` suffix are created in  `data_src/aligned` which allow you to see the detected facial *landmarks*
    * Look for faces where landmarks are misaligned and delete the `_debug` and original images for those
    * Once you’re done, delete all  `_debug` images by using the search bar to filter for `_debug`
* Run `4.2.6) data_src sort by final`
    * Choose a target image number around 90% of your total faces

## Extracting faces from destination video

You may choose to either extract from (1) the final video clip you want, or (2) one that is cut to include only the face you want to swap. If you choose 1, you may have to spend more time cleaning the extracted faces. If you choose 2 you will have to edit back the final video (and audio) after the swap.

* Name your final video `data_dst` and put it in the `\workspace` folder
* Run `3.2) extract PNG from video data_dst FULL FPS`
* Run `5) data_dst extract faces S3FD best GPU` 
* Run `5.2) data_dst sort by similar histogram`
* Run `5.1) data_dst check results` 
    * Delete all faces that are not the target face to swap, or are the target face but upside down or sideways. Every face that you leave in will be swapped in the final video.
* Run `5.1) data_dst check results debug`
    * Delete any faces that are not correctly aligned or missing alignment
    * Pay special attention to the jaw area
* Run `5) data_dst extract faces MANUAL RE-EXTRACT DELETED RESULTS DEBUG`
* Manual extraction for misaligned faces
    * For each face, move your cursor around until it aligns correctly onto the face
    * If it’s not aligning, use the mouse scroll wheel / zoom to change the size of the boxes
    * When alignment is correct, hit <kbd>enter</kbd>
    * Go back and forth with <kbd>,</kbd> and <kbd>.</kbd>. If you don’t want to align a frame just skip it with <kbd>.</kbd>
    * Mouse left click will lock/unlock landmarks. You can either lock it by clicking or hitting <kbd>enter</kbd>.

## Training

Run `6) train SAEHD` 

<table class="table table-striped table-sm">
  <tr>
    <th>Setting</th>
    <th>Value</th>
    <th>Notes</th>
  </tr>
    <tr>
    <td>iterations</td>
    <td>100000</td>
    <td>Or until previews are sharp with eyes and teeth details.</td>
  </tr>
  <tr>
    <td>resolution</td>
    <td>128</td>
    <td>Increasing resolution requires significant VRAM increase</td>
  </tr>
  <tr>
    <td>face_type</td>
    <td>f</td>
    <td></td>
  </tr>
  <tr>
    <td>learn_mask</td>
    <td>y</td>
    <td></td>
  </tr>
  <tr>
    <td>optimizer_mode</td>
    <td>2 or 3</td>
    <td>Modes 2/3 place work on the gpu and system memory. For a 8gb card you can place on mode 3 and still most likely be able to do 160 res fakes with small batch size.</td>
  </tr>
  <tr>
    <td>architecture</td>
    <td>df</td>
    <td></td>
  </tr>
  <tr>
    <td>ae_dims</td>
    <td>512</td>
    <td>Reduce if less GPU memory (256)</td>
  </tr>
  <tr>
    <td>ed_ch_dims</td>
    <td>21</td>
    <td>Reduce if less GPU memory</td>
  </tr>
  <tr>
    <td>random_warp</td>
    <td>y</td>
    <td></td>
  </tr>
  <tr>
    <td>trueface</td>
    <td>n</td>
    <td></td>
  </tr>
  <tr>
    <td>face_style_power</td>
    <td>0</td>
    <td>Can enable if you want to morph src more to dst. But disable after 15k iterations.</td>
  </tr>
  <tr>
    <td>bg_style_power</td>
    <td>10</td>
    <td>Turn off at 15k iterations. Styles on consume ~30% more vram so you will need to change batch size accordingly.</td>
  </tr>
  <tr>
    <td>color_transfer</td>
    <td>varies</td>
    <td>Try all modes in the interactive converter</td>
  </tr>
  <tr>
    <td>clipgrad</td>
    <td>n</td>
    <td></td>
  </tr>
  <tr>
    <td>batch_size</td>
    <td>8</td>
    <td>Higher if you don't run out of memory</td>
  </tr>
  <tr>
    <td>sort_by_yaw</td>
    <td>n</td>
    <td>No, unless you have very few src faces</td>
  </tr>
  <tr>
    <td>random_flip</td>
    <td>y</td>
    <td></td>
  </tr>
  <caption>For an NVIDIA GTX 1080 8gb GPU</caption>
</table>

## Optional: History timelapse

Before converting, you can make a timelapse of the preview history (if you saved it during training). Do this only if you understand what `ffmpeg` is.

```

> cd \workspace\model\SAEHD_history

> ffmpeg -r 120 -f image2 -s 1280x720 -i %05d0.jpg -vcodec libx264 -crf 25 -pix_fmt yuv420p history.mp4

```

## Convert

* Run `7) convert SAEHD`

Use the interactive converter and memorize the shortcut keys, it will speed up the process a lot.

<table class="table table-striped table-sm">
  <tr>
    <td>Setting</td>
    <td>Value</td>
    <td>Notes</td>
  </tr>
  <tr>
    <td>interactive_converter</td>
    <td>y</td>
    <td>Definitely use the interactive converter since you can try out all the different settings before converting all the frames</td>
  </tr>
  <tr>
    <td>mode</td>
    <td>overlay</td>
    <td></td>
  </tr>
  <tr>
    <td>mask_mode</td>
    <td>learned</td>
    <td></td>
  </tr>
  <tr>
    <td>erode_modifier</td>
    <td>0-50</td>
    <td>If src face is bleeding outside the edge of dst face increase this to "erode" away the src face on the outside</td>
  </tr>
  <tr>
    <td>blur_modifier</td>
    <td>10-200</td>
    <td>The more similar the face the lower you can set erode and blur and get great results.</td>
  </tr>
  <tr>
    <td>motion_blur</td>
    <td>0</td>
    <td></td>
  </tr>
  <tr>
    <td>color_transfer</td>
    <td>ebs</td>
    <td>Try all of them, can even use different ones for different scenes / lighting</td>
  </tr>
  <tr>
    <td>sharpen_mode</td>
    <td>box</td>
    <td></td>
  </tr>
  <tr>
    <td>sharpen_amount</td>
    <td>1-3</td>
    <td></td>
  </tr>
  <tr>
    <td>super_resolution</td>
    <td>RankSRGAN</td>
    <td>Enhances detail, especially around the eyes</td>
  </tr>
  <tr>
    <td>color_degrade_power</td>
    <td>n</td>
    <td></td>
  </tr>
  <tr>
    <td>export_alpha_mask</td>
    <td>n</td>
    <td>Outputs transparent PNGs for use in post-production tools if you need it</td>
  </tr>
</table>

* While conversion is running, you can preview the final images `data_dst\merged` folder to make sure it’s correct. If it’s not, just close the convert window, delete `/merged` and start conversion again.
* Run `8) converted to mp4`
    * Bitrate of 3-8 is sufficient for most

## Done 🤡

> Contributions by DFBlue, PlanetOfTheFakes
