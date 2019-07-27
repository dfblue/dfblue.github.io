---
layout: post
title:  "The DeepFaceLab Tutorial (always up-to-date)"
author: "DeepFakeBlue Team"
header_image: https://i.imgur.com/hshcJnl.jpg
date:   2019-07-27
categories: [tutorial, guide, how to, deepfacelab, deepfakes]
---

> DeepFaceLab version 2019-06-20

## Source and destination videos requirements

* High resolution (4k webm is best, lower than 1080p is not recommended)
* Faces not too far from camera and unobstructed
* Multiple angles, facial expressions
* Brightly and evenly lit
* Faces should somewhat match (beard, hat, hair, skin color, shape, glasses)
* Need at least 2 mins of good quality video, interview videos work well

## Extracting Faces from source video

* Name the source video `data_src` and place it in the `\workspace` folder.
    * Most formats that `ffmpeg` supports will work
* Run `2) extract images from video data_src`
    * Use PNG (better quality)
    * FPS <= 10 that gets you at least 500 images (1000-2000 is best)
* Run `4) data_src extract faces S3FD best GPU`
    * Extracted faces saved to `data_src\aligned`.
* Run`4.2.2) data_src sort by similar histogram` 
    * Groups similar detected faces together 
* Run `4.1) data_src check result` 
    * Delete faces that are not the right person, super blurry, cut off, upside down or sideways, or obstructed
* If you have more than 2000 detected faces run `4.2.4) data_src sort by dissimilar histogram` 
    * Run `4.1) data_src check result` again
    * Check faces towards the end, if many of them are similar, you can delete similar ones until you have 2000 faces
* Run `4.2.other) data_src util add landmarks debug images`
    * New images with `_debug` suffix are created in  `data_src/aligned` which allow you to see the detected facial *landmarks*
    * Look for faces where landmarks are misaligned and delete the `_debug` and original images for those
    * Once you’re done, delete all  `_debug` images by using the search bar to filter for `_debug`

## Extracting Faces from destination video

You may choose to either extract from (1) the final video clip you want, or (2) one that is cut to include only the face you want to swap. If you choose 1, you may have to spend more time cleaning the extracted faces. If you choose 2 you will have to edit back the final video (and audio) after the swap.

* Name your final video `data_dst` and put it in the `\workspace` folder
* Run `3.2) extract PNG from video data_dst FULL FPS`
* Run `5) data_dst extract faces S3FD best GPU` 
* Run `5.2) data_dst sort by similar histogram`
* Run `5.1) data_dst check results` 
    * Delete all faces that are not the target face to swap, or are the target face but upside down or sideways. Every face that you leave in will be swapped in the final video.
    * Create a folder called `removed/` in the `/aligned` folder
    * Move target faces that are obstructed, blurry, or partial into `removed/`
    * We will move these faces back into `/aligned` once training is done
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

* Run `6) train SAE` 
* For an NVIDIA GTX 1080 8gb GPU these are the recommended settings for the first 40,000 iterations
<table>
  <tr>
    <td>Setting</td>
    <td>Value</td>
    <td>Notes</td>
  </tr>
  <tr>
    <td>iterations</td>
    <td>40000</td>
    <td></td>
  </tr>
  <tr>
    <td>batch_size</td>
    <td>8</td>
    <td></td>
  </tr>
  <tr>
    <td>sort_by_yaw</td>
    <td>y</td>
    <td></td>
  </tr>
  <tr>
    <td>random_flip</td>
    <td>n</td>
    <td>If src doesn't have all the face angles that dst has</td>
  </tr>
  <tr>
    <td>src_face_scale</td>
    <td>0</td>
    <td>Generally not used, but increase if dst face is much bigger than src</td>
  </tr>
  <tr>
    <td>resolution</td>
    <td>128</td>
    <td>64 if less GPU memory</td>
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
    <td>1</td>
    <td></td>
  </tr>
  <tr>
    <td>archi</td>
    <td>df</td>
    <td></td>
  </tr>
  <tr>
    <td>ae_dims</td>
    <td>512</td>
    <td>Reduce if less GPU memory (256)</td>
  </tr>
  <tr>
    <td>e_ch_dims</td>
    <td>42</td>
    <td>Reduce if less GPU memory</td>
  </tr>
  <tr>
    <td>d_ch_dims</td>
    <td>21</td>
    <td>Reduce if less GPU memory</td>
  </tr>
  <tr>
    <td>multiscale_decoder</td>
    <td>y</td>
    <td></td>
  </tr>
  <tr>
    <td>ca_weights</td>
    <td>n</td>
    <td></td>
  </tr>
  <tr>
    <td>pixel_loss</td>
    <td>n</td>
    <td></td>
  </tr>
  <tr>
    <td>face_style_power</td>
    <td>0</td>
    <td></td>
  </tr>
  <tr>
    <td>bg_style_power</td>
    <td>0</td>
    <td></td>
  </tr>
  <tr>
    <td>apply_random_ct</td>
    <td>y</td>
    <td></td>
  </tr>
  <tr>
    <td>clipgrad</td>
    <td>n</td>
    <td></td>
  </tr>
  <tr>
    <td>pretrain</td>
    <td>n</td>
    <td></td>
  </tr>
</table>


* Rest of training


<table>
  <tr>
    <td>Setting</td>
    <td>Value</td>
    <td>Notes</td>
  </tr>
  <tr>
    <td>iterations</td>
    <td>♾</td>
    <td>Until details appear in the last column of preview</td>
  </tr>
  <tr>
    <td>batch_size</td>
    <td>12</td>
    <td>12 if you can, if OOM errors occur, switch back to 8</td>
  </tr>
  <tr>
    <td>pixel_loss</td>
    <td>y</td>
    <td>Could cause model collapse, so backup: yes and keep an eye on it</td>
  </tr>
</table>


## Optional: History timelapse

Before converting, you can make a timelapse of the preview history (if you saved it during training). Do this only if you understand what `ffmpeg` is.

```

> cd \workspace\model\SAE_history

> ffmpeg -r 120 -f image2 -s 1280x720 -i %06d.jpg -vcodec libx264 -crf 25 -pix_fmt yuv420p history.mp4

```

## Convert

* Run `7) convert SAE`

<table>
  <tr>
    <td>Setting</td>
    <td>Value</td>
    <td>Notes</td>
  </tr>
  <tr>
    <td>mode</td>
    <td>1 (overlay)</td>
    <td>4 (seamless) also works well</td>
  </tr>
  <tr>
    <td>hist_match</td>
    <td>n</td>
    <td>None of the histogram match settings color match well</td>
  </tr>
  <tr>
    <td>mask_mode</td>
    <td>1</td>
    <td>Use learned*FAN-prd*FAN-dst if dst face has obstructions</td>
  </tr>
  <tr>
    <td>erode_modifier</td>
    <td>0</td>
    <td>If src face is bleeding outside the edge of dst face increase this to "erode" away the src face on the outside</td>
  </tr>
  <tr>
    <td>blur_modifier</td>
    <td>10-100</td>
    <td>Adjust depending on results</td>
  </tr>
  <tr>
    <td>scale_modifier</td>
    <td>0</td>
    <td>Adjust if scaling of src face is needed</td>
  </tr>
  <tr>
    <td>color_transfer</td>
    <td>rct</td>
    <td></td>
  </tr>
  <tr>
    <td>super_resolution</td>
    <td>y</td>
    <td>Adds more detail</td>
  </tr>
  <tr>
    <td>degrade_color_power</td>
    <td>n</td>
    <td></td>
  </tr>
  <tr>
    <td>alpha_channel</td>
    <td>n</td>
    <td>Outputs transparent PNGs for use in post-production tools</td>
  </tr>
</table>

* While conversion is running, you can preview the final images `data_dst\merged` folder to make sure it’s correct. If it’s not just close the convert window, delete `/merged` and start conversion again.
* Run `8) converted to mp4`
    * Bitrate of 3-8 is sufficient

## Done 🤡

