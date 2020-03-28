---
title: "An Introduction to Physically Based Rendering"
date: 2020-22-20T16:22:09+01:00
draft: false
---

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


This tutorial is inspired from [https://learnopengl.com/PBR/Theory](https://learnopengl.com/PBR/Theory) and is adapted for the ray-tracing course available [here](../raytracing_practs).

One important challenge of 3D graphics is to design shading models which mimic real-life lighting behaviour while allowing intuitive control of object material. Is is also important to be able to make approximations that can provide real time feedback depending on the memory and computations time cost one can afford.

In this tutorial, we are interested in Physically Based Rendering (PBR) models, that contrally to simpler model like the Phong model, aim at simulating light behaviour in a more realistic way, approximating light related equations.
One important aspect of those models is that there are energy conservative, meaning that the amount of light absorbed, scatterned and emitted by object surfaces is equal to the amount of light received on the surface. 

Modeling this behaviour is highly correlated with how we represent object surfaces.  Indeed, light interaction with object surface is modeled by the Snell-Descartes law which describes how incident light gets refracted and reflected on a flat surface.

In practice object surfaces are not completly flat, some are rougher than the others. Thus, the microfacet model was introduced which defines a surface by a continous sequence of flat micro-surfaces that might be oriented differently, similutaing the smooth vs rought aspect of surfaces. This roughness feature is very important as it controls the amount of light that gets reflected and refracted.

Having stated all of this principles, our main goal remain the same, compute the color received by the eye for each pixel. We thus are interested in the color and intensity of light that either gets directly reflected from the surface to the eye and light that gets refracted and then re-emitted by the object through diffusion considering that all the light that gets absorb is lost. In addition, in this tutorial we will neglect the effect of scattering which gives more realistic results but is more costly to compute. 

The amount of light reflected at a specific direction at a given point of an object surface is given by the reflectance equation:

$$ L_o(p,v) = \int_A f_r(p,l,v) L_i(p,l)\, n \cdot l  \,dl $$

Where p is the point of interest on the object surface, v the direction from p to the eye, $L_o$ the RGB color persived by our eye from p, l the incident light direction , $L_i$ the incident light radiance on p from direction l, $f_r$ a function controlling the amount of light relfected to direction $v$ with respect to the material property at p and A the hemisphere surounding p on which we integrate all incoming light directions.

In our ray tracing case, we restrict incoming light sources to a given number of point light sources. Thus, the integral over $A$ can be tranformed into a sum over the different $p$ to light source directions. and $$ L_o(p,v) = \sum_l f_r(p,p-c_l,v) L_i(p,p-c_l)\, n \cdot \frac{(c_l-p)}{\left\lVert   c_l - p \right\rVert}  $$

The incoming reflectence $L_i(p,p-c_l)$ equals to the intesity of the light source $I$, multiplied by its color $C$, and weighted by an attenuation factor which depends on the distance to the source. 
In our case, we will consider that the intensity of light decrease with the square distance to the source: $$L_i(p,p-c_l) = \frac{IC}{{\left\lVert p - c_l\right\rVert}^2} $$.

The only unknown left is the $f_r$ function that controls the amount of reflected light with respect to materials properties. This function is called a BRDF which stands for Bidirectional Reflective Distribution Function. Several functions were proposed to simulate real-life materials behavior but all of them respect the energy conservation law, meaning that the anount of outgoing light do not exceed the anount of incoming light and above all the later is divided between reflected and refracted light. An important property of the BRDF functions is that there are intresectly symetric with respect to incomming and outgoing light because of the principle of reversibility of light.
In our case, we will use the Cook-Torrance BRDF model composed of a diffuse and a specular part: 

$$f_r = k_d f_l + k_s f_c$$

where $k_d$ is the amount of refracted light that gets re-emitted and $k_s$ the amount of relfected light with: 

$$ k_d = 1 - k_s $$

The $f_{l}$ function is the Lambertian diffusion distibution (which corresponds to the diffuse part of the Phong model). It considers that the diffused light is equally spread on all direcion:
 $$ f_l = \frac{C}{\pi} $$ 
Where C is the albedo of the object surface at point $p$. We can notice that the dot product between the normal and the light direction is done outside this function and is still present in the sum of in going contribution. $\pi$ is a normalization factor which accounts for the fact that we intgrate ingoing light over the hemisphere at point $p$. 

At this point it is important to mention that we must differenciate between two type of material: metals and di-electric (non metal) materials.
Indeed, while di-electric materials diffuse lights, it is not the case of metals that absorb all refracted light thus $k_d = 0$ for metals (with $k_s \leq 1$ because light still get refracted).

On the other side the $f_c$ is composed of two terms:

$$f_c = \frac{DG}{4(l \cdot n)(v \cdot n)}$$

with D called the Normal Ditribution Function and G the Geometry function. Additionally, $k_s = F$ where $F$ is the Fresnel term which describe the amount of light that gets refracted on a more macroscopic scale with respect to the point of view. The Fresnel term results from an equation that is not easy to solve, however it can be approximated using the Fresnel-Schlick approximation: 

$$ F_{Schlick}(h, v, F_0) = F_0 + (1 - F_0) (1 - (h \cdot v))^5 $$ 

With $F_0$ being the base reflectivity of the material and $h = \frac{l + v}{\left\lVert l + v \right\rVert}$ the half vector which correspond to the normal one facet should have to directly reflect the light into the eye.
This equation tells us that when $h$ and $v$ are perpendicular the amount of light reflect is at its maximum, in other words relfections occurs more at gazing angles. This effect can be easely noticed on puddles or wooden surfaces when looking from a top view or from a gazing angle. $F_0$ is computed as the amount of reflected light at normal incidence where $v$ and $l$ are colinear.
This equation, only.

Let us describe the micor-facet model in more details. We mentionned the roughness parameter that plays a role in the amount of reflect light. More concretly, this parameter desribe the amount of micro-facet that are aligned, where rough surface have a chaotic orientation distribution contrally to smooth surfaces:


Those micro-facets represents the surface of the object and their orientation directly affect the direction of reflected light:

That's why smooth surfaces typically behave like mirrors while reflections are blurier on rough surfaces.
We can now describe the role of the Normal Distribution Function D and the Geonetric function G.
D represent the amount of microfacet that are in the direction of the half vector $h$. This is the same as computing the amount of reflected light rays that are colinear to the view vector  $v$.
In our case, we chose the GGX distribution function as NDF:

$$D = NDF_{GGX TR}(n, h, \alpha) = \frac{\alpha^2}{\pi((n \cdot h)^2 (\alpha^2 - 1) + 1)^2}$$


The Geometric function simulates two phenomena that occurs between micro-facets namely obstruction and shadowing. In the two cases, either the incoming light cannot reach some micro-facets because there are in the shadows of others (shadowing) or reflected light is blocked by other facets (obtruction). Therefore, some amount of reflected light is "lost" and this is exactly the information given by the Geometric function. In our case we chose the Schlick-GGX approximation:

$$    G_S(n, v, k) =  \frac{n \cdot v}{(n \cdot v)(1 - k) + k } $$

Taking into account the two effects:

$$   G(n,v,l,k) =  G_S(n, v, k)  G_S(n, l, k) $$










