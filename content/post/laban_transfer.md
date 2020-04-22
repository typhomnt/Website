---
title: "Laban Effort Animation Transfer"
date: 2019-02-20T16:22:09+01:00
draft: false
---

One important aspect of my thesis work is adding expressivity to an input "neutral" animation. In our work, we decided to take the Laban Effort space as the main representation of the expressivity.
This 4D space is divided into Space, Time, Weight and Flow axis. The Space axis describes how direct or indirect a movment is, Time describes if a movment is rather sudden or sustained while Weight differenciate Light from Strong motions. Finally Flow describes if a motion sequence is free or bound. In this article, I will only present how to transfer Time and Weight to a neutral animation.
To do so, I will first introduce three animation operators: scaling, retiming and shaping operators. 
### Scaling ###

### Efforts Comparison  ###
Finally, I show below the comparison of the 4 efforts for several animations of the Mixamo database
![Kick Laban](/Images/Laban_Transfer/kick_laban.gif)
![Punch Laban](/Images/Laban_Transfer/punch_laban.gif)
![Stomp Laban](/Images/Laban_Transfer/stomp_laban.gif)
![Throw Laban](/Images/Laban_Transfer/throw_laban.gif)

I personnaly think that those modifiers do the job as Laban Effort qualities are recognizable. That remains true as long as the animation phases are correctly defined. Still there are ways of improvment: first, foot sliding should be removed for light and strong modifiers; secondly, light and strong motions are sometimes too fast for some animations. This is due to the fact that some animations present long moments where there is no speed. 
<!---In furture experimentations, I will try to use the equi-affine speed instead of the speed for the retiming operator, taking into account animation breakdowns.-->


Comparisons:

![Raise Light](/Images/Laban_Transfer/Arm_Gesture_Light.mp4.gif)
![Raise Strong](/Images/Laban_Transfer/Arm_Gesture_Strong.mp4.gif)
![Raise Sudden](/Images/Laban_Transfer/Arm_Gesture_Sudden.mp4.gif)
![Raise Sustained](/Images/Laban_Transfer/Arm_Gesture_Sustained.mp4.gif)


![Punch Light](/Images/Laban_Transfer/Punch_Light.mp4.gif)
![Punch Strong](/Images/Laban_Transfer/Punch_Strong.mp4.gif)
![Punch Sudden](/Images/Laban_Transfer/Punch_Sudden.mp4.gif)
![Punch Sustained](/Images/Laban_Transfer/Punch_Sustained.mp4.gif)


![Jump Light](/Images/Laban_Transfer/Jump_Light.mp4.gif)
![Jump Strong](/Images/Laban_Transfer/Jump_Strong.mp4.gif)
![Jump Sudden](/Images/Laban_Transfer/Jump_Sudden.mp4.gif)
![Jump Sustained](/Images/Laban_Transfer/Jump_Sustained.mp4.gif)


![Flip Light](/Images/Laban_Transfer/Flip_Light.mp4.gif)
![Flip Strong](/Images/Laban_Transfer/Flip_Strong.mp4.gif)
![Flip Sudden](/Images/Laban_Transfer/Flip_Sudden.mp4.gif)
![RaFlipise Sustained](/Images/Laban_Transfer/Flip_Sustained.mp4.gif)


![Stomp Light](/Images/Laban_Transfer/Stomp_Light.mp4.gif)
![Stomp Strong](/Images/Laban_Transfer/Stomp_Strong.mp4.gif)
![Stomp Sudden](/Images/Laban_Transfer/Stomp_Sudden.mp4.gif)
![Stomp Sustained](/Images/Laban_Transfer/Stomp_Sustained.mp4.gif)
