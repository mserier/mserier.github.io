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


         <p style="color:blue;font-size:18px;">This is demo text
	 
{% highlight csharp %}
	private void GenerateChunkMesh(object FA_Voxel_Render_LocalPosition)
	{
		
		
		generatingMeshVertices.Clear();
		generatingMeshTriangles.Clear();
		mainChunk=null;

		//Floor in steps of FA_Voxel_Chunk.CHUNK_UNITS_HALF and offset to the middle of the maximum range
		Vector3 flooredTransform = new Vector3(Mathf.Floor(((Vector3)FA_Voxel_Render_LocalPosition).x/FA_Voxel_Chunk.CHUNK_UNITS_HALF)*FA_Voxel_Chunk.CHUNK_UNITS_HALF +128  , Mathf.Floor(((Vector3)FA_Voxel_Render_LocalPosition).y/FA_Voxel_Chunk.CHUNK_UNITS_HALF)*FA_Voxel_Chunk.CHUNK_UNITS_HALF  +128 , Mathf.Floor(((Vector3)FA_Voxel_Render_LocalPosition).z/FA_Voxel_Chunk.CHUNK_UNITS_HALF)*FA_Voxel_Chunk.CHUNK_UNITS_HALF  +128 ) ;


		bool meshEmpty = true;


		for (byte y = 0; y < FA_Voxel_Chunk.CHUNK_UNITS; y+=1)
		{
			for (byte z = 0; z < FA_Voxel_Chunk.CHUNK_UNITS; z+=1)
			{
				for (byte x = 0; x < FA_Voxel_Chunk.CHUNK_UNITS; x+=1)
				{
					byte xP1 = (byte)(x+1);
					byte yP1 = (byte)(y+1);
					byte zP1 = (byte)(z+1);

					//remember y-up
					pointValues[0] = getChunkPointOrAdj(x, y, zP1, flooredTransform);
					pointValues[1] = getChunkPointOrAdj(xP1, y, zP1, flooredTransform);
					pointValues[2] = getChunkPointOrAdj(xP1, y, z, flooredTransform);
					pointValues[3] = getChunkPointOrAdj(x, y, z, flooredTransform);
					pointValues[4] = getChunkPointOrAdj(x, yP1, zP1, flooredTransform);
					pointValues[5] = getChunkPointOrAdj(xP1, yP1, zP1, flooredTransform);
					pointValues[6] = getChunkPointOrAdj(xP1, yP1, z, flooredTransform);
					pointValues[7] = getChunkPointOrAdj(x, yP1, z, flooredTransform);


					int triangulationIndex = calculate_triangle_index(pointValues);

					//if(!(triangulationIndex==0 || triangulationIndex==255))
					//if(!(pointValues[0]>=128||pointValues[1]>=128||pointValues[2]>=128||pointValues[3]>=128||pointValues[4]>=128||pointValues[5]>=128||pointValues[6]>=128||pointValues[7]>=128))
					{
						//meshEmpty = false;
					}
					
					int[] triangles = FA_ChunkSpawner.TRIANGLES[triangulationIndex];

					if(tmpDOT!=null)
						Instantiate(tmpDOT, new Vector3(x, y, z), Quaternion.identity);

					int i;
					for(i=0;triangles[i]>=0;i++)
					{
						//use this if the chunks arent exactly aligned
						//generatingMeshVertices.Insert(i, getVoxelVertexPositionAccurate(triangles[i])+new Vector3(x/2f - ((Vector3)FA_Voxel_Render_LocalPosition).x%FA_Voxel_Chunk.CHUNK_UNITS_HALF, y/2f - ((Vector3)FA_Voxel_Render_LocalPosition).y%FA_Voxel_Chunk.CHUNK_UNITS_HALF, z/2f - ((Vector3)FA_Voxel_Render_LocalPosition).z%FA_Voxel_Chunk.CHUNK_UNITS_HALF)  );
						generatingMeshVertices.Insert(i, getVoxelVertexPositionAccurate(triangles[i])+new Vector3(x/2f , y/2f , z/2f )  );

						generatingMeshTriangles.Add(generatingMeshTriangles.Count);//TODO is this dumb?
					}
				}
			}
		}


		//if(!meshEmpty)
			//MainThreadActions.Push( ()=> {var watch = System.Diagnostics.Stopwatch.StartNew(); chunkMesh.Clear(); chunkMesh.vertices = generatingMeshVertices.ToArray(); chunkMesh.triangles = generatingMeshTriangles.ToArray(); meshDone=true; mr.enabled=true; watch.Stop(); Debug.Log("setting verts took :"+watch.ElapsedTicks); } );
			MainThreadActions.Push( ()=> {chunkMesh.Clear(); chunkMesh.vertices = generatingMeshVertices.ToArray(); chunkMesh.triangles = generatingMeshTriangles.ToArray(); meshDone=true; mr.enabled=true;} );
			//Debug.Log("pushed, count is :"+MainThreadActions.Count);
	}
{% endhighlight %}
</p>  
