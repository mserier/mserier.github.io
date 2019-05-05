---
layout: subtitlepost
title: "Extending your game to the real world."
subtitle: (AKA Game State Integration)
---

You might find yourself in the situation where you want to integrate your game with external services. Custom lighting for your keyboard and mouse, interactive merchandise or even stage effects for a tournament.
To accomplish this successfully you need to

* Get the information out of the game.
* Receive and process the data.

## Getting the information out the game
Spoiler: You are probably going to create a udp server, yourself and not via a high level networking library!
Why? Because what you want is to present your game data without the need for in-order or reliability. And this is exactly what a simple connectionless udp protocol gives you. The server presents the information on a (local ip)+port and any peripheral can just listen on here and use whatever information it’s given. Now of course (and always in the programming world) you may actually in-order or reliability or even need a two way communication. In this case you should use a suitable network protocol.

<div id="player"></div>
<script type="text/javascript" src="https://www.youtube.com/iframe_api"></script>
<script type="text/javascript">
var player;
function onYouTubeIframeAPIReady() {
    player = new YT.Player('player', {
        //height: '390',
        //width: '640',
      	playerVars: {
          controls: 0,
          showinfo: 0,
          modestbranding: 1,
          loop: 1,
          fs: 0,
          cc_load_policy: 0,
          iv_load_policy: 3,
          autohide: 0
        },
        videoId: 'kgitmggEgrA',
        events: {
            'onReady': onPlayerReady,
            'onStateChange': onPlayerStateChange
        }
    });
}
function onPlayerReady(event) {
    loopStart();
    player.mute();
    player.playVideo();
}
function loopStart() {
    player.seekTo(2552);
}
function onPlayerStateChange(event) {
    if (event.data == YT.PlayerState.PLAYING) {
        setTimeout(loopStart, 12200);
    }
}
</script>
****
Example of a explosion stage effect. The player doesn't have enough time to diffuse the bomb and when it explodes ingame, a explosion effect is played in the arena screen and the bracelets start flashing.
****
 
Since it’s easier to create a server when you know what you need to send we will talk about receiving it first by creating a implementation for codemasters UDP Telemetry Output. This will work on the recent racing games like Dirt Rally, F1 2013 and newer.
## Receive and process the data
We will be creating a shift indicator with some random leds and an arduino I have laying around.


