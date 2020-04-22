---
title: "Base Mesh Creation"
date: 2019-02-20T16:22:09+01:00
draft: false
---

This project is in fact a second attemp to implement a fast base mesh creation method from [this paper](https://pdfs.semanticscholar.org/2009/3aea25b50e59c63998ba0377371c59bf007f.pdf).
The main idea is to create a animable mesh from a skeleton whose joints are represented by spheres with variable radius defining the distance from the mesh vertices.
This mesh generation algorithm is decomposed into 3 steps: First a simple init mesh is computed connecting each joint with successive quad extrusion and convex hull computation for T-junctions.
The resulting mesh is then refined through an iterative process, alternating subdivisions and evolutions. Finally, a an edge fairing optimazation is performed whose puporse is to prevent quad deformation as much as possible without changing the geometry.





### BMesh Evolution example  ###

![Example 1](/Images/BMesh/bmesh0.gif)
![Example 2](/Images/BMesh/B_Mesh_1.gif)


