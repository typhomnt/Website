---
title: "A Introspection on Non-Photorealistic Rendering"
date: 2019-02-20T16:22:09+01:00
draft: false
---



As I dived into the NPR world, I came across several problems and questions. 
The first one was, of course, what is NPR ? Looking at the wikipedia page at this date (April 2020), NPR "focuses on enabling a wide variety of expressive styles for digital art" like  "painting, drawing, technical illustration, and animated cartoons". The main idea is to repoduce expressive styles like cartoons using computers. But this definition also suggests that the goal of NPR is also to go futher than expressing existing traditional styles, meaning that we can create new styles with the new tools we have in our hands. And this can be done either in 2D or in 3D using our powerful computers. 

In my main reseach, I particularly focus on  expressing those styles through 3D content. The main reason I chose this path is because, while I am constantly mesmerised by what 2D digital artist like WLOP or Kuvshinov Ilya are producing, I always find 2D content as "incomplete" somehow. To be more specific, I feel like the integrety of what 2D artist are creating only belongs to their creators as their are the only one who can draw faithfully their characters, buildings, landscapes and environment. I know that this assertion is not completly true, because this skill can be transmitted like it as been done for series like Asterix and Obelix. Still, in comparison building 3D scene with models, illumination scheme, materials, textures, etc... better ensures the faithfullness of one universe and manipulate contents that are highly reusable. In fact, this comparison between 2D and 3D came clearer to me when I learned how video games and 3D animated movies were made. Indeed, one of the first step is to draw concept art of characters, weapons, environment and so on. Then 3D artists build 3D content from those concepts. While studios prefer 3D content over 2D for financial reasons and because skilled 2D animators are difficult to find, I believe that the overall integrety is better preserved.  


Closing this paranthesis, lets go back to the objective of NPR I described above and which is still quite vague. Thus, I wanted to be more precise on the styles I want to express especially through references. In the litterature and on the web we can find tons of different styles so I had to classify them. Actually, a lot of them are referenced by the tool used to create the paintings or the drawings, like Watercolor, Stippling, Pastel and so on. 

The tool is actually composed of three components: the drawing material (paint, ink , charcoal), the brush stroke (pen, pencil, brushes) and the texture of the support. At this point I have to mention that I consider digital painting as providing new elements to those components.

These tool are then the mean of creating 2D content but it does not define the actual composition of what we can find on these drawings. 
In my current research I decided to decompose the actual content as a combination of Line Arting, Shading and Brushing.

### Line art ###
Line Art depicts object silhouette and inner details with a varying thickness. While 2D cartoons use them a lot, many other styles do not include line art at all. 
In our 3D context, there are various ways of computing silhouttes and  


### Shading ###

Shading refers to how light interact with object surfaces and is actually a notion that has various mathematical representation like I present in this article on Physically-Based Rendering. 

This part can be composed of different effects. In my research, I cornered them to: 
- Diffusion
- Reflection (specular)
- Metalness
- Roughness
- Ambient Occlusion
- Subsurface scattering
- Absorption
- Transparency
- Rim lighting

I deliberatly merged material properties with light behavior functions because I also consider the functions as a property of the surface we want to represent. Ideally, these functions are all identical for each object of one drawing to keep a certain consistency.

### Brushing ###

Normally when drawing in 2D, being digitaly or traditionnaly, the shading and the texture of the brush are drawn at the same time. Here, I allow myself to decompose those two aspect and define brushing as applying a brush stroke (texture) colored either with a specific color or with a more sophisticated shading (yes brush strokes can be also shaded themselves).




### Toon Shading in animation  ###

Here is an example of toon shading during an uppercut jab animation downsampled at 15 fps

![Example 1](/Images/SkyEngine/Toon_shading.gif)
![Example 2](/Images/SkyEngine/Toon_Shading_Shadow.gif)


