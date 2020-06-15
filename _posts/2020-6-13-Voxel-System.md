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

### Project Structure
For this project I've choosen the Universal Project Template. This template uses the Universal Rendering Pipeline (URP) which formerly was know as Lightweight Render Pipeline.

In the scene hierarchy are the following relevant objects

**[FA_World]**
- Self spawning (as noted with the brackets) through a singleton .

**Chunks**
- Has the Chunk Manager (script) component which will spawn chunk objects as children.

