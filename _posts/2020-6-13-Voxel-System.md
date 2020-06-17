---
layout: subtitlepost
title: Voxel Experiment Writeup
subtitle: Creating and Rendering Voxels in Unity
categories:
  - blog
  - article
  - project
tags:
  - game
  - unity
  - shader
published: true
---

## Motivation
A while ago I came across an intriguing YouTube video: ["Coding Adventure: Marching Cubes"](https://www.youtube.com/watch?v=M3iI2l0ltbE) by Sebastian Lague. 

It's a simple process to generate a mesh from a field of values. I was immediately reminded of the games [Space Engineers](https://store.steampowered.com/app/244850/Space_Engineers/) and [Astroneer](https://store.steampowered.com/app/361420/ASTRONEER/) which probably use a similair system for their terrain. 

To challenge myself I've only used the video as reference and not looked at Sebastians code. I did take his advice to use an [existing triangulation table](http://paulbourke.net/geometry/polygonise/). But again I did not look at the code from that document.


## Project Content

This project is a proof on concept of a terrain generator which should be suitable for an open world sandbox game.
It has the following requirements:
- Generation need to be quick enough to generate new terrain live while playing.
- The terrain should be editable after generation.
- The terrain should be saved and loaded from the disk.

### Expample of simple terrain using the voxel system:
![Simple Terrain without props](/images/voxel_terrain_simple.png "Simple Terrain without props")

### Expample of how the terrain is editable.
![Terrain Hobbit Hole](/images/voxel_terrain_simple2.png "Terrain Hobbit Hole")


## Project Structure
For this project I've choosen the Universal Project Template. This template uses the Universal Rendering Pipeline (URP) which formerly was know as Lightweight Render Pipeline.

In the scene hierarchy are the following relevant objects

### Root
> ### [FA_World]
> > Self spawning (as noted with the brackets) through a singleton .
> 
> ### Terrain
> > Has the Voxel Terrain Spawner (script) component which will spawn chunk objects as children.



## Some title or segway idk

//explain single voxel (and that it requires 4 points values)
The term voxel means volumentric pixel, this can be any representation the simplest to understand being cubes (e.g. Minecraft terrain) every pixel (voxel) has a 3D location and on that location a cube is rendered. Since a cube is already a recognisable (and stackable) shape it's trivial to make a surface representation. The voxel system from this project is sligthy more complex. The pixels (voxels) can have multiple "shapes" (15 unique) and these shapes are found in the triangulation table.

Which shape a voxel has is determined by 8 values. To get these values we sample points from a 3D grid. If we make sure that every point is also used for the next voxel we automatically get a coherent surface. Because of this I will call the collection of 8 pixels a "voxel" and the values (which are technically the voxels) "voxel points"

//explain voxelchunks
To make the terrain generatable while playing I divided the voxels in chunks of values. Deciding the exact specifications of a single chunk is quite tricky. The smaller you make them the faster you can spawn them (generate/load the values, sampling every voxel creating the mesh data and the (abstracted behind Unity) sending of the mesh data to the gpu). However this also induces lots of overhead when you want lots of chunks at the same time (Keeping track of which chunks are loaded, disk i/o of actual the actual loading).
