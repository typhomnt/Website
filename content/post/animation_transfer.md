---
title: "Animation Transfer"
date: 2019-02-20T16:22:09+01:00
draft: false
---

One main goal of my thesis is to find a way to animate 3D characters using physical props as an animation tool. The motivation is to create an animation sequence from a play with figurines, trying to reproduce what the player was imagining.

 <!--- (I actually tried to tackle this task when I was still an ENSIMAG student during a 3 weeks project. At that time my first idea was to segment the curve using singular points and try to identify which action was performed by looking at the speed norm and direction. Basically, horizontal motions represented wlaking or running action while the other represented jumps and flying actions.
It is easy to identify the weaknesses of such an approach, it is limited to few actions and above all it is not really accurate, tweaking and thresholding is often needed for identifying singular points.
During my thesis I took inspiration from the paper untitled Motion Doodle where the authors were using 2D curves to created animation sequences . More particulary a given input curve, it was segmented into horizontal, vertical and oblic bins and animation were defined as a regular expression of curve bins. 
At that moment, I took a step back and told myself that they were creating a kind of motion language characterizing the curve by its different direction changes.) -->



### Space Time Doodle Transfer examples  ###
Below, I show transfered space-time doodles into animation sequences. It is important to note that all actions were learnt for the two first examples as well as for the garden example.
![Example 1](/Images/Animation_Transfer/Anim_eg1.gif)
![Example 2](/Images/Animation_Transfer/Anim_eg2.gif)
![Example 3](/Images/Animation_Transfer/Anim_eg3.gif)


