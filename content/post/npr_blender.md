---
title: "Making a NPR Shader in blender"
date: 2019-02-20T16:22:09+01:00
draft: false
---

I have been playing with blender for more than three years now (still so much to learn) and I decided also to test some NPR stuff inside it as it is one of my main interest of research.
The first thing I did was to play around the shading node system as well as the post processing effects like Ambient Occlusion or Bloom and test various things. As I would like to focus on "real-time" rendering, I tested everything inside EEVEE even if it is more limited than Cycles. I also looked at many videos and works that have been done inside blender for NPR. 
If you are interested in the subject you can take a look at (or join) the Blender NPR community [here](https://www.facebook.com/groups/BNPRandFreestyle) and also check out the Lightning Boy Studio [youtube channel](https://www.youtube.com/channel/UCd9i2MKimSaKezat1xkn8-A) there have many resources for that field.

Here I am going to describe a version of a blender NPR that I build, it is very far from perfect but I hope it will give you a good start and understanding in order to start exploring NPR in Blender yourself.

### Shader Structure ###

The shader is represented as a node group which itself is divided into several groups that correspond to light/shadow contributions like I describe in detail in my [introspection to NPR article]().
More preciselly, this NPR shader light/shadow contribution are divided into 5 parts: Diffuse, Specular, Ambient occlusion, SubSurface Scaterring (SSS) and Emission.
To this components, we also add a Rim light and Outline contribution which are heavily present in NPRs.

First let me tease the kind of final results you can obtain with this shader. 


