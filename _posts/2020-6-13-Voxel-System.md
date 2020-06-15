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

### Motivation
I came across an intriguing YouTube video: ["Coding Adventure: Marching Cubes"](https://www.youtube.com/watch?v=M3iI2l0ltbE) by Sebastian Lague. 

It's a simple process to generate a mesh from a field of values. I Immidiently was reminded of the games [Space Engineers](https://store.steampowered.com/app/244850/Space_Engineers/) and [Astroneer](https://store.steampowered.com/app/361420/ASTRONEER/) which probably use a similair system for their terrain. 

To challenge myself I've only used the video as reference and not looked at Sebastians code. I did take his advice to use an [existing triangulation table](http://paulbourke.net/geometry/polygonise/). But again I did not look at the code from that document.


### Project Content

This project is a proof on concept of a terrain generator which should be suitable for an open world sandbox game.
It has the following requirements:
- Generation need to be quick enough to generate new terrain live while playing.
- The terrain should be editable after generation.
- The terrain should be saved and loaded from the disk.

## Expample of simple terrain using the voxel system:
![Simple Terrain without props](/images/Voxel_Terrain_Simple.png "Simple Terrain without props")

## Expample of how the terrain is editable.
![whoops](/images/404.jpg "whoops") { width=50% }


### Project Structure
For this project I've choosen the Universal Project Template. This template uses the Universal Rendering Pipeline (URP) which formerly was know as Lightweight Render Pipeline.

In the scene hierarchy are the following relevant objects

> **[FA_World]**
> > Self spawning (as noted with the brackets) through a singleton .
> 
> **Terrain**
> > Has the Voxel Terrain Spawner (script) component which will spawn chunk objects as children.

