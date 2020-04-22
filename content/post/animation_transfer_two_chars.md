---
title: "Animation Transfer for two characters: Part 1"
date: 2019-02-20T16:22:09+01:00
draft: false
---

#{{< protected  path="." >}}


In the [animation transfer](../animation_transfer) article, I presented a method to create a animation sequence from a 6D trajectory. 
This method is centered around one character at a time. Here, I propose a way to extend it in order to take into account multiple characters and their interactions.




### Experiment 1: Don Juan and Ghost Scene  ###

This first experiment was done in the scope of an intership student's project.
The goal was to test our system in a context where the script was already written and where two chacters were "interacting" 
without any contact. We basically records the two Vive controller at the same time and compute one STD per character with our previous method.
The scene we chose is extracted from Don Juan and corresponds to the act V scene V in which Don Juan and his valet Sganarelle are arguing.
The scene was played in 4 different versions, with different stagings and actions.


| Don Juan      | Sganarelle         |
| ------------- |:-------------:|
| Walk      | Walk|
|Walk      |Run |
| Walk | Stopping |
| Yell |       |
| Arm Gesture | |
| Walk |      Walk |


## Version 1 ##
![DonJuan Sganarelle 1](/Images/Animation_Transfer/DJSga1.gif)
## Version 3 ##
![DonJuan Sganarelle 3](/Images/Animation_Transfer/DJSga3.gif)


### Experiment 2: Fight Scene###

While producing dramatic animation with different staging has proved efficient, I also tested the system on more dynamic scene
such as a street fighter like fight.
![Fight 1](/Images/Animation_Transfer/Fight1.gif)


### Detecting and Transferring Interactions ###

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  tex2jax: {
    inlineMath: [['$','$'], ['\\(','\\)']],
    displayMath: [['$$','$$'], ['\[','\]']],
    processEscapes: true,
    processEnvironments: true,
    skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
    TeX: { equationNumbers: { autoNumber: "AMS" },
         extensions: ["AMSmath.js", "AMSsymbols.js"] }
  }
});
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Queue(function() {
    // Fix <code> tags after MathJax finishes running. This is a
    // hack to overcome a shortcoming of Markdown. Discussion at
    // https://github.com/mojombo/jekyll/issues/199
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>


<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>


<!--- for offline
<script type="text/javascript"
   src="/Utils/MathJax/MathJax.js">
</script> -->



In the [animation transfer](../animation_transfer) article, we introduced how we defined and recognize single character action. Moreover they are represented as regular expression of motion tokens.
We propose to extend this formulation to character interactions. An interaction between n characters  is defined by two tuples $A_n $: a n-uplet of actions corresponding to the regular expression each should execute to trigger the interaction and a t-uplet $C_t$ of additional conditions the characters should follow. In our case we consider two types of conditions : distance between two characters $|P_i - P_j| \leq \alpha $ and gaze angles between two characters $ R_i \cdot R_j \leq \beta  $ or $ (P_j - P_i) \cdot R_i \geq \gamma$ with $\gamma \geq 0$. Conditions can be intersected or united. We note an interaction $I$ as an uplet $ \\{ A_n,C_t \\} $. Interaction recognition is perfomed as follows : each character doodle is read in parallel, while proceeding to the action recognition alogrithm for both independent doodles we look for token sequences matching interactions regular expression. Thus for each doodle a "current" interaction token sequence is stored and updated at each new read token tuple.
Then for each interaction $I$ between n characters, we search for all n-uplets satifying the interaction cues and regular expressions, testing the $n!*(k-n)$ configurations, $k being the number of characters$. In practice $n,k \leq 4$. When testing if a combinaison of $T_n$ token sequences matches an interaction regular expressions, several ordering might satifsfy them. In that case, for each regular expression $R_i$ of $I$ we keep the longest sequence $S_j$ matching it. When all sequences $\{S_i\}$ as well as all cues $\{C_i\}$ are satisfied, each characater with sequence $S_j$ will execute the related action of the corresponding matched regular expression $R_i$.


Below we show an example of a Kiss interaction whose regular expressions is identical to the Stomp action of each character. The Kiss interaction presented has three cues to be satisfied: the two characters must see each others (2 conditions) and they must be close enough (in the intimate space). The trajectory of the two examples are exactly the same, only the character sizes differ, that's why in the second example the Kiss interaction is not detected, the proximity cue fails.

## Kiss Interaction ##
![Bots Kiss](/Images/Animation_Transfer/Bots_Kiss.gif)
## Kiss Fail -> Stomps ##
![Bots Stomp](/Images/Animation_Transfer/Bots_Stomp.gif)


