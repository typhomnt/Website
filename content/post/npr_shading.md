---
title: "A Introspection on Non-Photorealistic Rendering"
date: 2019-02-20T16:22:09+01:00
draft: false
---

As I dived into the NPR world, I came across several problems and questions. 
The first one was, of course, what is NPR ? Looking at the wikipedia page at this date (April 2020), NPR "focuses on enabling a wide variety of expressive styles for digital art" like  "painting, drawing, technical illustration, and animated cartoons". The main idea is to repoduce expressive styles like cartoons using computers. But this definition also suggests that the goal of NPR is also to go futher than expressing existing traditional styles, meaning that we can create new styles with the new tools we have in our hands. And this can be done either in 2D or in 3D using our powerful computers. 

In my main reseach, I particularly focus on  expressing those styles through 3D content. The main reason I chose this path is because, while I am constantly mesmerised by what 2D digital artist like WLOP or Kuvshinov Ilya are producing, I always find 2D content as "incomplete" somehow. To be more specific, I feel like the integrety of what 2D artist are creating only belongs to their creators as their are the only one who can draw faithfully their characters, buildings, landscapes and environment. I know that this assertion is not completly true, because this skill can be transmitted like it as been done for series like Asterix and Obelix. Still, in comparison building 3D scene with models, illumination scheme, materials, textures, etc... better ensures the faithfullness of one universe and manipulate contents that are highly reusable. In fact, this comparison between 2D and 3D came clearer to me when I learned how video games and 3D animated movies were made. Indeed, one of the first step is to draw concept art of characters, weapons, environment and so on. Then 3D artists build 3D content from those concepts. While studios prefer 3D content over 2D for financial reasons and because skilled 2D animators are difficult to find, I believe that the overall integrety is better preserved.  

Closing this paranthesis, lets go back to the objective of NPR I described above and which is still quite vague. Thus, I wanted to be clearer about the styles I want to express especially through references. In the litterature and on the web we can find tons of different styles so I had to classify them. Actually, a lot of them are referenced by the tool used to create the paintings or the drawings, like Watercolor, Stippling, Pastel and so on. Note that styles are not only defined by the tool used to produce paintings but also includes how colors, shapes, brush strokes and so on, are used and represented.

The tool is actually composed of three components: the drawing material (paint, ink, charcoal), the brush stroke (pen, pencil, brushes) and the texture of the support. At this point I have to mention that I consider digital painting as providing new elements to those components.

These tool are then the mean of creating 2D content but it does not define the actual composition of what we can find on these drawings. 
In my current research I decided to decompose the actual content as a combination of Line Arting, Shading and Brushing.

# Line art #

Line Art depicts object silhouette and inner details with a varying thickness. While 2D cartoons use them a lot, many other styles do not include line art at all. 
In our 3D context, there are various ways of computing silhouttes and inner contour lines. Many of those techniques are referenced in the survey of [Benard et al](https://hal.inria.fr/hal-02189483/document) explaining how to extract line drawings from 3D models. 




# Shading #

Shading refers to how light interact with object surfaces and is actually a notion that has various mathematical representations like the one I present in this article on [Physically-Based Rendering](). 

Shading described the color and intensity of light that goes into our eye is the result of different contributions. Here, I cornered them to: 

- Diffusion 
- Reflection (specular)
- Metalness
- Roughness
- Ambient Occlusion (Distance to point of interest, screen space curvature maxima or minima)
- Ambient/environment/Indirect Lighting
- Subsurface scattering
- Emission
- Absorption
- Transparency
- Rim lighting

I deliberatly merged material properties with light behavior functions because I also consider the functions as a property of the surface we want to represent. Ideally, these functions are all identical for each object of one drawing to ensure consistency. Those different lighting components have been quite studied for Photorealistic rendering which makes the link between physic representation and 3D graphics.
However, I believe that NPR has still quite a room for improvment for integrating all those components which are taken into account by artists in many different ways. 
I even believe that there is something like a continuity between NPR and Photorealistic rendering. This is exatcly what I will focus on try to introduce in this article.

To do so, we need a generic shading model that will allow us to cover a wide range of artistic styles while being able to generate more photorealitic renderings. In this article, I will narrow the problem for a real-time rendering pipeline and thus focus on a model that works on G-Buffers and on screen-space versions of the light components I described above. 

To achieve our goal, I will detail how each light conponent is computed and how we can create a continuum from NPR to Photorealism (or the other way around). In addition, I want to highlight the fact that tweaking the light component will not be sufficient enough to obtain a large range of NPR styles, we also need to be able to simplify the geometry and to apply different blending modes.

## Inspiration Examples  ##

Before diving into the technical part, let me show you some examples of the range of artisic styles we want achieve and point out what they have in common and their differences. First, let's look at traditionnal style before seeing how humans tried to reproduce them using computers. 

## Lighting Components ##

It is time to dive into the math and physics of each lighting component as well as their simplification. Like I introduced in the [Physically-Based Rendering]() the amount of outgoing light at a specific point of one surface and at a specific point of view if given by the light equation itself described by a light function that we called a BRDF in the tutorial. This function basically allow us to estimate the diffuse and specular intensity at any given point of the surface which depends on the incomming light but also of the material properties like the roughness and the metalness. However, you can notice that this function do not take into account the transimission and absorption of the light. Taking them into account necessitate to use BRDF of higher dimensions which are out of the scope of real-time rendering by the time I am wrtiting this article. To simulate absorption and Subsurface scattering (or SSS) we will resort to some tricks which are quite efficient while not fully realistic. 
In the following sections I will descrbe how each light component is formulated in a mathematical fashion while skipinning the relation with the light equation.

## Diffusion ##


Light diffusion is the phenomenum where lights enters inside the objects surface and is then re-emitted after mupltiple outside its surface usually inside the normal hemisphere defined by the surface normal at the ncomming light ray. 

Mathematically speaking the amount of light diffused at any given point of the surface is computed through the scalar product between the light direction and the normal 

 

## Simplifying the geometry ##

One main limitation of tweaking the lighting component as we did in the previous section is that we tweak only a very local information whereas some styles need to take into account neighbouring regions.
Let me show you why.
Normal Bilateral filtering. 


## Parameterization ##



## Color Blending  ##


# Brushing #

When drawing in 2D, being digitaly or traditionnaly, the shading and the texture of the brush are drawn at the same time. Here, I allow myself to decompose those two aspect and define brushing as applying a brush stroke (texture) colored either with a specific color or with a more sophisticated shading (brush strokes can be also shaded themselves).



# Canvas Texture #

Brush are applied to textured canvas which play a role 

# Tone mapping and Adjustments #



# NPR in animation  #

Here is an example of toon shading during an uppercut jab animation downsampled at 15 fps

![Example 1](/Images/SkyEngine/Toon_shading.gif)
![Example 2](/Images/SkyEngine/Toon_Shading_Shadow.gif)


