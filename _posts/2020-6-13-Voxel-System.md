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

The aim of this article is just to demonstrate how I interperated and implemented the "marching cube" algorithm but not to explain the concept of polygonising or the marching cube algorithm. If you want to have a good explenation I recommend using [Sebastian Lague's video](https://www.youtube.com/watch?v=M3iI2l0ltbE) and the listed resources from his video description.


## Project Content

This project is a proof on concept of a terrain generator which should be suitable for an open world sandbox game.
It has the following requirements:
- Generation need to be quick enough to generate new terrain live while playing.
- The terrain should be editable after generation.
- The terrain should be saved and loaded from the disk.

### Expample of simple terrain using the voxel system:
[<img src="/images/voxel_terrain_simple.png">](/images/voxel_terrain_simple.png) 

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

---


## A voxel

The term voxel means volumentric pixel, this can be any representation of a pixel as long as it has "volume". A simple example being cubes (e.g. Minecraft terrain), every pixel (voxel) has a 3D location and on that location a cube is rendered. 

Since a cube is already a recognisable (and stackable) shape it's trivial to make a surface representation of an 3D shape. The voxel system from this project is sligthy more complex. The pixels (voxels) can have multiple "shapes" (15 unique) and these shapes are defined in the triangulation table.
<br><br><br>
<a title="Jmtrivial / GPL (http://www.gnu.org/licenses/gpl.html)" href="https://commons.wikimedia.org/wiki/File:MarchingCubes.svg"><img width="800" alt="MarchingCubes" src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/MarchingCubes.svg/1024px-MarchingCubes.svg.png"></a>

-The configurations the voxel can take depending on the voxel points


Which shape a voxel has is determined by 8 values. To get these values we sample points from a 3D grid. If we make sure that every point is also used for the next voxel we automatically get a coherent surface. Because of this I will call the collection of 8 pixels a "voxel" and the values (which are technically the voxels) "voxel points".

<a href="/images/linked_voxels.png"><img src="/images/linked_voxels.png" height="300" style="vertical-align:middle;margin:10px 10px"> </a>

-Visualisation of how each (2D) voxel samples it's voxel points.

## Chunks of voxels

To make the terrain generatable while playing I divided the voxels in chunks of values. 

Originally I made a single chunk contain 4^3 voxelPoints but this ended up too low resolution for my liking. You could ofcourse scale it down and have more chunks at the same time but the overhead of keeping track of all of the transforms alone proved to be problematic. So I bumped it up to 8^3 voxelPoints per chunk and I found this to be good enough.

<a href="/images/8x8_chunk_simplex.png"><img src="/images/8x8_chunk_simplex.png" height="400" style="vertical-align:middle;margin:10px 10px"> </a>

-Single 8x8 chunk filled with simplex noise.

## Code

To finally get to it, I have made a function that
1. Loops through all the positions of the chunk.
2. For every position it gets the associated chunkPoints.
3. Looks up which shape the voxel should have (from the triangulation table)
4. Converts the shape to 3D coordinates in the world so we can generate the mesh.
5. Further more the function actually sets the mesh to the GameObject but I've omitted this for now (The function is multithreaded and needs synchronisation to the main thread).



{% highlight csharp %}
private byte[] pointValues = new byte[8];

private void GenerateChunkMesh(object FA_Voxel_Render_LocalPosition)
{

	//Step 1
	for (byte y = 0; y < FA_Voxel_Chunk.CHUNK_UNITS; y+=1)
	{
		for (byte z = 0; z < FA_Voxel_Chunk.CHUNK_UNITS; z+=1)
		{
			for (byte x = 0; x < FA_Voxel_Chunk.CHUNK_UNITS; x+=1)
			{
				byte xP1 = (byte)(x+1);
				byte yP1 = (byte)(y+1);
				byte zP1 = (byte)(z+1);
				//(Step 1)
				
				
				//Step 2
				//remember y-up
				pointValues[0] = getChunkPointOrAdj(x, y, zP1, flooredTransform);
				pointValues[1] = getChunkPointOrAdj(xP1, y, zP1, flooredTransform);
				pointValues[2] = getChunkPointOrAdj(xP1, y, z, flooredTransform);
				pointValues[3] = getChunkPointOrAdj(x, y, z, flooredTransform);
				pointValues[4] = getChunkPointOrAdj(x, yP1, zP1, flooredTransform);
				pointValues[5] = getChunkPointOrAdj(xP1, yP1, zP1, flooredTransform);
				pointValues[6] = getChunkPointOrAdj(xP1, yP1, z, flooredTransform);
				pointValues[7] = getChunkPointOrAdj(x, yP1, z, flooredTransform);
				//(Step 2)


				//Step 3
				int triangulationIndex = calculate_triangle_index(pointValues);
				
				int[] triangles = FA_ChunkSpawner.TRIANGLES[triangulationIndex];
				//(Step 3)


				//Step 4
				int i;
				for(i=0;triangles[i]>=0;i++)
				{
					//For step 5 you should store these vertices in a list or buffer.
					getVoxelVertexPositionAccurate(triangles[i])+new Vector3(x/2f , y/2f , z/2f )
				}
				//(Step 4)
			}
		}
	}
	//Step 5 you should use the vertices to create a mesh object here.
}
{% endhighlight %}
