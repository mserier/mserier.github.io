---
layout: subtitlepost
title: Structure your (open) worlds
subtitle: A few words on different world scenarios and suitable data structures.
categories:
  - blog
  - article
tags:
  - networking
published: true
---

I will be giving some examples of different world structures and how I store my worlds from my game projects. For each structure I will give a quick list of reguirements


## Open World Sandbox

---- 
*Lots of placeable items/objects
*Different parts of the world need to be loaded and unloaded quickly
*Often has dense and sparse parts

I've considered to options:
Pregenerate a complex world and fully sync the devices on all players. This means that connect times are pretty long since the whole world needs to be downloaded to the player but it means that the world isn't limited in complexity and player count. Once the player has an up to date world it can receive all changes and apply this localy.
Another option was to keep the generation algorithm so fast that you only store the placed, changed and removed objects of the world. This means that the world sizes are way smaller but you will probably end up with limitations and it can become quite the hassle to make sure all clients keep synced and the world will become more demanding the more it gets changed.

Since I'm not worried about loading times for experiments and I don't have a fully worked out world generator I will just keep it to the simpler solution.


----
