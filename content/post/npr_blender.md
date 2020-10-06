---
title: "Making a NPR Shader in Blender"
summary: Introduction to NPR inside Blender for a basic but complete shader.
date: 2019-02-20T16:22:09+01:00
draft: false
---

I have been playing with blender for more than three years now (still so much to learn) and I decided also to test some NPR stuff inside it as it is one of my main interest of research.
The first thing I did was to play around the shading node system as well as the post processing effects like Ambient Occlusion or Bloom and test various things. As I would like to focus on "real-time" rendering, I tested everything inside EEVEE even if it is more limited than Cycles. I also looked at many videos and works that have been done inside blender for NPR. 
If you are interested in the subject you can take a look at (or join) the Blender NPR community [here](https://www.facebook.com/groups/BNPRandFreestyle) and also check out the Lightning Boy Studio [youtube channel](https://www.youtube.com/channel/UCd9i2MKimSaKezat1xkn8-A) there have many resources for that field.

Here I am going to describe a version of a blender NPR that I build, it is very far from perfect but I hope it will give you a good start and understanding in order to start exploring NPR in Blender yourself.

### Shader Structure ###

The shader is represented as a node group which itself is divided into several groups that correspond to light/shadow contributions like I describe in detail in my [introspection to NPR article](../npr_shading).
More preciselly, this NPR shader light/shadow contribution are divided into 5 parts: Diffuse, Specular, Ambient occlusion, SubSurface Scaterring (SSS) and Emission.
Furthermore, I also include a Rim light and Outline contributions which are heavily present in NPR shaders.

First let me tease the kind of final results you can obtain with this shader. 



As you can see the range of rendering style can vary from very cartoonish shading to more realistic results.
The main tool to make such variations is to use color ramps to tweak the output of base shaders like the Diffuse or Glossy (Specular) shaders.
There is more to this of course and we will go into details on the next sections.

Here is an overview of what the node group looks like:


You can notice our 7 contributions nodes as well as how they are blended together. 
In this configuration you can notice that the main component Diffuse and Specular are added together with the Ambient occulsion terms darkening the objects through a multiplication with the Diffuse component. The SSS, Rim Light and Outline are blended in an additive fashion.

Let's now dig into each component independently.


### Diffuse Component ###

The 

### Specular Component ###


### Ambient Occlusion Component ###

### SubSurface Scaterring Component ###

### Emission Component ###


### Rim Light and Outline ###

### Blending ###
