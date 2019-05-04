---
layout: post
title: Extending your game to the real world.<h4 style="line-height:0px;"><br>(AKA Game State Integration)</h4>
---

You might find yourself in the situation where you want to integrate your game with external services. Custom lighting for your keyboard and mouse, interactive merchandise or even stage effects for a tournament.
To accomplish this successfully you need to

* Get the information out of the game.
* Receive and process the data.

## Getting the information out the game
Spoiler: You are probably going to create a udp server, yourself and not via a high level networking library!
Why? Because what you want is to present your game data without the need for in-order or reliability. And this is exactly what a simple connectionless udp protocol gives you. The server presents the information on a (local ip)+port and any peripheral can just listen on here and use whatever information it’s given. Now of course (and always in the programming world) you may actually in-order or reliability or even need a two way communication. In this case you should use a suitable network protocol.
[insert csgo bomb stage effect example that would rather have a reliable protocol since the amount of data is very low but it is very important]
 
Since it’s easier to create a server when you know what you need to send we will talk about receiving it first by creating a implementation for codemasters UDP Telemetry Output. This will work on the recent racing games like Dirt Rally, F1 2013 and newer.
## Receive and process the data
We will be creating a shift indicator with some random leds and an arduino I have laying around.


