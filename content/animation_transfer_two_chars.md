---
title: "Animation Transfer for tow characters: Part 1"
date: 2019-02-20T16:22:09+01:00
draft: false
---

In the [animation transfer](../animation_transfer) article, I presented a method to create a animation sequence from a 6D trajectory. 
This method is centered around one character at a time. Here, I propose a way to extend it in order to take into account multiple characters and their interactions.




### Experiment 1: Don Juan and Ghost Scene  ###

This first experiment was done in the scope of an intership student's project.
The goal was to test our system in a context where the script was already written and where two chacters were "interacting" 
without any contact. We basically records the two Vive controller at the same time and compute one STD per character with our previous method.
The scene we chose is extracted from Don Juan and corresponds to the act V scene V in which Don Juan is being threaten by a ghost.


| Don Juan      | Ghost         |
| ------------- |:-------------:|
| Attack Neutral      | Pose|
| Attack Panic      |Stand |
| Back Away | Walk |
| Crouch |    Threaten   |
| Draw Sword | |
| Run |       |
| Sit Iddle |       |
| Stand Up Neutral | |
| Uncrouch |      |
| Walk Arms Forward |    |
| Walk Iddle | |

Here is a preliminary animation, showing the first part of the scene.

![DonJuan Spectre 1](/Images/Animation_Transfer/Anim_djs_eg1.gif)

### Detecting and Transferring Interactions ###

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: ["tex2jax.js"],
    jax: ["input/TeX", "output/HTML-CSS"],
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
      processEscapes: true
    },
    "HTML-CSS": { fonts: ["TeX"] }
  });
</script>
<script type="text/javascript"
   src="/Utils/MathJax/MathJax.js">
</script>

In the [animation transfer](../animation_transfer) article, we introduced how we defined and recognize single character action. Moreover they are represented as regular expression of motion tokens.
We propose to extend this formulation to character interactions. An interaction between n characters  is defined by two tuples : a n-uplet of actions corresponding to the regular expression each sould execute to trigger the interaction and a t-uplet of additional conditions the characters should follow. In our case we consider two types of conditions : distance between two characters and gaze angles between two characters. Conditions can be interected or united

$ \alpha \beta $




