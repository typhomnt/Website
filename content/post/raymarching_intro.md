---
title: "An Introduction to Raymarching"
date: 2020-22-20T16:22:09+01:00
draft: false
math: true
html : true
css: style.css
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


This tutorial is part of the ray-tracing course available [here](../raytracing_practs).

## Introduction ##

In this tutorial, we will describe another rendering approach which is very similar to ray tracing but operates in a slightly different way considering that surfaces are defined by distance field, namelly Ray Marching. Indeed, ray tracing lie on the principle that we can compute analytically ray-surface intersections, being with triangles or more complex surfaces. However, this is can be limitation as we are bound to display only surfaces for which we are able to compute ray-surface intersection. Thus, Ray Marching comes in place and  allows us to display any implicit surface defined by an expression $f(x,y,z) = 0$, in other words a distance field. In our context the $f$ function will be referred as Signed Distance Function (SDF) meaning that when $f(p) > 0$ means that $p$ lie outside the surface and when $ f(p) < 0 $ it is inside.

The fundamental principle remains the same, for each pixel of the screen we cast a ray going from the camera center to the pixel but instead of computing intersection by solving an equation, we will go through the generated ray step by step and check if we intersect an object at each step. More precisely, for a given ray $r(t) = at + c_{camera}$ we will increase the value of $t$ until we have $f(r(t)) \leq 0$ meaning that the current point on the considered ray is inside a surface of our scene. 
\newline

There are two main ways of increasing the value of $t$ while going through a light ray: one is to increase $t$ by a constant small value until we intersect one surface while the other is to compute the minimal distance we can travel without intersecting anything by taking the minimum of all $f(r(t))$. This second approach is called Sphere (or Spherical) Marching and will be the approach we will use in this practical.
\newline

Below is the code of the ray marching main loop which is very similar to our previous trace function.
Notice that IntersectObjects has been replaced by rayMarch which will be the function returning the minimal $f(r(t))$ among all the scene surfaces. 

\noindent 



	vec3 march(in Ray r)
	{
		vec3 accum = vec3(0.0f);
		vec3 mask = vec3(1.0f);
		int nb_refl = 0;
		float c_refl = 0.3;
		Ray curr_ray = r;	
		for(int i = 0 ; i <= nb_refl ; i++)
		{
			ISObj io = rayMarch(curr_ray);
			if(io.t >= 0)
			{
				HitSurface hs = HitSurface(curr_ray.ro + io.d*curr_ray.rd
						,computeSDFNormal(io,curr_ray.ro + io.d * curr_ray.rd)
						,color,roughness,ao,metal) ;
				vec3 color = directIllumination(hs,true,c_refl);		
				accum = accum + mask * color;
				mask = mask*c_refl;
				curr_ray  = Ray(hs.hit_point + 0.001*hs.normal
				,reflect(curr_ray.rd,hs.normal));
			}
		}
		return accum;
	}

	ISObj rayMarch(in Ray r)
	{
		int nb_step = 255;

		float depth = 0.0f;
		float eps = 0.001;
		for (int i = 0; i < nb_step; i++) 
		{
			ISObj io = sceneSDF(r.ro + depth * r.rd);
			if (io.d <= eps) 
			{
				return ISObj(depth,io.t,io.i);
			}
			// Move along the view ray
		   	depth += io.d - DIST_MIN;

		   	 if (depth >= DIST_MAX) 
			 {
				// Gone too far; give up
				return ISObj(DIST_MAX,-1,-1);
			 }
		}

		return ISObj(DIST_MAX,-1,-1);
	}


Additionally, in this practical surfaces will be represented as surface operation units SdOpU which represent a surface operation between two SDF.
As seen during the lecture, it is quite easy to blend, unite, intersect or invert surfaces using the SDF representation. 


	//SDF operation Unit structure
	struct SdOpU
	{
		int s_t; /start type
		int s_id; //start id
		int e_t; // end type
		int e_id; //end id
		int op_id; // operation id
	};


Using this structure, we will need two additional functions to retrieve a SdOpU SDF and compute the distance with respect to an input point.

	float getSDF(int type, int id,in vec3 p)
	{
		if(type == 0)
		{
			return SDFPlane(planes[id],p);
		}
		else if(type == 1)
		{
			return SDFSphere(spheres[id],p);
		}
		else if(type == 2)
		{
			return SDFBox(boxes[id],p);
		}
		return 0.0;
	}

	float SDFSdOpU(in SdOpU unit, in vec3 p)
	{
		float s_sdf = getSDF(unit.s_t,unit.s_id,p);
		float e_sdf = getSDF(unit.e_t,unit.e_id,p);
		if(unit.op_id == 0)
		{
			return opUnion(s_sdf,e_sdf);
		}
		else if(unit.op_id == 1)
		{
			return opSubtraction(s_sdf,e_sdf);
		}
		else if(unit.op_id == 2)
		{
			return opIntersection(s_sdf,e_sdf);
		}
		else if(unit.op_id == 3)
		{
			return opSmoothUnion(s_sdf,e_sdf,1.0f);
		}
		
		return 0.0;
		
	}


The getSDF function returns the corresponding SDF related to the given surface type and its id while SDFSdOpU  compute the SdOPU operation between the two SDF of the unit. Below are the code for the different operation we will use.


	float opUnion( float d1, float d2 ) {  return min(d1,d2); }

	float opSubtraction( float d1, float d2 ) { return max(-d1,d2); }

	float opIntersection( float d1, float d2 ) { return max(d1,d2); }

	float opSmoothUnion( float d1, float d2, float k ) 
	{
	    float h = clamp( 0.5 + 0.5*(d2-d1)/k, 0.0, 1.0 );
	    return mix( d2, d1, h ) - k*h*(1.0-h); 
	}


Finally, in the same manner we implemented intersectScene in our ray tracing, we will implement the sceneSDF function, returning the distance to the closest surface in the scene. Additionally, we will compute the surface normal using its discrete derivative $f(p + \epsilon) - f(p - \epsilon)$


	const int suo_type = 10;

	ISObj sceneSDF(in vec3 point)
	{
	    ISObj nearest = ISObj(DIST_MAX,-1,-1);
	    for(int j = 0 ; j < suo_nbr ;j++)
	    {
	    	float dist = SDFSdOpU(suos[j],point);
	    	if(dist >= 0 && dist < nearest.d)
	    		nearest = ISObj(dist,suo_type,j);
	    }
	    return nearest;
	}



	vec3 computeSDFNormal(in ISObj is,in vec3 p) 
	{
		vec3 p_x_p = p + vec3(0.001, 0, 0);
		vec3 p_x_m = p - vec3(0.001, 0, 0);
		vec3 p_y_p = p + vec3(0, 0.001, 0);
		vec3 p_y_m = p - vec3(0, 0.001, 0);
		vec3 p_z_p = p + vec3(0, 0, 0.001);
		vec3 p_z_m = p - vec3(0, 0, 0.001);

		if(is.t == suo_type)
		{
			 return normalize(vec3(SDFSdOpU(suos[is.i],p_x_p) - SDFSdOpU(suos[is.i],p_x_m)
					,SDFSdOpU(suos[is.i],p_y_p) - SDFSdOpU(suos[is.i],p_y_m)
					,SDFSdOpU(suos[is.i],p_z_p) - SDFSdOpU(suos[is.i],p_z_m)
					));
		}

	    return vec3(0,0,0);
	}




## References ##

Open GL Tutorials: [https://learnopengl.com/](https://learnopengl.com/)  

Siggraph Courses: [http://blog.selfshadow.com/publications/s2013-shading-course/](http://blog.selfshadow.com/publications/s2013-shading-course/) [http://blog.selfshadow.com/publications/s2014-shading-course/](http://blog.selfshadow.com/publications/s2014-shading-course/) 

Real Time Rendering Advances: [http://advances.realtimerendering.com/](http://advances.realtimerendering.com/)

BRDF Model Comparison: [https://diglib.eg.org/handle/10.2312/EGWR.EGSR05.117-126](https://diglib.eg.org/handle/10.2312/EGWR.EGSR05.117-126)

Path tracing and Global Illumination: [http://www.graphics.stanford.edu/courses/cs348b-01/course29.hanrahan.pdf](http://www.graphics.stanford.edu/courses/cs348b-01/course29.hanrahan.pdf) [http://web.cs.wpi.edu/~emmanuel/courses/cs563/write_ups/zackw/realistic_raytracing.html](http://web.cs.wpi.edu/~emmanuel/courses/cs563/write_ups/zackw/realistic_raytracing.html)

GLSL / Shadertoy: [https://www.opengl.org/documentation/glsl/](https://www.opengl.org/documentation/glsl/) [https://www.shadertoy.com/](https://www.shadertoy.com/) [http://www.iquilezles.org/](http://www.iquilezles.org/)



