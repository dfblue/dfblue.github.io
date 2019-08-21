---
layout: post
title:  "Deepfake Tools Overview July 2019"
author: "DeepFakeBlue"
image: 
date:   2019-08-08
categories: [deepfake, software, tools, technology, deepfacelab, faceswap]
---

Welcome to DeepFakeBlue’s first deepfake software overview. In this publication, we’ll be looking at the top two deepfake applications that users around the web are using. Most, if not all, of those Keanu, Trump, and Jim Carrey deepfakes you’ve seen on YouTube were made using these two tools! Let’s take a deeper look.

## DeepFaceLab

![DeepFaceLab](https://i.imgur.com/LPeZ7z6.jpg)

[https://github.com/iperov/DeepFaceLab](https://github.com/iperov/DeepFaceLab)

DeepFaceLab has been in development since June 4th, 2018 on GitHub. It seems to have roots in the old deepfakes.club software as the url [deepfakes.club](https://deepfakes.club) now points to the GitHub repository. For DeepFakeBlue, it worked smoothly the first time we used it with the 2019-06-20 version. 

Unfortunately the English instructions included with DeepFaceLab are Google Translated from Russian and quite difficult to understand, especially to newcomers to deepfaking. But don’t worry, we put together a [tutorial for using DeepFaceLab](https://pub.dfblue.com/pub/2019-07-27-deepfacelab-tutorial) that pulls together best practices and notes from multiple forums, threads, and our own experience.

DeepFaceLab is a bit overwhelming at first because the tool consists of many .bat script files that you have to double click to run in a certain order. But once you get used to the process, it feels natural. I’m a software engineer, but I don’t think you would need any software skills to use it, even though it isn’t a full GUI like FaceSwap.

Regarding the code and repository itself, it leaves a lot to be desired when it comes to documentation. The commit messages are short and cryptic, which makes it difficult to contribute to the project, or even understand its development history. 

I don’t write python, so I haven’t delved deep into the code itself, but it looks modularized and thought out. The trickier parts are event commented. For example in a recent commit ([https://github.com/iperov/DeepFaceLab/commit/8484060e01b7139cbc900f932e10f96f869ac5c9](https://github.com/iperov/DeepFaceLab/commit/8484060e01b7139cbc900f932e10f96f869ac5c9)) we get some details on the return type of this function, which could be hard to grok: 

```
...
#overridable , return [ [model, filename],... ]  list
...
```

Regarding cloud usage of DeepFaceLab, @iperov are testing Google Colab support in the main repository (it was already available in a fork https://github.com/chervonij/DFL-Colab). See [https://github.com/iperov/DeepFaceLab/tree/colab_test](https://github.com/iperov/DeepFaceLab/tree/colab_test) 

Our biggest concern about DeepFaceLab is a [commit](https://github.com/iperov/DeepFaceLab/commit/3d29130d5c68b816fcb69b771c2d5dc58351e2e2) from June 12th, 2019 with the title "closing the repo". It seems as if `iperov`, the main developer decided to abandon the project because of lack of donor support.

![closing the repo](https://i.imgur.com/6AjW9Po.jpg)

However, a week later on June 19th, `iperov` reversed their decision and removed this statement from the Readme. In our eyes, this puts DeepFaceLab’s future development on unstable ground.

## FaceSwap

![FaceSwap](https://i.imgur.com/OHawBOf.jpg)

[https://github.com/deepfakes/faceswap](https://github.com/deepfakes/faceswap)

FaceSwap’s initial commit was on December 17th, 2018 by GitHub user `joshua-wu`. It’s lineage is difficult to understand because it is related to a tool called OpenFaceSwap that is now defunct, but OpenFaceSwap’s website deepfakes.club now points to DeepFaceLab’s GitHub repo. If anyone has more details on FaceSwap’s lineage, please reach out to us via [GitHub issues](https://github.com/dfblue/dfblue.github.io/issues) or on Reddit (u/deepfakeblue).

The key advantage FaceSwap has over other tools is its GUI. The authors have translated all the python script files into a point-and-click interface where a user can select files and options easily. However, you can also easily access the underlying python scripts and run those individually as well. This theoretically gives us accessibility as well as customization and automation.

FaceSwap has a very different personality than DeepFaceLab. The main developers `torzdf`, `bryanlyon` and `kilroythethird` have been active in the last few months, adding features and fixing bugs at a rapid pace. They also take effort to document their development and git commits making it fairly easy to follow the progress.

The authors also give special attention to keeping deepfake usage ethical. Although they cannot stop anyone from using it in nefarious ways, they at least add a statement to the very beginning of the [Readme](https://github.com/deepfakes/faceswap/blob/master/README.md) to show us they are thinking about how their tools might be used.

Another real benefit of using FaceSwap is the active [discord server](https://discordapp.com/invite/FC54sYg) for asking questions and getting help. That plus priority support for donors makes it a go-to choice for larger projects. DeepFakeBlue has not had a chance to verify that this support actually happens. Since we are so early in the technology that bugs and issues will no doubt surface, this is a huge advantage over DeepFaceLab, which at the time of writing does not have a support forum or chat community. Also listed on their Readme is implementing specific features and algorithms in exchange for donor support. Having this kind of say in the feature development could go a long way because new methodologies for the various parts of the software are being published every week on arxiv.

And if all that wasn’t enough, there is a great [forum](https://faceswap.dev/forum/index.php) with extensive FAQ and info on the various parts of the pipeline.

Recent example deepfake done with FaceSwap:

[https://faceswap.dev/blog/faceswap-featured-in-corridor-digital-video](https://faceswap.dev/blog/faceswap-featured-in-corridor-digital-video) 

Keanu Reeves Stops A ROBBERY! - [https://www.youtube.com/watch?v=3dBiNGufIJw](https://www.youtube.com/watch?v=3dBiNGufIJw)

## Side-by-side

<table>
  <tr>
    <td></td>
    <td>DeepFaceLab</td>
    <td>FaceSwap</td>
  </tr>
  <tr>
    <td>GitHub commits</td>
    <td><img src="https://imgur.com/xLkyqQB.jpg"></td>
    <td><img src="https://imgur.com/LbwPw4X.jpg"></td>
  </tr>
  <tr>
    <td></td>
    <td><img src="https://imgur.com/zjAI3Xu.jpg"></td>
    <td><img src="https://imgur.com/6S2MJIr.jpg"></td>
  </tr>
  <tr>
    <td></td>
    <td><img src="https://imgur.com/Cn2EnIt.jpg"></td>
    <td><img src="https://imgur.com/tv12IxN.jpg"></td>
  </tr>
  <tr>
    <td>Release process</td>
    <td>Uploaded to Mega or Google Drive</td>
    <td>GitHub Releases (latest code pulled from GitHub during install)</td>
  </tr>
  <tr>
    <td>Main developer</td>
    <td><img src="https://imgur.com/MSf3nNR.jpg"></td>
    <td><img src="https://imgur.com/E7cEZ5T.jpg"></td>
  </tr>
  <tr>
    <td>Official guide</td>
    <td><a href="https://github.com/iperov/DeepFaceLab/blob/master/doc/manual_en_google_translated.pdf">guide</a></td>
    <td><a href="https://faceswap.dev/forum/viewtopic.php?f=4&t=20">guide</a></td>
  </tr>
  <tr>
    <td>Official support community</td>
    <td>None</td>
    <td>https://discordapp.com/invite/FC54sYg
https://faceswap.dev/forum/</td>
  </tr>
  <tr>
    <td>Google Colab</td>
    <td>Supported</td>
    <td>No out of the box support</td>
  </tr>
</table>

## Honorable mention?

### dfaker/DF

[https://github.com/dfaker/df](https://github.com/dfaker/df) 

DeepFakeBlue has not evaluated DF since it is much less user friendly than DeepFaceLab and FaceSwap, has little or no support community and generally looks to be defunct as the last commit was on Feb 23rd, 2018. **Additionally, the df model has been merged into FaceSwap.**

## Future updates

Since this is our first overview of deepfake tools, we went deeper into the tools and features, but going forward we will focus only on new happenings in the given month. We’ll look at commits and break down new features, bug fixes, as well as new papers and articles relating to deepfakes.

## Follow us on [Twitter](https://twitter.com/dfblue) or [Reddit](https://reddit.com/u/deepfakeblue) for the next update.

