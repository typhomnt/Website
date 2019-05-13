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


![DonJuan Spectre 1](/Images/Animation_Transfer/Anim_djs_eg1.gif)




