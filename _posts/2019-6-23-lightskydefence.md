---
layout: post
title: LightSkyDefence - VR tower defence!
categories: [blog, project]
tags: [unity, vr, school]
published: true
---

[Motivation]{dir="ltr"}
=======================

[This document will guide you through the technical aspects of our game
Light Sky Defense. It will explain our project structure, development
process and elaborate code.]{dir="ltr"}

[]{dir="ltr"}

**[What is Light Sky Defense?]{dir="ltr"}**

[Light Sky Defense is a VR tower defense experience where enemies follow
a 3D path and try to reach the end. As the player, you can place towers
alongside the path that will hinder and destroy the incoming waves of
enemies.]{dir="ltr"}

[]{dir="ltr"}

[Table of contents]{dir="ltr"}
==============================

[**[Motivation](#motivation) 1**]{dir="ltr"}

[**[Table of contents](#table-of-contents) 2**]{dir="ltr"}

[**[Architecture](#architecture) 5**]{dir="ltr"}

[**[Physics](#physics) 6**]{dir="ltr"}

> [[Collision matrix](#collision-matrix) 6]{dir="ltr"}

[**[Messages](#messages) 8**]{dir="ltr"}

> [[Global messages](#global-messages) 8]{dir="ltr"}
>
> [[Local messages](#local-messages) 8]{dir="ltr"}

[**[GameManager (Script)](#gamemanager-script) 10**]{dir="ltr"}

> [[State management](#state-management) 10]{dir="ltr"}
>
> [[GameStates](#gamestates) 11]{dir="ltr"}
>
> [[WavesState](#wavesstate) 13]{dir="ltr"}
>
> [[WavesEndState](#wavesendstate) 13]{dir="ltr"}
>
> [[InfinityState](#infinitystate) 13]{dir="ltr"}
>
> [[LoseState](#losestate) 13]{dir="ltr"}
>
> [[WinState](#winstate) 13]{dir="ltr"}

[**[Path](#path) 14**]{dir="ltr"}

> [[Calculation](#calculation) 14]{dir="ltr"}
>
> [[Visualisation](#visualisation) 15]{dir="ltr"}
>
> [[Path (Script)](#path-script) 15]{dir="ltr"}

[**[Common components](#common-components) 20**]{dir="ltr"}

> [[Damageable (Script)](#damageable-script) 20]{dir="ltr"}
>
> [[Repairable (Script)](#repairable-script) 21]{dir="ltr"}
>
> [[PlacementCost (Script)](#placementcost-script) 22]{dir="ltr"}
>
> [[SpawnCreditOnDie (Script)](#spawncreditondie-script) 22]{dir="ltr"}
>
> [[RenderableColliders (Script)](#renderablecolliders-script)
> 22]{dir="ltr"}
>
> [[WorldPlaceable (Script)](#worldplaceable-script) 24]{dir="ltr"}

[**[Scoreboard](#scoreboard) 26**]{dir="ltr"}

> [[Statistics](#statistics) 26]{dir="ltr"}

[**[Towers](#towers) 28**]{dir="ltr"}

> [[BaseTower (Prefab)](#basetower-prefab) 28]{dir="ltr"}
>
> [[Components](#components) 28]{dir="ltr"}
>
> [[Tower states](#tower-states) 29]{dir="ltr"}
>
> [[Health Bar](#health-bar) 30]{dir="ltr"}
>
> [[BaseTower (Script)](#basetower-script) 31]{dir="ltr"}
>
> [[OnDie](#ondie) 32]{dir="ltr"}
>
> [[OnDelete](#ondelete) 32]{dir="ltr"}
>
> [[Active and Idle state](#active-and-idle-state) 33]{dir="ltr"}
>
> [[Jam state](#jam-state) 33]{dir="ltr"}
>
> [[Win and lose state](#win-and-lose-state) 34]{dir="ltr"}
>
> [[IdleRotationState (Script)](#idlerotationstate-script)
> 34]{dir="ltr"}
>
> [[JamState (Script)](#jamstate-script) 35]{dir="ltr"}
>
> [[Machine gun](#machine-gun) 36]{dir="ltr"}
>
> [[Components](#components-1) 36]{dir="ltr"}
>
> [[ShootBulletState (Script)](#shootbulletstate-script) 36]{dir="ltr"}
>
> [[Rotation](#rotation) 37]{dir="ltr"}
>
> [[Shooting](#shooting) 38]{dir="ltr"}
>
> [[Shotgun](#shotgun) 40]{dir="ltr"}
>
> [[ShotgunState (Script)](#shotgunstate-script) 40]{dir="ltr"}
>
> [[Shooting multiple bullets](#shooting-multiple-bullets)
> 41]{dir="ltr"}
>
> [[Missile launcher](#missile-launcher) 42]{dir="ltr"}
>
> [[ShootMissileState (Script)](#shootmissilestate-script)
> 42]{dir="ltr"}

[**[Projectiles](#projectiles) 45**]{dir="ltr"}

> [[EnemyBullet](#enemybullet) 45]{dir="ltr"}
>
> [[EnemyJammerBullet](#enemyjammerbullet) 46]{dir="ltr"}
>
> [[Missile (Prefab)](#missile-prefab) 47]{dir="ltr"}
>
> [[Homing](#homing) 48]{dir="ltr"}
>
> [[Explosion](#explosion) 49]{dir="ltr"}

[**[Enemy](#enemy) 50**]{dir="ltr"}

> [[Enemy (Script)](#enemy-script) 51]{dir="ltr"}
>
> [[Rigidbody Steering (Script)](#rigidbody-steering-script)
> 51]{dir="ltr"}
>
> [[FollowPath (Script)](#followpath-script) 54]{dir="ltr"}
>
> [[Flocking (Script)](#flocking-script) 55]{dir="ltr"}
>
> [[Wander (Script)](#wander-script) 58]{dir="ltr"}
>
> [[Enemy abilities](#enemy-abilities) 60]{dir="ltr"}
>
> [[EnemyAoEHeal(Script)](#enemyaoehealscript) 61]{dir="ltr"}
>
> [[EnemySelfHeal (Script)](#enemyselfheal-script) 62]{dir="ltr"}
>
> [[EnemySplit (Script)](#enemysplit-script) 63]{dir="ltr"}
>
> [[FindTarget (Script)](#findtarget-script) 63]{dir="ltr"}
>
> [[ShootAtTarget (Script)](#shootattarget-script) 64]{dir="ltr"}
>
> [[ShootMissileAtTarget (Script)](#shootmissileattarget-script)
> 66]{dir="ltr"}

[**[Controls](#controls) 68**]{dir="ltr"}

> [[SteamVR Input System](#steamvr-input-system) 68]{dir="ltr"}
>
> [[Actions](#actions) 69]{dir="ltr"}
>
> [[Using an action](#using-an-action) 70]{dir="ltr"}
>
> [[SteamVR Interaction System](#steamvr-interaction-system)
> 72]{dir="ltr"}
>
> [[Player (Prefab)](#player-prefab) 72]{dir="ltr"}
>
> [[Interactable (Script)](#interactable-script) 73]{dir="ltr"}
>
> [[Hand (Prefab)](#hand-prefab) 73]{dir="ltr"}
>
> [[Haptic feedback](#haptic-feedback) 74]{dir="ltr"}
>
> [[Button hints (Prefab)](#button-hints-prefab) 74]{dir="ltr"}
>
> [[Dial (Script)](#dial-script) 75]{dir="ltr"}
>
> [[SpawnDialOption (Script)](#spawndialoption-script) 80]{dir="ltr"}

[**[Wave System](#wave-system) 84**]{dir="ltr"}

> [[Wave step](#wave-step) 84]{dir="ltr"}
>
> [[Creating a wave](#creating-a-wave) 85]{dir="ltr"}

[**[Graphics](#graphics) 88**]{dir="ltr"}

> [[Post processing](#post-processing) 88]{dir="ltr"}
>
> [[Intersect Shader](#intersect-shader) 88]{dir="ltr"}

[]{dir="ltr"}

[Architecture]{dir="ltr"}
=========================

[The top level directory structure of our project is to separate types
of files into different directories. So directly below the
LightSkyDefense directory we have script, prefab, model and more
directories. Everything in a deeper level is grouped into logically
related files. So under the prefab directory we have a tower and enemy
directory.]{dir="ltr"}

[]{dir="ltr"}

[Since we use Unity we are also using the Unity architecture. Roughly
speaking this splits the type of objects into two: MonoBehaviour (also
called scripts) and GameObjects. In this split GameObjects are the
objects that exist on the scene, while scripts are objects that can be
attached to GameObjects to determine behaviour.]{dir="ltr"}

[]{dir="ltr"}

[![](media/image16.png){width="6.026042213473316in"
height="3.8956233595800525in"}](https://www.draw.io/?page-id=9mDlmz31va8omdzqz6yL&scale=auto#G1WPOe0bq8OltblvDXPd7ISROP4uWW9644)[]{dir="ltr"}

[]{dir="ltr"}

[Unity scripts can be created in several languages like: C\#,
UnityScript (Javascript for Unity). We've decided to use only use C\#
since everyone has a lot of experience in C\#.]{dir="ltr"}

[]{dir="ltr"}

[There are some singleton scripts: GameManager, Player and Path,
attached to the GameManager, Player and Path GameObjects. These scripts
should not be attached to any other GameObject since this break the
singleton guarantee.]{dir="ltr"}

[]{dir="ltr"}

[Physics]{dir="ltr"}
====================

[Game objects in our game collide with other game objects. However, not
all objects should collide with each other.]{dir="ltr"}

[Collision matrix]{dir="ltr"}
-----------------------------

[Unity has [[Layer-based collision
detection]{.underline}](https://docs.unity3d.com/Manual/LayerBasedCollision.html),
this allows us to specify the layers that collide with each
other.]{dir="ltr"}

[]{dir="ltr"}

[We use nine
[[layers]{.underline}](https://docs.unity3d.com/Manual/class-TagManager.html)
to group our game objects. Of these nine layers one is built-in, the
other eight are custom layers, the built-in layer(s) are marked with a
\*.]{dir="ltr"}

[]{dir="ltr"}

[You can create new layers in the project settings. Go to "Edit" \>
"Project settings" \> "Tags and layers" to create new layers. Once
you've created your layers, go to "Edit" \> "Project settings" \>
"Physics" to configure the collision matrix.]{dir="ltr"}

[]{dir="ltr"}

[The following table represents our collision matrix:]{dir="ltr"}

[]{dir="ltr"}

  []{dir="ltr"}                       [**Towers**]{dir="ltr"}   [**Enemies**]{dir="ltr"}   [**Path**]{dir="ltr"}   [**Controls**]{dir="ltr"}   [**Projectiles**]{dir="ltr"}   [**UI\***]{dir="ltr"}   [**Pickups**]{dir="ltr"}   [**Enemy ability**]{dir="ltr"}   [**Enemy projectile**]{dir="ltr"}
  ----------------------------------- ------------------------- -------------------------- ----------------------- --------------------------- ------------------------------ ----------------------- -------------------------- -------------------------------- -----------------------------------
  [**Towers**]{dir="ltr"}             [**x**]{dir="ltr"}        [**x**]{dir="ltr"}         [**x**]{dir="ltr"}      [**x**]{dir="ltr"}          []{dir="ltr"}                  []{dir="ltr"}           []{dir="ltr"}              [**x**]{dir="ltr"}               [**x**]{dir="ltr"}
  [**Enemies**]{dir="ltr"}            [**x**]{dir="ltr"}        [**x**]{dir="ltr"}         [**x**]{dir="ltr"}      []{dir="ltr"}               [**x**]{dir="ltr"}             []{dir="ltr"}           []{dir="ltr"}              [**x**]{dir="ltr"}               []{dir="ltr"}
  [**Path**]{dir="ltr"}               [**x**]{dir="ltr"}        [**x**]{dir="ltr"}         []{dir="ltr"}           [**x**]{dir="ltr"}          []{dir="ltr"}                  [**x**]{dir="ltr"}      []{dir="ltr"}              []{dir="ltr"}                    []{dir="ltr"}
  [**Controls**]{dir="ltr"}           [**x**]{dir="ltr"}        []{dir="ltr"}              [**x**]{dir="ltr"}      []{dir="ltr"}               []{dir="ltr"}                  [**x**]{dir="ltr"}      [**x**]{dir="ltr"}         []{dir="ltr"}                    []{dir="ltr"}
  [**Projectiles**]{dir="ltr"}        []{dir="ltr"}             [**x**]{dir="ltr"}         []{dir="ltr"}           []{dir="ltr"}               []{dir="ltr"}                  []{dir="ltr"}           []{dir="ltr"}              []{dir="ltr"}                    [**x**]{dir="ltr"}
  [**UI\***]{dir="ltr"}               []{dir="ltr"}             []{dir="ltr"}              [**x**]{dir="ltr"}      [**x**]{dir="ltr"}          []{dir="ltr"}                  [**x**]{dir="ltr"}      []{dir="ltr"}              []{dir="ltr"}                    []{dir="ltr"}
  [**Pickups**]{dir="ltr"}            []{dir="ltr"}             []{dir="ltr"}              []{dir="ltr"}           [**x**]{dir="ltr"}          []{dir="ltr"}                  []{dir="ltr"}           []{dir="ltr"}              []{dir="ltr"}                    []{dir="ltr"}
  [**Enemy ability**]{dir="ltr"}      [**x**]{dir="ltr"}        [**x**]{dir="ltr"}         []{dir="ltr"}           []{dir="ltr"}               []{dir="ltr"}                  []{dir="ltr"}           []{dir="ltr"}              []{dir="ltr"}                    []{dir="ltr"}
  [**Enemy projectile**]{dir="ltr"}   [**x**]{dir="ltr"}        []{dir="ltr"}              []{dir="ltr"}           []{dir="ltr"}               [**x**]{dir="ltr"}             []{dir="ltr"}           []{dir="ltr"}              []{dir="ltr"}                    []{dir="ltr"}

[]{dir="ltr"}

[The configured collision matrix in Unity will look like
this:]{dir="ltr"}

[]{dir="ltr"}

![](media/image14.png){width="3.34375in" height="3.3125in"}\
[]{dir="ltr"}

[Messages]{dir="ltr"}
=====================

[Messages are used to broadcast events objects in the scene. This is
useful in many scenarios where, the player wins or loses, the player
pauses or resumes the game, and when an enemy or tower is
destroyed.]{dir="ltr"}

[Global messages]{dir="ltr"}
----------------------------

[Creating your own global message and broadcasting it to all entities is
very easy. You need to find all active game objects and then invoke the
SendMessage method them.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [foreach (var gameObject in                                           |
| FindObjectsOfType\<GameObject\>())]{dir="ltr"}                        |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ gameObject.SendMessage(]{dir="ltr"}                                 |
|                                                                       |
| [\"MyGlobalMessage\",]{dir="ltr"}                                     |
|                                                                       |
| [ SendMessageOptions.DontRequireReceiver]{dir="ltr"}                  |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[The following global messages are implemented the game.]{dir="ltr"}

[]{dir="ltr"}

  **[Event name]{dir="ltr"}**   **[Invoked by]{dir="ltr"}**
  ----------------------------- ---------------------------------
  [OnPauseGame]{dir="ltr"}      [PauseMenuBehaviour]{dir="ltr"}
  [OnResumeGame]{dir="ltr"}     [PauseMenuBehaviour]{dir="ltr"}
  [OnWinGame]{dir="ltr"}        [GameManager]{dir="ltr"}
  [OnLoseGame]{dir="ltr"}       [PlayerStatistics]{dir="ltr"}
  [OnWaveStart]{dir="ltr"}      [Wave]{dir="ltr"}

[Local messages]{dir="ltr"}
---------------------------

[We can broadcast a message to all MonoBehaviours of a GameObject. This
allows us to make very small and reusable scripts for our game
objects.The following code sample shows you how to broadcast a message
to a component's gameObject.]{dir="ltr"}

[]{dir="ltr"}

+------------------------------------------------------+
| [gameObject.BroadcastMessage(]{dir="ltr"}            |
|                                                      |
| [ \"MyLocalMessage\",]{dir="ltr"}                    |
|                                                      |
| [ argumentValue,]{dir="ltr"}                         |
|                                                      |
| [ SendMessageOptions.DontRequireReceiver]{dir="ltr"} |
|                                                      |
| [);]{dir="ltr"}                                      |
+------------------------------------------------------+

[]{dir="ltr"}

[Unity will try to invoke the "MyLocalMessage" function on all scripts
that are added to the gameObject.]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [class MyScript : MonoBehaviour {]{dir="ltr"}                         |
|                                                                       |
| [\...]{dir="ltr"}                                                     |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Invoked when the "MyLocalMessage" message is received by this    |
| script]{dir="ltr"}                                                    |
|                                                                       |
| [ void MyLocalMessage(float healthChange) {]{dir="ltr"}               |
|                                                                       |
| [ Debug.Log(\$"Health changed by: {healthChange");]{dir="ltr"}        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [\...]{dir="ltr"}                                                     |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[Local messages are described in the component that emit
them.]{dir="ltr"}

[]{dir="ltr"}

[GameManager (Script)]{dir="ltr"}
=================================

[The GameManager is implemented through a state machine.]{dir="ltr"}

[State management]{dir="ltr"}
-----------------------------

[In order to fit the state machine into the unity mold we had to rethink
and tweak it a little. Rather than a start and end method we simply do
setup in OnEnable and cleanup in OnDisable. Any actual behaviour of the
state can be done in the Update method or a Coroutine.]{dir="ltr"}

[]{dir="ltr"}

[The GameManager script has two important properties for state
management. InitialState and LoseState. In our game Initial state will
be either WavesState or InifiniteState, while the LoseState property is
set to an instance of the LoseState script.]{dir="ltr"}

[]{dir="ltr"}

[The first thing the GameManager does is take all the GameState scripts
that are put on the GameManager and disable them. Then it enables only
the InitialState property of the GameManager.]{dir="ltr"}

[]{dir="ltr"}

+----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                         |
|                                                                      |
| [/// Initialize the game state]{dir="ltr"}                           |
|                                                                      |
| [/// \</summary\>]{dir="ltr"}                                        |
|                                                                      |
| [public void Awake()]{dir="ltr"}                                     |
|                                                                      |
| [{]{dir="ltr"}                                                       |
|                                                                      |
| [ // Save initial state reference to current state field]{dir="ltr"} |
|                                                                      |
| [ CurrentState = InitialState;]{dir="ltr"}                           |
|                                                                      |
| [ GameStates = GetComponents\<GameState\>();]{dir="ltr"}             |
|                                                                      |
| []{dir="ltr"}                                                        |
|                                                                      |
| [ // Disable all states and enable the current state]{dir="ltr"}     |
|                                                                      |
| [ foreach (var state in GameStates)]{dir="ltr"}                      |
|                                                                      |
| [{]{dir="ltr"}                                                       |
|                                                                      |
| [ state.enabled = false;]{dir="ltr"}                                 |
|                                                                      |
| [}]{dir="ltr"}                                                       |
|                                                                      |
| []{dir="ltr"}                                                        |
|                                                                      |
| [ CurrentState.enabled = true;]{dir="ltr"}                           |
|                                                                      |
| [}]{dir="ltr"}                                                       |
+----------------------------------------------------------------------+

[]{dir="ltr"}

[Unity engine, rather than the game manager, will drive the states. So
in order to switch states you need to disable the current state and
enable the one you want to switch to. This will make Unity call OnEnable
and OnDisable on the scripts. The GameState class has a function to do
this.]{dir="ltr"}

[]{dir="ltr"}

+----------------------------------------------------------------------+
| [public void SetGameState(GameState gameState)]{dir="ltr"}           |
|                                                                      |
| [{]{dir="ltr"}                                                       |
|                                                                      |
| [ if (gameState == null)]{dir="ltr"}                                 |
|                                                                      |
| [{]{dir="ltr"}                                                       |
|                                                                      |
| [ return;]{dir="ltr"}                                                |
|                                                                      |
| [}]{dir="ltr"}                                                       |
|                                                                      |
| []{dir="ltr"}                                                        |
|                                                                      |
| [ foreach (var state in GameManager.Instance.GameStates)]{dir="ltr"} |
|                                                                      |
| [{]{dir="ltr"}                                                       |
|                                                                      |
| [ state.enabled = false;]{dir="ltr"}                                 |
|                                                                      |
| [}]{dir="ltr"}                                                       |
|                                                                      |
| []{dir="ltr"}                                                        |
|                                                                      |
| [ gameState.enabled = true;]{dir="ltr"}                              |
|                                                                      |
| [ GameManager.Instance.CurrentState = gameState;]{dir="ltr"}         |
|                                                                      |
| [}]{dir="ltr"}                                                       |
+----------------------------------------------------------------------+

[]{dir="ltr"}

[GameStates]{dir="ltr"}
-----------------------

[The player can start the game in two different modes: Normal and
Infinity mode. Normal mode provides the player with a set amount of
waves, meaning beating the last wave results in winning the game.
Infinity mode has no set amount of waves and can therefore not be won.
In both modes the player will lose if their life counter hits
zero.]{dir="ltr"}

[]{dir="ltr"}

[This description results in the following state diagram:]{dir="ltr"}

[]{dir="ltr"}

[![](media/image12.png){width="6.255208880139983in"
height="1.7543821084864393in"}](https://www.draw.io/?page-id=m0tgIQ1W0dBQ3J8xYi6Y&scale=auto#G1velzY7Amu8P-YiY2fmegRSkw_uu94nLb)[]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

[The condition to trigger each State, as well as a description of each
state can be seen in the table below.]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

  **[State]{dir="ltr"}**       **[Condition]{dir="ltr"}**                                           **[Description]{dir="ltr"}**
  ---------------------------- -------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------
  [WavesState]{dir="ltr"}      [When the player selects Normal mode.]{dir="ltr"}                    [The state the game is in if the final wave has not been reached yet during Normal mode and the player has lives left.]{dir="ltr"}
  [InfinityState]{dir="ltr"}   [When the player selects Infinity mode.]{dir="ltr"}                  [The state the game is in if the player has selected Infinity mode and has lives left in this mode.]{dir="ltr"}
  [WavesEndState]{dir="ltr"}   [When the last wave has spawned during the WavesState.]{dir="ltr"}   [The state the game is in if the final wave has been reached during Normal mode and the player has lives left.]{dir="ltr"}
  [LoseState]{dir="ltr"}       [When the player reaches zero lives.]{dir="ltr"}                     [The state the game is in after a player loses all their lives. It will show a "Game Over" text in the player's view.]{dir="ltr"}
  [WinState]{dir="ltr"}        [When the player beats the final wave in Normal mode.]{dir="ltr"}    [The state the game is in after beating the last wave in Normal mode. It will show a "Victory" text in the player's view.]{dir="ltr"}

[]{dir="ltr"}

[A class diagram of all the states can be seen below.]{dir="ltr"}

[]{dir="ltr"}

[![](media/image23.png){width="5.182292213473316in"
height="4.830779746281714in"}](https://www.draw.io/?page-id=gCRMZIth-i-edPDIHTYn&scale=auto#G1velzY7Amu8P-YiY2fmegRSkw_uu94nLb)[]{dir="ltr"}

### [WavesState]{dir="ltr"}

[This State has two properties: Waves and WavesEndState. Waves is an
array of Wave objects. WavesEndState is a GameState object. When this
state is enabled it will start a coroutine executing the Start() method
of each Wave. When it has finished spawning each wave it will set the
GameState to the WavesEndState Value. When this state is disabled it
will stop the coroutine]{dir="ltr"}

[]{dir="ltr"}

### [WavesEndState]{dir="ltr"}

[This state has two property: CheckInterval, which by default is set to
1 and WinState. When this state is enabled it will start a coroutine to
check, every CheckInterval seconds, if all the enemies are dead. If they
all are it will set the GameState to the WinState. OnDisable it will
stop this coroutine]{dir="ltr"}

[]{dir="ltr"}

### [InfinityState]{dir="ltr"}

[This state has an Enemy and SpawnInterval property, on enable it will
start a coroutine to spawn the enemy object every SpawnInterval.
OnDisable it will stop this coroutine.]{dir="ltr"}

[]{dir="ltr"}

### [LoseState]{dir="ltr"}

[When this state is enabled it will spawn a game over text in front of
the player. This game over text is the value of the prefab
property.]{dir="ltr"}

### [WinState]{dir="ltr"}

[When this state is enabled it will broadcast the onGameWin event to
each GameObject. After that it will spawn a massive game win text in
front of the player. This game win text is the value of the prefab
property.]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

[Path]{dir="ltr"}
=================

[Every level has one path that indicates the route of the enemies.
Enemies will follow this path to regenerate energy while trying to reach
the end. The path is drawn between the edges of the VR play
area.]{dir="ltr"}

[]{dir="ltr"}

![](media/image26.png){width="6.270833333333333in"
height="4.013888888888889in"}[]{dir="ltr"}

[]{dir="ltr"}

[The following components are added to the path game object.]{dir="ltr"}

[]{dir="ltr"}

  **[Component]{dir="ltr"}**   **[Type]{dir="ltr"}**       **[What is it used for]{dir="ltr"}**
  ---------------------------- --------------------------- ---------------------------------------------------------------
  [Path]{dir="ltr"}            [Script]{dir="ltr"}         [Used to generate the points that are on the path]{dir="ltr"}
  [LineRenderer]{dir="ltr"}    [LineRenderer]{dir="ltr"}   [Used to visualize the path]{dir="ltr"}

[Calculation]{dir="ltr"}
------------------------

[The path exists of multiple curves so is can take different shapes. One
curve is a cubic Bezier curve, based on Casteljau's algorithm. Each
curve has two handles, see the image below for an
impression.]{dir="ltr"}

![](media/image11.png){width="3.0297856517935258in"
height="1.9010422134733158in"}[]{dir="ltr"}

[]{dir="ltr"}

[P0 indicates the start and P3 the end. P1 and P2 will 'pull' the line
which makes the line bend.]{dir="ltr"}

[Visualisation]{dir="ltr"}
--------------------------

[We want a smooth path which looks realistic. Sharp angles in a path
don't look realistic. When combining multiple curves, we need to adjust
the handles to each other to make a smooth path.]{dir="ltr"}

[]{dir="ltr"}

[The path is visualized using the LineRenderer, the LineRenderer draws a
2D line along the points in our path. We can change the shape of the
line in our LineRenderer. The material that is applied to the
LineRenderer is used to fill in the shape of the line.]{dir="ltr"}

[Path (Script)]{dir="ltr"}
--------------------------

[The path script is used to generate points from the configured
parameters.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------+-----------------------------------+
| **[Property]{dir="ltr"}**         | **[Description]{dir="ltr"}**      |
+===================================+===================================+
| [Line segments]{dir="ltr"}        | [The amount of points that will   |
|                                   | be generated]{dir="ltr"}          |
+-----------------------------------+-----------------------------------+
| [Curves]{dir="ltr"}               | [A list of curves that represent  |
|                                   | the full path.]{dir="ltr"}        |
|                                   |                                   |
|                                   | []{dir="ltr"}                     |
|                                   |                                   |
|                                   | [Every curve has 4 Vector3        |
|                                   | parameters:]{dir="ltr"}           |
|                                   |                                   |
|                                   | []{dir="ltr"}                     |
|                                   |                                   |
|                                   | -   [Start]{dir="ltr"}            |
|                                   |                                   |
|                                   | -   [Start tangent]{dir="ltr"}    |
|                                   |                                   |
|                                   | -   [End tangent]{dir="ltr"}      |
|                                   |                                   |
|                                   | -   [End]{dir="ltr"}              |
+-----------------------------------+-----------------------------------+

[]{dir="ltr"}

[The function CreateWaypoints instantiates path points based on the
WaypointCount value. The higher the value, the more precise the path
becomes. We loop over every curve with a stepSize based on the amount of
curves there are. With the increasing stepSize and linear interpolation
we calculate the position and instantiate a waypoint. Every waypoint
gets a heading to the next waypoint.]{dir="ltr"}

[]{dir="ltr"}

![](media/image32.png){width="5.55632874015748in"
height="3.4427088801399823in"}[]{dir="ltr"}

[]{dir="ltr"}

[The following code fragment shows the implementation of a simplified
version of the casteljau algorithm.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private void CreateWaypoints()]{dir="ltr"}                           |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ \_wayPoints = new GameObject\[WaypointCount\];]{dir="ltr"}          |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // The step size between each waypoint]{dir="ltr"}                  |
|                                                                       |
| [ // Note: this must be a double because the float type precision     |
| isn\'t]{dir="ltr"}                                                    |
|                                                                       |
| [precise enough]{dir="ltr"}                                           |
|                                                                       |
| [ var stepSize = 1d / (WaypointCount / Curves.Length);]{dir="ltr"}    |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Current waypoint index]{dir="ltr"}                               |
|                                                                       |
| [ var waypointIndex = 0;]{dir="ltr"}                                  |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Loop through each curve to create a full path]{dir="ltr"}        |
|                                                                       |
| [ foreach (var curve in Curves)]{dir="ltr"}                           |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // Calculate points using the \'le castelle\' algorithm]{dir="ltr"} |
|                                                                       |
| [ // Note: this must be a double because the float type precision     |
| isn\'t]{dir="ltr"}                                                    |
|                                                                       |
| [precise enough]{dir="ltr"}                                           |
|                                                                       |
| [ for (var interval = 0d; interval \<= 1.0d; interval +=              |
| stepSize)]{dir="ltr"}                                                 |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var floatInterval = (float)interval;]{dir="ltr"}                    |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var ap1 = Vector3.Lerp(]{dir="ltr"}                                 |
|                                                                       |
| [curve.Start,]{dir="ltr"}                                             |
|                                                                       |
| [curve.StartTangent,]{dir="ltr"}                                      |
|                                                                       |
| [floatInterval]{dir="ltr"}                                            |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var ap2 = Vector3.Lerp(]{dir="ltr"}                                 |
|                                                                       |
| [curve.StartTangent,]{dir="ltr"}                                      |
|                                                                       |
| [curve.EndTangent,]{dir="ltr"}                                        |
|                                                                       |
| [floatInterval]{dir="ltr"}                                            |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [ ]{dir="ltr"}                                                        |
|                                                                       |
| [var ap3 = Vector3.Lerp(]{dir="ltr"}                                  |
|                                                                       |
| [curve.EndTangent,]{dir="ltr"}                                        |
|                                                                       |
| [curve.End,]{dir="ltr"}                                               |
|                                                                       |
| [floatInterval]{dir="ltr"}                                            |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var bp1 = Vector3.Lerp(ap1, ap2, floatInterval);]{dir="ltr"}        |
|                                                                       |
| [ var bp2 = Vector3.Lerp(ap2, ap3, floatInterval);]{dir="ltr"}        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var localPosition = Vector3.Lerp(bp1, bp2,                          |
| floatInterval);]{dir="ltr"}                                           |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Create new points from intermediate values]{dir="ltr"}           |
|                                                                       |
| [ \_wayPoints\[waypointIndex\] = Instantiate(]{dir="ltr"}             |
|                                                                       |
| [ Prefab,]{dir="ltr"}                                                 |
|                                                                       |
| [ transform.position + localPosition,]{dir="ltr"}                     |
|                                                                       |
| [ Quaternion.identity,]{dir="ltr"}                                    |
|                                                                       |
| [ transform]{dir="ltr"}                                               |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // We can\'t set the direction to the current waypoint]{dir="ltr"}  |
|                                                                       |
| [// when we\'re creating the first waypoint]{dir="ltr"}               |
|                                                                       |
| [ if (waypointIndex == 0)]{dir="ltr"}                                 |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ waypointIndex++;]{dir="ltr"}                                        |
|                                                                       |
| [ continue;]{dir="ltr"}                                               |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var currentWaypoint = \_wayPoints\[waypointIndex\];]{dir="ltr"}     |
|                                                                       |
| [ var previousWaypoint = \_wayPoints\[waypointIndex -                 |
| 1\];]{dir="ltr"}                                                      |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Update rotation of previous waypoint]{dir="ltr"}                 |
|                                                                       |
| [// to make it look towards the current one]{dir="ltr"}               |
|                                                                       |
| [                                                                     |
| previousWaypoint.transform.LookAt(currentWaypoint.transform);]{dir="l |
| tr"}                                                                  |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Set the rotation of the current waypoint equal to]{dir="ltr"}    |
|                                                                       |
| [// the previous waypoint to make sure that the last]{dir="ltr"}      |
|                                                                       |
| [ // point in the path looks towards a similar direction]{dir="ltr"}  |
|                                                                       |
| [ currentWaypoint.transform.rotation =]{dir="ltr"}                    |
|                                                                       |
| [previousWaypoint.transform.rotation;]{dir="ltr"}                     |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Increment waypoint counter]{dir="ltr"}                           |
|                                                                       |
| [ waypointIndex++;]{dir="ltr"}                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[We don't want players to be able to place towers in the Path, we need
colliders along the path to implement this behaviour. After we set a
waypoints rotation we create capsule colliders that span the distance
between two waypoints.]{dir="ltr"}

[]{dir="ltr"}

![](media/image29.png){width="5.461963035870516in"
height="3.4114588801399823in"}[]{dir="ltr"}

[]{dir="ltr"}

[A collider's position will be the center of a GameObject by default. We
want our collider to be between two waypoints we can set the local
position of a collider by changing the center property.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [var capsuleCollider =                                                |
| currentWaypoint.AddComponent\<CapsuleCollider\>();]{dir="ltr"}        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [var distanceBetweenWaypoints = Vector3.Distance(]{dir="ltr"}         |
|                                                                       |
| [ currentWaypoint.transform.position,]{dir="ltr"}                     |
|                                                                       |
| [ previousWaypoint.transform.position]{dir="ltr"}                     |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Padding is pathWidth multiplied by 2 to make sure]{dir="ltr"}     |
|                                                                       |
| [// that colliders fully overlap without any gaps]{dir="ltr"}         |
|                                                                       |
| [var padding = PathWidth \* 2;]{dir="ltr"}                            |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// The center (local position) of the path collider]{dir="ltr"}      |
|                                                                       |
| [// should be the distance between two waypoints]{dir="ltr"}          |
|                                                                       |
| [capsuleCollider.center = new Vector3(]{dir="ltr"}                    |
|                                                                       |
| [ 0,]{dir="ltr"}                                                      |
|                                                                       |
| [ 0,]{dir="ltr"}                                                      |
|                                                                       |
| [-(distanceBetweenWaypoints / 2)]{dir="ltr"}                          |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Set the dimensions of the path]{dir="ltr"}                        |
|                                                                       |
| [capsuleCollider.height = distanceBetweenWaypoints +                  |
| padding;]{dir="ltr"}                                                  |
|                                                                       |
| [capsuleCollider.radius = PathWidth;]{dir="ltr"}                      |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Capsule collider height should be applied to the Z                |
| axis]{dir="ltr"}                                                      |
|                                                                       |
| [capsuleCollider.direction = 2; // 2 == Z-axis]{dir="ltr"}            |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[Common components]{dir="ltr"}
==============================

[Below are multiple components that are not bound to specific objects
(enemies, towers, etc.) and can be used for a wide variety of
objects.]{dir="ltr"}

[Damageable (Script)]{dir="ltr"}
--------------------------------

[Added to GameObjects that should have health. The following properties
can be configured when the Damageable script is added to a
GameObject.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**    **[Type]{dir="ltr"}**   **[Default value]{dir="ltr"}**
  ---------------------------- ----------------------- --------------------------------
  [MaxHealth]{dir="ltr"}       [float]{dir="ltr"}      [1]{dir="ltr"}
  [InitialHealth]{dir="ltr"}   [float]{dir="ltr"}      [1]{dir="ltr"}

[]{dir="ltr"}

[The following messages are invoked by the Damagable script.]{dir="ltr"}

[]{dir="ltr"}

  **[Message name]{dir="ltr"}**   **[Invoked when]{dir="ltr"}**          **[Arguments]{dir="ltr"}**
  ------------------------------- -------------------------------------- -------------------------------------
  [OnUpdateHealth]{dir="ltr"}     [Health changes]{dir="ltr"}            [Float - Health change.]{dir="ltr"}
  [OnDie]{dir="ltr"}              [Health falls below zero]{dir="ltr"}   []{dir="ltr"}

[]{dir="ltr"}

[The Damageable scripts are added to GameObjects that should "die" when
their health falls below zero. The "OnUpdateHealth" message is
broadcasted to all children of the gameObject whenever this function is
invoked. If health falls below zero the "OnDie" message is broadcasted
to all children of the gameObject]{dir="ltr"}

[]{dir="ltr"}

+----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                         |
|                                                                      |
| [/// Decrease or increase health until health is below 0]{dir="ltr"} |
|                                                                      |
| [/// \</summary\>]{dir="ltr"}                                        |
|                                                                      |
| [/// \<param name=\"amount\"\>\</param\>]{dir="ltr"}                 |
|                                                                      |
| [public void UpdateHealth(float amount)]{dir="ltr"}                  |
|                                                                      |
| [{]{dir="ltr"}                                                       |
|                                                                      |
| [ Health = Mathf.Min(Health + amount, MaxHealth);]{dir="ltr"}        |
|                                                                      |
| []{dir="ltr"}                                                        |
|                                                                      |
| [ if (Health \<= 0)]{dir="ltr"}                                      |
|                                                                      |
| [{]{dir="ltr"}                                                       |
|                                                                      |
| [ gameObject.BroadcastMessage(]{dir="ltr"}                           |
|                                                                      |
| [ \"OnDie\",]{dir="ltr"}                                             |
|                                                                      |
| [ null,]{dir="ltr"}                                                  |
|                                                                      |
| [ SendMessageOptions.DontRequireReceiver]{dir="ltr"}                 |
|                                                                      |
| [);]{dir="ltr"}                                                      |
|                                                                      |
| []{dir="ltr"}                                                        |
|                                                                      |
| [ Destroy(gameObject);]{dir="ltr"}                                   |
|                                                                      |
| [ return;]{dir="ltr"}                                                |
|                                                                      |
| [}]{dir="ltr"}                                                       |
|                                                                      |
| []{dir="ltr"}                                                        |
|                                                                      |
| [ gameObject.BroadcastMessage(]{dir="ltr"}                           |
|                                                                      |
| [ \"OnUpdateHealth\",]{dir="ltr"}                                    |
|                                                                      |
| [ amount,]{dir="ltr"}                                                |
|                                                                      |
| [ SendMessageOptions.DontRequireReceiver]{dir="ltr"}                 |
|                                                                      |
| [);]{dir="ltr"}                                                      |
|                                                                      |
| [}]{dir="ltr"}                                                       |
+----------------------------------------------------------------------+

[Repairable (Script)]{dir="ltr"}
--------------------------------

[Attached to GameObjects that are repairable by the player.The following
properties can be configured when the Repairable script is attached to a
GameObject.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**             **[Type]{dir="ltr"}**   **[Default Value]{dir="ltr"}**
  ------------------------------------- ----------------------- --------------------------------
  [Heal Per Second]{dir="ltr"}          [float]{dir="ltr"}      [0.5]{dir="ltr"}
  [Heal Check Interval]{dir="ltr"}      [float]{dir="ltr"}      [0.2]{dir="ltr"}
  [Haptic Pulse Duration]{dir="ltr"}    [float]{dir="ltr"}      [0.1]{dir="ltr"}
  [Haptic Pulse Frequency]{dir="ltr"}   [float]{dir="ltr"}      [35]{dir="ltr"}

[]{dir="ltr"}

[The following messages are invoked by the Repairable
script.]{dir="ltr"}

[]{dir="ltr"}

  **[Message Name]{dir="ltr"}**   **[Invoked When]{dir="ltr"}**                                  **[Arguments]{dir="ltr"}**
  ------------------------------- -------------------------------------------------------------- ----------------------------
  [OnHandHoverBegin]{dir="ltr"}   [Player hovers the controller over the tower]{dir="ltr"}       [-]{dir="ltr"}
  [OnHandHoverEnd]{dir="ltr"}     [Player moves the controller away from the tower]{dir="ltr"}   [-]{dir="ltr"}

[]{dir="ltr"}

[The Repairable scripts are added to the towers, to give players a way
to repair their damaged towers. Once the player hovers their hand over a
tower, a coroutine is started that repeatedly calls the HealDamageable
method. This coroutine stops as soon as the player removes their hand
from the tower.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private IEnumerator HealDamageable(Hand hand)]{dir="ltr"}            |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // No need to repair when tower is full health]{dir="ltr"}          |
|                                                                       |
| [ if (\_damageable.Health \>= \_damageable.MaxHealth)]{dir="ltr"}     |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // Invoke this method recursively]{dir="ltr"}                       |
|                                                                       |
| [ yield return new WaitForSeconds(HealCheckInterval);]{dir="ltr"}     |
|                                                                       |
| [ yield return HealDamageable(hand);]{dir="ltr"}                      |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // We need to divide by 100 to get the health that]{dir="ltr"}      |
|                                                                       |
| [// should be added every 100ms]{dir="ltr"}                           |
|                                                                       |
| [ \_damageable.UpdateHealth(HealPerSecond \*                          |
| HealCheckInterval);]{dir="ltr"}                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Apply haptic feedback during healing process]{dir="ltr"}         |
|                                                                       |
| [ hand.TriggerHapticPulse(HapticPulseDuration, HapticPulseFrequency,  |
| 1);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Invoke this method recursively]{dir="ltr"}                       |
|                                                                       |
| [ yield return new WaitForSeconds(HealCheckInterval);]{dir="ltr"}     |
|                                                                       |
| [ yield return HealDamageable(hand);]{dir="ltr"}                      |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[PlacementCost (Script)]{dir="ltr"}
-----------------------------------

[This is a component added to towers to give them a price. The player
will be unable to place a tower unless their credit total matches the
tower price, that is given to this script as a property. After placing
the tower, the price will be deducted from the player's credit
total.]{dir="ltr"}

[SpawnCreditOnDie (Script)]{dir="ltr"}
--------------------------------------

[When an enemy dies, it spawns a credit to pick up. A player can
interact with this credit, and its value will be added to your
currency.]{dir="ltr"}

[RenderableColliders (Script)]{dir="ltr"}
-----------------------------------------

[This component will find all colliders in the transform and its
children. For every collider a representing gameobject is be spawned
with a special material. This material has a custom shader which is
transparent and has an effect where it intersects with another object.
The closer the player is to the object the more transparent the material
gets so that it won't obstruct the players view.]{dir="ltr"}

[]{dir="ltr"}

![](media/image5.png){width="2.53125in" height="2.6875in"}[]{dir="ltr"}

[]{dir="ltr"}

[Intersect effect example: the scoreboard is mostly behind the tower
range except its edge which is inside the sphere.]{dir="ltr"}

[]{dir="ltr"}

[The creation of the representing gameobject is specific to each
collider type. For the BoxCollider, SphereCollider and CapsuleColliders
a static primative mesh is created and reused for every instance to
optimise resources.]{dir="ltr"}

[]{dir="ltr"}

[MeshCollider:]{dir="ltr"}

+-----------------------------------------------------------------------+
| [case MeshCollider meshCollider:]{dir="ltr"}                          |
|                                                                       |
| [ meshFilter.mesh = meshCollider.sharedMesh;]{dir="ltr"}              |
|                                                                       |
| [ meshGameObject.transform.localScale =                               |
| collider.transform.localScale;]{dir="ltr"}                            |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ break;]{dir="ltr"}                                                  |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[BoxCollider:]{dir="ltr"}

+-----------------------------------------------------------------------+
| [case BoxCollider boxCollider:]{dir="ltr"}                            |
|                                                                       |
| [ meshFilter.mesh = CubeMesh;]{dir="ltr"}                             |
|                                                                       |
| [ meshGameObject.transform.localScale = boxCollider.size;]{dir="ltr"} |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ break;]{dir="ltr"}                                                  |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[SphereCollider:]{dir="ltr"}

+-----------------------------------------------------------------------+
| [case SphereCollider sphereCollider:]{dir="ltr"}                      |
|                                                                       |
| [ meshFilter.mesh = SphereMesh;]{dir="ltr"}                           |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var largestLocalScaleComponent = Mathf.Max(]{dir="ltr"}             |
|                                                                       |
| [ Mathf.Max(transform.localScale.x,                                   |
| transform.localScale.y),]{dir="ltr"}                                  |
|                                                                       |
| [ transform.localScale.z]{dir="ltr"}                                  |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var worldSpaceScale = sphereCollider.radius \* 2 \*                 |
| largestLocalScaleComponent;]{dir="ltr"}                               |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ meshGameObject.transform.localScale = new Vector3(]{dir="ltr"}      |
|                                                                       |
| [ worldSpaceScale / transform.localScale.x,]{dir="ltr"}               |
|                                                                       |
| [ worldSpaceScale / transform.localScale.y,]{dir="ltr"}               |
|                                                                       |
| [ worldSpaceScale / transform.localScale.z]{dir="ltr"}                |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ break;]{dir="ltr"}                                                  |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[CapsuleCollider:]{dir="ltr"}

+------------------------------------------------------------------+
| [case CapsuleCollider capsuleCollider:]{dir="ltr"}               |
|                                                                  |
| [ meshFilter.mesh = CapsuleMesh;]{dir="ltr"}                     |
|                                                                  |
| [ meshGameObject.transform.localScale = new Vector3(]{dir="ltr"} |
|                                                                  |
| [ capsuleCollider.radius \* 2,]{dir="ltr"}                       |
|                                                                  |
| [ capsuleCollider.height / 2,]{dir="ltr"}                        |
|                                                                  |
| [ capsuleCollider.radius \* 2]{dir="ltr"}                        |
|                                                                  |
| [);]{dir="ltr"}                                                  |
|                                                                  |
| []{dir="ltr"}                                                    |
|                                                                  |
| [ break;]{dir="ltr"}                                             |
+------------------------------------------------------------------+

[WorldPlaceable (Script)]{dir="ltr"}
------------------------------------

[This component has two functionalities, to give the user visual
feedback if he's able to place the object and to block the object when
it was placed while being in an invalid location. The following
properties are configurable on the WorldPlaceable script.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**      **[Description]{dir="ltr"}**
  ------------------------------ ------------------------------------------------------------------------------------------------------------------------------------------------------------------
  [Object collider]{dir="ltr"}   [The collider that will be used for Is Visual]{dir="ltr"}
  [Is Visual]{dir="ltr"}         [If true, first material's color will be changed to invalid color when it's colliding with a collider in either of the path, enemies or tower layers]{dir="ltr"}
  [Invalid Color]{dir="ltr"}     [Customizable color, it's recommended to have this fairly different from the original color.]{dir="ltr"}

[]{dir="ltr"}

![](media/image38.png){width="6.192708880139983in"
height="3.4314227909011374in"}[]{dir="ltr"}

[]{dir="ltr"}

[Scoreboard]{dir="ltr"}
=======================

[The scoreboard is a game object that is spawned whenever you begin a
new game. The scoreboard is a tool used by the player to keep track of
various statistics of the game. These statistics include the score,
current wave number and remaining lives. The scoreboard also functions
as an audio source, announcing various things during the gameplay that
could be useful to the player, like "Wave spawned".\
\
The scoreboard remains within the player's vision as the player looks
around, but not too centered, so it doesn't bother the player too much.
This way the player won't lose track of the scoreboard.]{dir="ltr"}

![](media/image7.png){width="2.71205927384077in" height="2.5364588801399823in"}[]{dir="ltr"}
--------------------------------------------------------------------------------------------

[The following components are added to the scoreboard to make it
function correctly:]{dir="ltr"}

[]{dir="ltr"}

  **[Component]{dir="ltr"}**                 **[Type]{dir="ltr"}**      **[Description]{dir="ltr"}**
  ------------------------------------------ -------------------------- -------------------------------------------------------------------------------------------------------------------
  [Scoreboard]{dir="ltr"}                    [Script]{dir="ltr"}        [Contains all the various methods required to keep the statistics shown on the scoreboard up-to-date.]{dir="ltr"}
  [AudioSource]{dir="ltr"}                   [AudioSource]{dir="ltr"}   [Used to play announcements.]{dir="ltr"}
  [ScoreboardSteeringBehaviour]{dir="ltr"}   [Script]{dir="ltr"}        [Contains the methods required to rotate the scoreboard and to move it along the player's vision.]{dir="ltr"}

[Statistics]{dir="ltr"}
-----------------------

[The scoreboard has one initial child. This child is a canvas and has a
total of six text fields on it. Each text field either contains the name
of a statistic or the value of a statistic.]{dir="ltr"}

[]{dir="ltr"}

[The statistics function as follows:]{dir="ltr"}

[]{dir="ltr"}

  **[Statistic]{dir="ltr"}**   **[Description]{dir="ltr"}**
  ---------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------
  [Score]{dir="ltr"}           [Displays the current score the player has. Score is gained whenever an enemy is defeated.]{dir="ltr"}
  [Wave]{dir="ltr"}            [Displays the current wave the player is in. Also shows the total amount of waves the player can expect.]{dir="ltr"}
  [Lives]{dir="ltr"}           [Displays the player's remaining lives. A life is lost whenever an enemy reaches the end of the path. When this drops to zero, the player loses.]{dir="ltr"}

[]{dir="ltr"}

[The scoreboard script collects most of its statistics from the Player
Statistics. Any update to the values of stats will be updated there
first and those changes will then be broadcasted to the scoreboard
through the OnPlayerStatisticsUpdate method. The wave numbers get
updates by the scoreboard itself, however, the total wave amount is
taken from the WavesState component within the Gamemanager.]{dir="ltr"}\
[]{dir="ltr"}

[Towers]{dir="ltr"}
===================

[Light Sky Defense has three main tower types: a machine gun, a missile
launcher and a shotgun. Even though the towers act in a different way,
there is some similarity between the towers. We've created a base tower
script that is used to manage the behaviour for all towers.]{dir="ltr"}

[BaseTower (Prefab)]{dir="ltr"}
-------------------------------

[All towers share some functionality, this functionality is implemented
in the BaseTower class. The BaseTower class also acts as a host for the
"state machine" implementation.]{dir="ltr"}

### [Components]{dir="ltr"}

  **[Component]{dir="ltr"}**       **[Type]{dir="ltr"}**   [**What is it used for**]{dir="ltr"}
  -------------------------------- ----------------------- -------------------------------------------------------------------------------------------
  [Interactable]{dir="ltr"}        [Script]{dir="ltr"}     [Used to make towers interactable by the player.]{dir="ltr"}
  [Deleteable]{dir="ltr"}          [Script]{dir="ltr"}     [Used to make towers deletable by the player.]{dir="ltr"}
  [Damageable]{dir="ltr"}          [Script]{dir="ltr"}     [Used to give towers health and make them damageable by enemies.]{dir="ltr"}
  [Repairable]{dir="ltr"}          [Script]{dir="ltr"}     [Used to give players the option to repair their towers when damaged.]{dir="ltr"}
  [BaseTower]{dir="ltr"}           [Script]{dir="ltr"}     [Used to provide towers with states, enemy detection and a buildable preview.]{dir="ltr"}
  [IdleRotationState]{dir="ltr"}   [Script]{dir="ltr"}     [Used to make towers rotate aimlessly when put in the Idle State.]{dir="ltr"}
  [JamState]{dir="ltr"}            [Script]{dir="ltr"}     [Used to stop towers from shooting after getting put in the Jam State.]{dir="ltr"}
  [CelebrationState]{dir="ltr"}    [Script]{dir="ltr"}     [Used to have towers shoot fireworks after a game is won.]{dir="ltr"}

### [Tower states]{dir="ltr"}

[It's a hassle to create a "real" state machine for a Unity component
because the states won't be configurable in the Unity editor. Instead,
we've created several classes that inherit from TowerState, these
classes are added to the tower prefabs. The states are then referenced
in the BaseTower script using the Unity editor.]{dir="ltr"}

[]{dir="ltr"}

[A tower can have the following states:]{dir="ltr"}

[]{dir="ltr"}

  **[StateName]{dir="ltr"}**      **[Description]{dir="ltr"}**
  ------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------
  [IdleState]{dir="ltr"}          [This state is enabled when a tower is waiting for an enemy to get in its range.]{dir="ltr"}
  [ActiveState]{dir="ltr"}        [This state does the heavy lifting, it is enabled when enemies are within the towers' range.]{dir="ltr"}
  [CelebrationState]{dir="ltr"}   [This state is enabled when the player wins the game. A player wins the game when all enemies are killed after the last wave ends.]{dir="ltr"}
  [CondemnState]{dir="ltr"}       [This state is enabled when the player loses.]{dir="ltr"}
  [JamState]{dir="ltr"}           [This state is enabled when a tower is jammed and can't operate normally]{dir="ltr"}

[]{dir="ltr"}

[The behaviour for the tower states are implemented in scripts that
inherit the TowerState class. The TowerState abstract class can be used
to change the state of a tower.]{dir="ltr"}

[]{dir="ltr"}

[![](media/image6.png){width="6.203125546806649in"
height="2.8327602799650045in"}](https://www.draw.io/?page-id=cVtbPN3QPF5NSDrd8CgF&scale=auto#G1c5hBVLcxtg8zhcW1pUzi6nyyxUMuUoRz)[]{dir="ltr"}

### [Health Bar]{dir="ltr"}

[To keep track of each tower's health, they have been provided with a
health bar. This is a vertical slider that is located on the left side
of the tower (player facing the tower). The health bar keeps track of
the tower's current health value and adjusts the green bar accordingly.
The slider has been drawn on a canvas that is connected to the
tower.]{dir="ltr"}

[]{dir="ltr"}

![](media/image3.png){width="1.453125546806649in"
height="2.23463801399825in"}[]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

  **[Component]{dir="ltr"}**        **[Type]{dir="ltr"}**   **[Description]{dir="ltr"}**
  --------------------------------- ----------------------- -------------------------------------------------------------------------------
  [Slider]{dir="ltr"}               [Script]{dir="ltr"}     [Used to provide functionality to the slider.]{dir="ltr"}
  [Image (Background)]{dir="ltr"}   [Script]{dir="ltr"}     [Used to adjust properties for the background part of the slider.]{dir="ltr"}
  [Image (Foreground)]{dir="ltr"}   [Script]{dir="ltr"}     [Used to adjust properties for the foreground part of the slider.]{dir="ltr"}

[]{dir="ltr"}

[The slider script contains a few important adjustable parts of the
slider, but in our case we have only adjusted it's view from top to
bottom (instead of left to right) and adjusted its maximum value to fit
our tower's maximum health.]{dir="ltr"}

[]{dir="ltr"}

[Both image scripts should be roughly kept the same as they are
initially provided to you, apart from the visual aspect option that you
can toy around with to change its look. Its current look is what seemed
best in our game.]{dir="ltr"}

[]{dir="ltr"}

[On load the HealthBarElement which is a slider with a green foreground
and red background, is updated to the health values of the tower. When
the component receives an OnUpdateHealth() message, it will update the
HealtBarElement to display the correct value.]{dir="ltr"}

[]{dir="ltr"}

+--------------------------------------------------------------------+
| [void Start()]{dir="ltr"}                                          |
|                                                                    |
| [{]{dir="ltr"}                                                     |
|                                                                    |
| [ \_damageable = Tower.GetComponent\<Damageable\>();]{dir="ltr"}   |
|                                                                    |
| []{dir="ltr"}                                                      |
|                                                                    |
| [ HealthBarElement.maxValue = \_damageable.MaxHealth;]{dir="ltr"}  |
|                                                                    |
| [ HealthBarElement.value = \_damageable.InitialHealth;]{dir="ltr"} |
|                                                                    |
| [}]{dir="ltr"}                                                     |
|                                                                    |
| []{dir="ltr"}                                                      |
|                                                                    |
| [void OnUpdateHealth()]{dir="ltr"}                                 |
|                                                                    |
| [{]{dir="ltr"}                                                     |
|                                                                    |
| [ HealthBarElement.value = \_damageable.Health;]{dir="ltr"}        |
|                                                                    |
| [}]{dir="ltr"}                                                     |
+--------------------------------------------------------------------+

### [BaseTower (Script)]{dir="ltr"}

[The BaseTower script is added to the BaseTower prefab. This script
manages the towers' state, checks if enemies are in range, and make sure
that the tower OnDie and OnDelete functionality is implemented
correctly. The following properties can be configured on the BaseTower
script.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**         **[Type]{dir="ltr"}**        **[Description]{dir="ltr"}**
  --------------------------------- ---------------------------- -------------------------------------------------------------------------------------------------------------
  [IdleState]{dir="ltr"}            [TowerState]{dir="ltr"}      [The state that will be enabled when there aren't any enemies in range.]{dir="ltr"}
  [ActiveState]{dir="ltr"}          [TowerState]{dir="ltr"}      [The state that will be enabled when enemies are in the towers' range.]{dir="ltr"}
  [CelebrationState]{dir="ltr"}     [TowerState]{dir="ltr"}      [The state that will be enabled when a player wins.]{dir="ltr"}
  [CondemnState]{dir="ltr"}         [TowerState]{dir="ltr"}      [The state that will be enabled when a player loses.]{dir="ltr"}
  [JamState]{dir="ltr"}             [JamState]{dir="ltr"}        [The state that will be enabled when a tower is jammed by a jammer projectile.]{dir="ltr"}
  [InitialState]{dir="ltr"}         [TowerState]{dir="ltr"}      [The state that will be enabled when a tower is instantiated.]{dir="ltr"}
  [ProjectileSpawns]{dir="ltr"}     [Transform\[\]]{dir="ltr"}   [The locations from which projectiles are emitted.]{dir="ltr"}
  [DetectionLayerMask]{dir="ltr"}   [LayerMask]{dir="ltr"}       [The layer mask that is used when checking for collisions.]{dir="ltr"}
  [Range]{dir="ltr"}                [Float]{dir="ltr"}           [The range which is used to check for collision with objects that match the DetectionLayerMask.]{dir="ltr"}
  [Preview]{dir="ltr"}              [Buildable]{dir="ltr"}       [Used to get the price of a tower for the tower delete behaviour.]{dir="ltr"}

[]{dir="ltr"}

#### [OnDie]{dir="ltr"}

[A tower loses health when an enemy or enemy bullets collide with the
tower. The OnDie message is broadcasted when the health in the
Damageable script less than zero. A tower is destroyed when it
"dies".]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                    |
|                                                                 |
| [/// Invoked by Damageable when the tower hits 0 hp]{dir="ltr"} |
|                                                                 |
| [/// \</summary\>]{dir="ltr"}                                   |
|                                                                 |
| [public void OnDie()]{dir="ltr"}                                |
|                                                                 |
| [{]{dir="ltr"}                                                  |
|                                                                 |
| [ Destroy(gameObject);]{dir="ltr"}                              |
|                                                                 |
| [}]{dir="ltr"}                                                  |
+-----------------------------------------------------------------+

#### [OnDelete]{dir="ltr"}

[A tower can be deleted by the player because the Deleteable script is
added to the BaseTower prefab. The deletable script will broadcast the
OnDelete message when a tower should be deleted. The tower is refunded
and destroyed when the player deletes a tower.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                          |
|                                                                       |
| [/// Invoked by Deletable when the player holds down the              |
| trigger]{dir="ltr"}                                                   |
|                                                                       |
| [/// \</summary\>]{dir="ltr"}                                         |
|                                                                       |
| [public void OnDelete()]{dir="ltr"}                                   |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // Increment funds on tower delete]{dir="ltr"}                      |
|                                                                       |
| [ var playerStatistics =                                              |
| Player.instance.GetComponent\<PlayerStatistics\>();]{dir="ltr"}       |
|                                                                       |
| [ playerStatistics.UpdateFunds(Preview.Price);]{dir="ltr"}            |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ Destroy(gameObject);]{dir="ltr"}                                    |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

#### [Active and Idle state]{dir="ltr"}

[Every tower keeps track of targets (Enemies) in its range. When there
are no targets in range, the tower will switch to IdleState. When there
is a target in its range, it switches to ActiveState.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private void FixedUpdate()]{dir="ltr"}                               |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ TargetsInRange = Physics.OverlapSphere(]{dir="ltr"}                 |
|                                                                       |
| [ transform.position,]{dir="ltr"}                                     |
|                                                                       |
| [ Range,]{dir="ltr"}                                                  |
|                                                                       |
| [(int)DetectionLayerMask]{dir="ltr"}                                  |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ if (TargetsInRange.Length \< 1 && CurrentState ==                   |
| ActiveState)]{dir="ltr"}                                              |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ CurrentState.SetTowerState(IdleState);]{dir="ltr"}                  |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [ if (TargetsInRange.Length \> 0 && CurrentState ==                   |
| IdleState)]{dir="ltr"}                                                |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ CurrentState.SetTowerState(ActiveState);]{dir="ltr"}                |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

#### [Jam state]{dir="ltr"}

[When a jammer projectile hits a tower its state will be changed to
JamState. The JamState will Unjam after the given jamTime.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                          |
|                                                                       |
| [/// Update the tower state when the tower gets hit by a              |
| jammer]{dir="ltr"}                                                    |
|                                                                       |
| [/// \</summary\>]{dir="ltr"}                                         |
|                                                                       |
| [public void OnJam(float jamTime)]{dir="ltr"}                         |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ JamState.JamTime = jamTime;]{dir="ltr"}                             |
|                                                                       |
| [ CurrentState.SetTowerState(JamState);]{dir="ltr"}                   |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

#### [Win and lose state]{dir="ltr"}

[When the player wins or loses the OnGameWin and OnGameLose messages are
broadcasted to all gameObjects. We use these messages in the BaseTower
script to change the state of a tower to the CelebrationState when the
player wins, and the CondemnState when the player loses.]{dir="ltr"}

[]{dir="ltr"}

+---------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                  |
|                                                               |
| [/// Update the tower state when the player wins]{dir="ltr"}  |
|                                                               |
| [/// \</summary\>]{dir="ltr"}                                 |
|                                                               |
| [public void OnGameWin()]{dir="ltr"}                          |
|                                                               |
| [{]{dir="ltr"}                                                |
|                                                               |
| [ CurrentState.SetTowerState(CelebrationState);]{dir="ltr"}   |
|                                                               |
| [}]{dir="ltr"}                                                |
|                                                               |
| []{dir="ltr"}                                                 |
|                                                               |
| [/// \<summary\>]{dir="ltr"}                                  |
|                                                               |
| [/// Update the tower state when the player loses]{dir="ltr"} |
|                                                               |
| [/// \</summary\>]{dir="ltr"}                                 |
|                                                               |
| [public void OnGameLose()]{dir="ltr"}                         |
|                                                               |
| [{]{dir="ltr"}                                                |
|                                                               |
| [ CurrentState.SetTowerState(CondemnState);]{dir="ltr"}       |
|                                                               |
| [}]{dir="ltr"}                                                |
+---------------------------------------------------------------+

### [IdleRotationState (Script)]{dir="ltr"}

[The IdleRotationState is the initial state that is enabled. The
IdleRotationState will continuously rotate the tower to a random
location. The following properties can be configured on the
IdleRotationState component.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**    **[Type]{dir="ltr"}**   **[Description]{dir="ltr"}**
  ---------------------------- ----------------------- -------------------------------------------------------------------
  [RotationSpeed]{dir="ltr"}   [Float]{dir="ltr"}      [Angle in degrees that the tower rotates every second]{dir="ltr"}

[]{dir="ltr"}

[The IdleRotationState continuously rotates the tower towards a random
rotation using the Quaternion.RotateTowards function. When the angle
between the tower rotation and the random rotation is less than 1 degree
we get a new random rotation.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                          |
|                                                                       |
| [/// Continuously rotate the tower]{dir="ltr"}                        |
|                                                                       |
| [/// \</summary\>]{dir="ltr"}                                         |
|                                                                       |
| [private void FixedUpdate()]{dir="ltr"}                               |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ if (Quaternion.Angle(transform.rotation, \_randomRotation) \<       |
| 1)]{dir="ltr"}                                                        |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ \_randomRotation = Random.rotation;]{dir="ltr"}                     |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ transform.rotation = Quaternion.RotateTowards(]{dir="ltr"}          |
|                                                                       |
| [ transform.rotation,]{dir="ltr"}                                     |
|                                                                       |
| [ \_randomRotation,]{dir="ltr"}                                       |
|                                                                       |
| [ RotationSpeed \* Time.deltaTime]{dir="ltr"}                         |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

### [JamState (Script)]{dir="ltr"}

[Every tower can be jammed by the JammerEnemy, the JamState is enabled
when the tower is hit by a jammer bullet. When the JamState is enabled a
JamEffect will be played, we also invoke the Unjam method using the
Invoke method from the Unity MonoBehaviour once the JamTime has elapsed.
Unjam will set the state back to the tower's idle state.]{dir="ltr"}

[]{dir="ltr"}

+-------------------------------------------------+
| [public class JamState : TowerState]{dir="ltr"} |
|                                                 |
| [{]{dir="ltr"}                                  |
|                                                 |
| [ public float JamTime;]{dir="ltr"}             |
|                                                 |
| []{dir="ltr"}                                   |
|                                                 |
| [\[Header(\"Jam effect\")\]]{dir="ltr"}         |
|                                                 |
| [ public ParticleSystem JamEffect;]{dir="ltr"}  |
|                                                 |
| []{dir="ltr"}                                   |
|                                                 |
| [ void OnEnable()]{dir="ltr"}                   |
|                                                 |
| [{]{dir="ltr"}                                  |
|                                                 |
| [ JamEffect.Play();]{dir="ltr"}                 |
|                                                 |
| []{dir="ltr"}                                   |
|                                                 |
| [ CancelInvoke(\"Unjam\");]{dir="ltr"}          |
|                                                 |
| [ Invoke(\"Unjam\", JamTime);]{dir="ltr"}       |
|                                                 |
| [}]{dir="ltr"}                                  |
|                                                 |
| []{dir="ltr"}                                   |
|                                                 |
| [ private void Unjam()]{dir="ltr"}              |
|                                                 |
| [{]{dir="ltr"}                                  |
|                                                 |
| [ SetTowerState(Tower.IdleState);]{dir="ltr"}   |
|                                                 |
| [}]{dir="ltr"}                                  |
|                                                 |
| [}]{dir="ltr"}                                  |
+-------------------------------------------------+

[]{dir="ltr"}

[Machine gun]{dir="ltr"}
------------------------

[The machine gun is one of the towers that can be placed into the world
by the player. This particular tower shoots high velocity, low damage
bullets at a low interval.]{dir="ltr"}

[]{dir="ltr"}

![](media/image18.png){width="2.9114588801399823in"
height="2.421763998250219in"}[]{dir="ltr"}

[]{dir="ltr"}

### [Components]{dir="ltr"}

[The following components are added to the machine gun game
object.]{dir="ltr"}

[]{dir="ltr"}

  **[Component]{dir="ltr"}**      **[Type]{dir="ltr"}**   **[What is it used for]{dir="ltr"}**
  ------------------------------- ----------------------- -----------------------------------------------------------------
  [Sphere Collider]{dir="ltr"}    [Collider]{dir="ltr"}   [Used for control interactions and enemy collisions]{dir="ltr"}
  [ShootBulletState]{dir="ltr"}   [Script]{dir="ltr"}     [Used to make a tower shoot correctly]{dir="ltr"}

### [ShootBulletState (Script)]{dir="ltr"}

[The ShootBulletState is what makes the Machine Gun what it is. The
properties of the ShootBulletState can be seen in the table
below:]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**       **[Type]{dir="ltr"}**    **[Meaning]{dir="ltr"}**
  ------------------------------- ------------------------ ---------------------------------------------------------------------------------
  [Rotation Speed]{dir="ltr"}     [float]{dir="ltr"}       [The speed at which the tower rotates towards its target]{dir="ltr"}
  [Angle Threshold]{dir="ltr"}    [float]{dir="ltr"}       [Used to determine whether an enemy is in front of the tower or not]{dir="ltr"}
  [Projectile]{dir="ltr"}         [Rigidbody]{dir="ltr"}   [The projectile shot by the tower]{dir="ltr"}
  [Projectile Speed]{dir="ltr"}   [float]{dir="ltr"}       [The speed of the projectile]{dir="ltr"}
  [Cooldown]{dir="ltr"}           [float]{dir="ltr"}       [Time between projectile shots]{dir="ltr"}

[]{dir="ltr"}

#### [Rotation]{dir="ltr"}

[The rotation of the Machine Gun works in such a way that it has a form
of pursuit behaviour. After finding a target, the tower predicts the
position the target will be in, in the time it will take for a bullet to
reach the target. By doing this, the tower can slightly angle itself in
front of the enemy and then shoot, resulting in the enemy getting hit
after moving in that timeframe.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private void UpdateRotation()]{dir="ltr"}                            |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var targetDistance = Vector3.Distance(]{dir="ltr"}                  |
|                                                                       |
| [ transform.position,]{dir="ltr"}                                     |
|                                                                       |
| [ \_target.position]{dir="ltr"}                                       |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var travelTime = targetDistance / ProjectileSpeed;]{dir="ltr"}      |
|                                                                       |
| [ var targetDisplacement = \_target.velocity \*                       |
| travelTime;]{dir="ltr"}                                               |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var predictedlookRotation = Quaternion.LookRotation(]{dir="ltr"}    |
|                                                                       |
| [(\_target.position + targetDisplacement) -                           |
| transform.position]{dir="ltr"}                                        |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Rotate our transform a step closer to the target\'s.]{dir="ltr"} |
|                                                                       |
| [ transform.rotation = Quaternion.RotateTowards(]{dir="ltr"}          |
|                                                                       |
| [ transform.rotation,]{dir="ltr"}                                     |
|                                                                       |
| [ predictedlookRotation,]{dir="ltr"}                                  |
|                                                                       |
| [ RotationSpeed \* Time.deltaTime]{dir="ltr"}                         |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

#### [Shooting]{dir="ltr"}

[After finding a target, the Machine Gun will check if the target is in
front of the tower. If so, a coroutine is started and the tower will
fire continuously until the target leaves the tower's
radius.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private void EnableShootRoutineWhenTargetIsInFov()]{dir="ltr"}       |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // Angle between target and center of tower]{dir="ltr"}             |
|                                                                       |
| [ var targetAngle = Vector3.Angle(]{dir="ltr"}                        |
|                                                                       |
| [ \_target.position - transform.position,]{dir="ltr"}                 |
|                                                                       |
| [ transform.forward]{dir="ltr"}                                       |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Only start shooting when enemy is in front of tower, stop        |
| shooting otherwise]{dir="ltr"}                                        |
|                                                                       |
| [ if (targetAngle \<= AngleThreshold &&                               |
| !\_coroutineStarted)]{dir="ltr"}                                      |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ StartCoroutine(ShootProjectile());]{dir="ltr"}                      |
|                                                                       |
| [ \_coroutineStarted = true;]{dir="ltr"}                              |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [ else if (targetAngle \> AngleThreshold &&                           |
| \_coroutineStarted)]{dir="ltr"}                                       |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ StopAllCoroutines();]{dir="ltr"}                                    |
|                                                                       |
| [ \_coroutineStarted = false;]{dir="ltr"}                             |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[Each time a shot is fired, a given cooldown (in the properties) is
incurred before the next shot can be fired.]{dir="ltr"}

[]{dir="ltr"}

+--------------------------------------------------------------+
| [protected virtual IEnumerator ShootProjectile()]{dir="ltr"} |
|                                                              |
| [{]{dir="ltr"}                                               |
|                                                              |
| [ // Wait before we start shooting]{dir="ltr"}               |
|                                                              |
| [ yield return new WaitForSeconds(Cooldown);]{dir="ltr"}     |
|                                                              |
| []{dir="ltr"}                                                |
|                                                              |
| [ foreach (var spawn in Tower.ProjectileSpawns)]{dir="ltr"}  |
|                                                              |
| [{]{dir="ltr"}                                               |
|                                                              |
| [ var newProjectile = Instantiate(]{dir="ltr"}               |
|                                                              |
| [ Projectile,]{dir="ltr"}                                    |
|                                                              |
| [ spawn.position,]{dir="ltr"}                                |
|                                                              |
| [ spawn.rotation]{dir="ltr"}                                 |
|                                                              |
| [);]{dir="ltr"}                                              |
|                                                              |
| []{dir="ltr"}                                                |
|                                                              |
| [ newProjectile.AddForce(]{dir="ltr"}                        |
|                                                              |
| [ transform.forward \* ProjectileSpeed,]{dir="ltr"}          |
|                                                              |
| [ ForceMode.VelocityChange]{dir="ltr"}                       |
|                                                              |
| [);]{dir="ltr"}                                              |
|                                                              |
| [}]{dir="ltr"}                                               |
|                                                              |
| []{dir="ltr"}                                                |
|                                                              |
| [ // Invoke this method recursively]{dir="ltr"}              |
|                                                              |
| [ yield return ShootProjectile();]{dir="ltr"}                |
|                                                              |
| [}]{dir="ltr"}                                               |
+--------------------------------------------------------------+

[]{dir="ltr"}

[]{dir="ltr"}

 []{dir="ltr"}
-------------

[Shotgun]{dir="ltr"}
--------------------

[The shotgun is a tower that shoots multiple bullets and is best used
for area control and pushback.]{dir="ltr"}

[]{dir="ltr"}

![](media/image19.png){width="2.9209175415573054in"
height="2.619792213473316in"}[]{dir="ltr"}

[\
The concrete state(s) can be seen in the table below:]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**   **[Concrete]{dir="ltr"}**
  --------------------------- ----------------------------
  [ActiveState]{dir="ltr"}    [Shotgun State]{dir="ltr"}

[\
The shotgun state is this tower's unique feature.]{dir="ltr"}

### [ShotgunState (Script)]{dir="ltr"}

[The shotgun states make the tower act like a shotgun. The ShotgunState
extends the ShootBulletState since it works almost exactly like the
ShootBulletState. The only difference is that it inaccurately shoots
multiple bullets towards the enemy instead of just one. The properties
of the shotgun state can be seen in the table below:]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**                    **[Type]{dir="ltr"}**   **[Meaning]{dir="ltr"}**
  -------------------------------------------- ----------------------- -----------------------------------------------------------------------------------------
  [PelletCount]{dir="ltr"}                     [Integer]{dir="ltr"}    [The amount of bullets to be spawned]{dir="ltr"}
  [Spread]{dir="ltr"}                          [Float]{dir="ltr"}      [The spread in degrees of the bullets.]{dir="ltr"}
  [ProjectileSpeedVariationRatio]{dir="ltr"}   [Float]{dir="ltr"}      [The total damage when all bullets hit. Damage per Bullet is Damage/Bullets]{dir="ltr"}

[]{dir="ltr"}

#### [Shooting multiple bullets]{dir="ltr"}

[To get the random angle we make a new Quaternion through an Euler
vector in which the x and y are random values between -spread and
spread. The z, which is the forward vector, is always 1. We apply this
rotation towards the default forward vector to get a new rotated
direction to shoot towards. We do this for each bullet that we
shoot.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [for (int i = 0; i \< PelletCount; i++)]{dir="ltr"}                   |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var currentSpawn = Tower.ProjectileSpawns\[]{dir="ltr"}             |
|                                                                       |
| [ i % Tower.ProjectileSpawns.Length]{dir="ltr"}                       |
|                                                                       |
| [\];]{dir="ltr"}                                                      |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Instantiate new projectile]{dir="ltr"}                           |
|                                                                       |
| [ var projectile = Instantiate(]{dir="ltr"}                           |
|                                                                       |
| [ Projectile,]{dir="ltr"}                                             |
|                                                                       |
| [ currentSpawn.position,]{dir="ltr"}                                  |
|                                                                       |
| [ currentSpawn.rotation]{dir="ltr"}                                   |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Create random local rotation]{dir="ltr"}                         |
|                                                                       |
| [ var localRotation = Quaternion.Euler(]{dir="ltr"}                   |
|                                                                       |
| [ Random.Range(-Spread, Spread),]{dir="ltr"}                          |
|                                                                       |
| [ Random.Range(-Spread, Spread),]{dir="ltr"}                          |
|                                                                       |
| [ 1]{dir="ltr"}                                                       |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Multiply random rotation with spawn direction to create shoot    |
| direction]{dir="ltr"}                                                 |
|                                                                       |
| [ var shootDirection = localRotation \*                               |
| currentSpawn.forward;]{dir="ltr"}                                     |
|                                                                       |
| [ var speedVariation = Random.Range(]{dir="ltr"}                      |
|                                                                       |
| [-ProjectileSpeedVariationRatio,]{dir="ltr"}                          |
|                                                                       |
| [ ProjectileSpeedVariationRatio]{dir="ltr"}                           |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Apply force to projectile in shoot direction]{dir="ltr"}         |
|                                                                       |
| [ projectile.AddForce(]{dir="ltr"}                                    |
|                                                                       |
| [ shootDirection \* (ProjectileSpeed + speedVariation),]{dir="ltr"}   |
|                                                                       |
| [ ForceMode.VelocityChange]{dir="ltr"}                                |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[Missile launcher]{dir="ltr"}
-----------------------------

[The missile launcher is an object that can be placed into the world by
the player. The missile launcher shoots homing missiles at
enemies.]{dir="ltr"}

[]{dir="ltr"}

![](media/image13.png){width="2.284346019247594in"
height="2.2031255468066493in"}[]{dir="ltr"}

[]{dir="ltr"}

[The following components are added to the MissileLauncher
prefab.]{dir="ltr"}

[]{dir="ltr"}

  **[Component]{dir="ltr"}**         **[Type]{dir="ltr"}**   **[What is it used for]{dir="ltr"}**
  ---------------------------------- ----------------------- ---------------------------------------------------------------
  [BoxCollider]{dir="ltr"}           [Collider]{dir="ltr"}   [Used for control interactions or enemy collision]{dir="ltr"}
  [Shoot Missile State]{dir="ltr"}   [Script]{dir="ltr"}     [Used to make a tower shoot correctly]{dir="ltr"}

### [ShootMissileState (Script)]{dir="ltr"}

[The ShootMissileState script gives the missile launcher it's ability to
fire missiles. These missiles are fired in a unique way, a way that can
be accomplished with the various methods in this script.\
]{dir="ltr"}

[The following properties can be configured on the ShootMissileState
script:]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**    **[Type]{dir="ltr"}**     **[Description]{dir="ltr"}**
  ---------------------------- ------------------------- ----------------------------------------------------------------------------------------------------------------
  [RotationSpeed]{dir="ltr"}   [Float]{dir="ltr"}        [Angle in degrees that the tower rotates every second]{dir="ltr"}
  [Projectile]{dir="ltr"}      [GameObject]{dir="ltr"}   [A prefab of the projectile it fires]{dir="ltr"}
  [Cooldown]{dir="ltr"}        [Float]{dir="ltr"}        [The time it takes to fire again]{dir="ltr"}
  [EjectInterval]{dir="ltr"}   [Float]{dir="ltr"}        [The time the launcher needs to wait before it can eject a missile from its next projectile spawn.]{dir="ltr"}

[]{dir="ltr"}

[To provide the tower with continuous firing, we make use of a
coroutine. A coroutine can be used to call a method repeatedly until
manually stopped. To make sure the tower has all it's missiles, we call
Reload once before the coroutine.]{dir="ltr"}

[ ]{dir="ltr"}

+------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                   |
|                                                |
| [/// Start shooting missiles]{dir="ltr"}       |
|                                                |
| [/// \</summary\>]{dir="ltr"}                  |
|                                                |
| [private void OnEnable()]{dir="ltr"}           |
|                                                |
| [{]{dir="ltr"}                                 |
|                                                |
| [ Reload();]{dir="ltr"}                        |
|                                                |
| [ StartCoroutine(ShootMissiles());]{dir="ltr"} |
|                                                |
| [}]{dir="ltr"}                                 |
|                                                |
| []{dir="ltr"}                                  |
|                                                |
| [/// \<summary\>]{dir="ltr"}                   |
|                                                |
| [/// Stop shooting missiles]{dir="ltr"}        |
|                                                |
| [/// \</summary\>]{dir="ltr"}                  |
|                                                |
| [private void OnDisable()]{dir="ltr"}          |
|                                                |
| [{]{dir="ltr"}                                 |
|                                                |
| [ StopAllCoroutines();]{dir="ltr"}             |
|                                                |
| [}]{dir="ltr"}                                 |
+------------------------------------------------+

[]{dir="ltr"}

[The rotation of a missile launcher is based on the position and amount
of enemies in its range. It adds all the enemy positions together and
divides them by the enemy count, basically aiming itself towards the
middle of the enemy cluster. The reason this works, is because the
missiles will rotate themselves towards the enemy (so the launcher
doesn't have to).]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private void FixedUpdate()]{dir="ltr"}                               |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var lookDirection = Vector3.zero;]{dir="ltr"}                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ foreach (var collider in Tower.TargetsInRange)]{dir="ltr"}          |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ lookDirection += collider.transform.position;]{dir="ltr"}           |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ lookDirection /= Tower.TargetsInRange.Length;]{dir="ltr"}           |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ transform.rotation = Quaternion.RotateTowards(]{dir="ltr"}          |
|                                                                       |
| [ transform.rotation,]{dir="ltr"}                                     |
|                                                                       |
| [ Quaternion.LookRotation(lookDirection -                             |
| transform.position),]{dir="ltr"}                                      |
|                                                                       |
| [ RotationSpeed \* Time.deltaTime]{dir="ltr"}                         |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[The missile launcher contains four projectile spawn slots. These will
launch (Eject) a missile on a EjectInterval. After receiving a message,
the missile will launch itself. This will visually empty the missile
slot. The next missile will be fired from the next projectile spawn.
When the missile launcher has no more projectiles in its slots, it will
reload with a cooldown.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                          |
|                                                                       |
| [/// Shoot missiles with an interval]{dir="ltr"}                      |
|                                                                       |
| [/// \</summary\>]{dir="ltr"}                                         |
|                                                                       |
| [private IEnumerator ShootMissiles()]{dir="ltr"}                      |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ for (int i = 0; i \< Tower.ProjectileSpawns.Length;                 |
| i++)]{dir="ltr"}                                                      |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // First child in projectile spawn will be missile game             |
| object]{dir="ltr"}                                                    |
|                                                                       |
| [ var missile = Tower.ProjectileSpawns\[i\].GetChild(0);]{dir="ltr"}  |
|                                                                       |
| [ missile.SendMessage(\"OnEject\");]{dir="ltr"}                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Continue coroutine after eject interval]{dir="ltr"}              |
|                                                                       |
| [ yield return new WaitForSeconds(EjectInterval);]{dir="ltr"}         |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Reload and invoke this method again after given                  |
| cooldown]{dir="ltr"}                                                  |
|                                                                       |
| [ Reload();]{dir="ltr"}                                               |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ yield return new WaitForSeconds(Cooldown);]{dir="ltr"}              |
|                                                                       |
| [ yield return ShootMissiles();]{dir="ltr"}                           |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[When all of the spawn slots of the missile launcher are empty, Reload()
will be called. The tower will instantiate new missiles in its spawn
slots.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                          |
|                                                                       |
| [/// Spawns a new missile where the slot is empty]{dir="ltr"}         |
|                                                                       |
| [/// \</summary\>]{dir="ltr"}                                         |
|                                                                       |
| [private void Reload()]{dir="ltr"}                                    |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ foreach (var spawn in Tower.ProjectileSpawns)]{dir="ltr"}           |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ if (spawn.childCount \> 0)]{dir="ltr"}                              |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ continue;]{dir="ltr"}                                               |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ Instantiate(Projectile, spawn.position, spawn.rotation,             |
| spawn);]{dir="ltr"}                                                   |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[Projectiles]{dir="ltr"}
========================

[Light Sky Defense has three different projectiles: EnemyBullet,
EnemyJammerBullet and the Missile. Most projectiles are used to deal
damage to either towers or enemies, but some are used to provide other
effects.]{dir="ltr"}

[EnemyBullet]{dir="ltr"}
------------------------

[The EnemyBullet is a projectile fired by the Shooter Enemy. It's a
regular bullet with no extra effects.]{dir="ltr"}

[]{dir="ltr"}

![](media/image25.png){width="3.3229166666666665in"
height="2.78125in"}[\
]{dir="ltr"}

[]{dir="ltr"}

[The following components are added to the EnemyBullet
prefab:]{dir="ltr"}

[]{dir="ltr"}

  **[Component]{dir="ltr"}**             **[Type]{dir="ltr"}**    **[Used for]{dir="ltr"}**
  -------------------------------------- ------------------------ -------------------------------------------------------
  [Rigidbody]{dir="ltr"}                 [Rigidbody]{dir="ltr"}   [To give it collision possibilities]{dir="ltr"}
  [Linear Bullet Behaviour]{dir="ltr"}   [Script]{dir="ltr"}      [Describes the bullet behaviour]{dir="ltr"}
  [Sphere Collider]{dir="ltr"}           [Collider]{dir="ltr"}    [Used for collision detection with towers]{dir="ltr"}

[]{dir="ltr"}

[The following properties can be configured in the Linear Bullet
Behaviour script:]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**    **[Type]{dir="ltr"}**   **[Description]{dir="ltr"}**
  ---------------------------- ----------------------- ------------------------------------------------------------------
  [Bullet Damage]{dir="ltr"}   [float]{dir="ltr"}      [The damage the bullet does]{dir="ltr"}
  [Time Alive]{dir="ltr"}      [float]{dir="ltr"}      [The time the bullet lives before it destroys itself]{dir="ltr"}

[]{dir="ltr"}

[EnemyJammerBullet]{dir="ltr"}
------------------------------

[The EnemyJammerBullet is a projectile fired by the JammerEnemy. It is
similar to the regular EnemyBullet in almost every way, except that it
deals no damage, but provides a paralyzing effect.]{dir="ltr"}

[]{dir="ltr"}

![](media/image4.png){width="3.2941863517060366in"
height="3.0468755468066493in"}[]{dir="ltr"}

[]{dir="ltr"}

[To make this happen, there's a script called "EnemyJammerProjectile"
attached to the JammerBullet. This script takes a unique property,
called JamTime. The value given to this property is the time a turret
remains in the Jam State after getting hit.]{dir="ltr"}

[Missile (Prefab)]{dir="ltr"}
-----------------------------

[The missile is more complex than the other two projectile types. It
deals area of effect damage and it's homing.]{dir="ltr"}

[]{dir="ltr"}

![](media/image2.png){width="4.21875in"
height="2.1458333333333335in"}[]{dir="ltr"}

[]{dir="ltr"}

[The following components are given to the Missile:]{dir="ltr"}

[]{dir="ltr"}

  **[Component]{dir="ltr"}**       **[Type]{dir="ltr"}**         **[Used for]{dir="ltr"}**
  -------------------------------- ----------------------------- --------------------------------------------------------------
  [Sphere Collider]{dir="ltr"}     [Collider]{dir="ltr"}         [Used to detect collision]{dir="ltr"}
  [Rigidbody]{dir="ltr"}           [Rigidbody]{dir="ltr"}        [Used for collision effects]{dir="ltr"}
  [Trail Renderer]{dir="ltr"}      [Trail Renderer]{dir="ltr"}   [Used to draw the trail that follows the missile]{dir="ltr"}
  [Missile Behaviour]{dir="ltr"}   [Script]{dir="ltr"}           [Used to make the missile behave like it should]{dir="ltr"}

[]{dir="ltr"}

[The most important component is the Missile Behaviour script. It
provides the missile with a blast effect that deals damage to multiple
enemies at once and with a homing effect that makes the way the missile
launcher rotates more logical.]{dir="ltr"}

[]{dir="ltr"}

[The Missile Behaviour script has the following properties:]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**           **[Type]{dir="ltr"}**         **[Description]{dir="ltr"}**
  ----------------------------------- ----------------------------- ---------------------------------------------------------------------------------------
  [Damage Curve]{dir="ltr"}           [AnimationCurve]{dir="ltr"}   [A curve showing how much damage is dealt to enemies at specific ranges.]{dir="ltr"}
  [Explosion Power]{dir="ltr"}        [float]{dir="ltr"}            [Determines the knockback done by the missiles.]{dir="ltr"}
  [Explosion Range]{dir="ltr"}        [float]{dir="ltr"}            [Determines how far enemies can get hit by a missile explosion.]{dir="ltr"}
  [Explosion Effect]{dir="ltr"}       [ParticleSystem]{dir="ltr"}   [The effect that is shown when a missile explodes.]{dir="ltr"}
  [Detection Range]{dir="ltr"}        [float]{dir="ltr"}            [The range at which a missile can detect an enemy]{dir="ltr"}
  [Collision Layer Mask]{dir="ltr"}   [LayerMask]{dir="ltr"}        [The layer of gameobjects the missile will detect]{dir="ltr"}
  [Time Alive]{dir="ltr"}             [float]{dir="ltr"}            [The time it takes before the missile destroys itself if nothing gets hit]{dir="ltr"}
  [Eject Force]{dir="ltr"}            [float]{dir="ltr"}            [The power at which the missile gets shot out of the tower]{dir="ltr"}
  [Max Speed]{dir="ltr"}              [float]{dir="ltr"}            [The speed at which the missile flies towards its target]{dir="ltr"}

[]{dir="ltr"}

### [Homing]{dir="ltr"}

[After a missile gets ejected, it flies in a straight line at high speed
for a second. After that, it will lock itself onto a target in its
detection range and curve towards it. This way, no matter where the
launcher ejects the missile, it will almost always find a target and hit
it.]{dir="ltr"}

[]{dir="ltr"}

+-------------------------------------------------------------------------+
| [public void FixedUpdate()]{dir="ltr"}                                  |
|                                                                         |
| [{]{dir="ltr"}                                                          |
|                                                                         |
| [ if (\_target == null)]{dir="ltr"}                                     |
|                                                                         |
| [{]{dir="ltr"}                                                          |
|                                                                         |
| [ var collider = FindClosestTarget(]{dir="ltr"}                         |
|                                                                         |
| [ \_missile.transform.position,]{dir="ltr"}                             |
|                                                                         |
| [ \_missile.DetectionRange,]{dir="ltr"}                                 |
|                                                                         |
| [ \_missile.CollisionLayerMask]{dir="ltr"}                              |
|                                                                         |
| [);]{dir="ltr"}                                                         |
|                                                                         |
| []{dir="ltr"}                                                           |
|                                                                         |
| [ \_target = collider?.transform;]{dir="ltr"}                           |
|                                                                         |
| [}]{dir="ltr"}                                                          |
|                                                                         |
| []{dir="ltr"}                                                           |
|                                                                         |
| [ var direction = \_target != null]{dir="ltr"}                          |
|                                                                         |
| [? Vector3.Normalize(]{dir="ltr"}                                       |
|                                                                         |
| [\_target.transform.position - \_missile.transform.position]{dir="ltr"} |
|                                                                         |
| [)]{dir="ltr"}                                                          |
|                                                                         |
| [: \_rigidbody.transform.forward;]{dir="ltr"}                           |
|                                                                         |
| []{dir="ltr"}                                                           |
|                                                                         |
| [ // Make missile overshoot]{dir="ltr"}                                 |
|                                                                         |
| [ var force = direction \* \_missile.MaxSpeed;]{dir="ltr"}              |
|                                                                         |
| []{dir="ltr"}                                                           |
|                                                                         |
| [ \_rigidbody.AddForce(force, ForceMode.Acceleration);]{dir="ltr"}      |
|                                                                         |
| [ \_missile.transform.rotation = Quaternion.LookRotation(]{dir="ltr"}   |
|                                                                         |
| [\_rigidbody.velocity]{dir="ltr"}                                       |
|                                                                         |
| [);]{dir="ltr"}                                                         |
|                                                                         |
| [}]{dir="ltr"}                                                          |
|                                                                         |
| []{dir="ltr"}                                                           |
+-------------------------------------------------------------------------+

### [Explosion]{dir="ltr"}

[Whenever a missile hits a target, it will explode in a wide radius,
dealing damage to multiple targets in its explosion range. This damage
is based on the configuration in the damage curve property. The closer
the target is, the more damage it receives, up to a maximum. After
getting hit, an explosion effect is shown and a force is applied using
AddExplosion force.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [public void OnCollisionEnter(Collision collision)]{dir="ltr"}        |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // Create new explosion effect instance on missile                  |
| position]{dir="ltr"}                                                  |
|                                                                       |
| [ MonoBehaviour.Instantiate(]{dir="ltr"}                              |
|                                                                       |
| [ \_missile.ExplosionEffect,]{dir="ltr"}                              |
|                                                                       |
| [ \_missile.transform.position,]{dir="ltr"}                           |
|                                                                       |
| [ \_missile.transform.rotation]{dir="ltr"}                            |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var colliders = Physics.OverlapSphere(]{dir="ltr"}                  |
|                                                                       |
| [ \_missile.transform.position,]{dir="ltr"}                           |
|                                                                       |
| [ \_missile.ExplosionRange,]{dir="ltr"}                               |
|                                                                       |
| [(int)\_missile.CollisionLayerMask]{dir="ltr"}                        |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ foreach (var collider in colliders)]{dir="ltr"}                     |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var damageable = collider.GetComponent\<Damageable\>();]{dir="ltr"} |
|                                                                       |
| [ var rigidbody = collider.GetComponent\<Rigidbody\>();]{dir="ltr"}   |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var enemyDistance = Vector3.Distance(]{dir="ltr"}                   |
|                                                                       |
| [ \_missile.transform.position,]{dir="ltr"}                           |
|                                                                       |
| [ collider.transform.position]{dir="ltr"}                             |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var damage =                                                        |
| \_missile.DamageCurve.Evaluate(enemyDistance);]{dir="ltr"}            |
|                                                                       |
| [ damageable?.UpdateHealth(-damage);]{dir="ltr"}                      |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ rigidbody?.AddExplosionForce(]{dir="ltr"}                           |
|                                                                       |
| [ \_missile.ExplosionPower,]{dir="ltr"}                               |
|                                                                       |
| [ \_missile.transform.position,]{dir="ltr"}                           |
|                                                                       |
| [ \_missile.ExplosionRange]{dir="ltr"}                                |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ MonoBehaviour.Destroy(\_missile.gameObject);]{dir="ltr"}            |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[Enemy]{dir="ltr"}
==================

[The game has multiple enemy types of which multiple are spawned during
the waves. All enemies are prefab-variants of BaseEnemy. An enemy will
follow the path and can die from damage inflicted by towers. When it
does its die effect will be played, the player will get its score and a
credit will be spawned. If an enemy reaches the end of the path it will
fly into planet earth. The enemy will implode and the player will lose a
life point when the enemy collides with planet earth.]{dir="ltr"}

[]{dir="ltr"}

[The BaseEnemy has the following components:]{dir="ltr"}

[]{dir="ltr"}

  **[Component]{dir="ltr"}**                  **[Description]{dir="ltr"}**
  ------------------------------------------- ---------------------------------------------------------------------------------
  [Rigidbody]{dir="ltr"}                      [Physics dynamics (moving, knockback effect, hit detection).]{dir="ltr"}
  [Enemy (Script)]{dir="ltr"}                 [Store the Die and Heal effects.]{dir="ltr"}
  [Damageable (Script)]{dir="ltr"}            [Store health.]{dir="ltr"}
  [Spawn Credit On Die (Script)]{dir="ltr"}   [The credit to drop when the enemy is killed.]{dir="ltr"}
  [Update Score On Die (Script)]{dir="ltr"}   [Reward the player with score when the enemy is killed.]{dir="ltr"}
  [RigidBody Steering (Script)]{dir="ltr"}    [The enemy follows the path according to the behaviour the weights.]{dir="ltr"}

[]{dir="ltr"}

[Enemy (Script)]{dir="ltr"}
---------------------------

[The enemy script stores the explosion and effects and the damage it
will inflict.]{dir="ltr"}

[]{dir="ltr"}

  **[Properties]{dir="ltr"}**            **[Description]{dir="ltr"}**
  -------------------------------------- ----------------------------------------------------------------------------------
  [Explode Pitch]{dir="ltr"}             [.The base pitch of the explode sound effect.]{dir="ltr"}
  [Explode Pitch Variation]{dir="ltr"}   [How much the pitch variates between explosions.]{dir="ltr"}
  [Explode Effect]{dir="ltr"}            [The particle system for the visual explosion effect.]{dir="ltr"}
  [Explode Sound]{dir="ltr"}             [The sound played on explosion.]{dir="ltr"}
  [Heal Effect]{dir="ltr"}               [The particle system for the visual healing effect.]{dir="ltr"}
  [Damage]{dir="ltr"}                    [Amount of damage that the enemy will inflict to the player's lives.]{dir="ltr"}

[Rigidbody Steering (Script)]{dir="ltr"}
----------------------------------------

[The rigidbody steering script is used to apply a steering behaviour to
a rigidbody.]{dir="ltr"}

[]{dir="ltr"}

![](media/image28.png){width="6.270833333333333in"
height="3.736111111111111in"}[]{dir="ltr"}

[]{dir="ltr"}

  **[Properties]{dir="ltr"}**                        **[Description]{dir="ltr"}**
  -------------------------------------------------- ----------------------------------------------------------------------------------------------
  [Max Speed]{dir="ltr"}                             [The max speed which will be passed to the steering behaviours in from the list.]{dir="ltr"}
  [Weighted Steering Behaviours (List)]{dir="ltr"}   [How much the pitch variates between explosions.]{dir="ltr"}

[]{dir="ltr"}

![](media/image9.png){width="6.270833333333333in"
height="2.9583333333333335in"}[]{dir="ltr"}

[The weighted steering behaviours are calculated and then combined using
WeightedTruncatedRunningSumWithPrioritization. The value returned from
that function is than used as the steering force.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [var steeringForces =]{dir="ltr"}                                     |
|                                                                       |
| [new Tuple\<Vector3,                                                  |
| float\>\[WeightedSteeringBehaviours.Length\];]{dir="ltr"}             |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [for (int i = 0; i \< WeightedSteeringBehaviours.Length;              |
| i++)]{dir="ltr"}                                                      |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var weightedSteeringBehaviour =                                     |
| WeightedSteeringBehaviours\[i\];]{dir="ltr"}                          |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ steeringForces\[i\] = new Tuple\<Vector3, float\>(]{dir="ltr"}      |
|                                                                       |
| [                                                                     |
| weightedSteeringBehaviour.SteeringBehaviour.Calculate(gameObject),]{d |
| ir="ltr"}                                                             |
|                                                                       |
| [ weightedSteeringBehaviour.Weight]{dir="ltr"}                        |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [var steeringForce =                                                  |
| WeightedTruncatedRunningSumWithPrioritization.Calculate(]{dir="ltr"}  |
|                                                                       |
| [ steeringForces,]{dir="ltr"}                                         |
|                                                                       |
| [ MaxSpeed]{dir="ltr"}                                                |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [\_rigidbody.AddForce(steeringForce);]{dir="ltr"}                     |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[Besides applying a steering force to the objects the RigidbodySteering
script also rotates the object to the velocities direction.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [// No need to update rotation]{dir="ltr"}                            |
|                                                                       |
| [if (\_rigidbody.velocity == Vector3.zero)]{dir="ltr"}                |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ return;]{dir="ltr"}                                                 |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Rotate enemy towards velocity direction]{dir="ltr"}               |
|                                                                       |
| [var rotation =                                                       |
| Quaternion.LookRotation(\_rigidbody.velocity);]{dir="ltr"}            |
|                                                                       |
| [\_rigidbody.MoveRotation(rotation);]{dir="ltr"}                      |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[The Steering Behaviours list has the following behaviours in the
BaseEnemy prefab.]{dir="ltr"}

[]{dir="ltr"}

  **[Behaviour]{dir="ltr"}**     **[Weight]{dir="ltr"}**   **[Description]{dir="ltr"}**
  ------------------------------ ------------------------- -------------------------------------------------------------------------------------------
  [EnemyFollowPath]{dir="ltr"}   [1]{dir="ltr"}            [The enemy will follow the path from the first waypoint to the last waypoint.]{dir="ltr"}
  [EnemyFlocking]{dir="ltr"}     [0.2]{dir="ltr"}          [The enemies will flock together when they are in a certain radius.]{dir="ltr"}
  [EnemyWander]{dir="ltr"}       [0.5]{dir="ltr"}          [The enemies will wander individually.]{dir="ltr"}

### [FollowPath (Script)]{dir="ltr"}

[The FollowPath script is used to make a gameobject follow the path
linearly from the beginning to the end.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**     **[Type]{dir="ltr"}**   **[Default value]{dir="ltr"}**
  ----------------------------- ----------------------- --------------------------------
  [HandoverMargin]{dir="ltr"}   [Float]{dir="ltr"}      [0.0000001f]{dir="ltr"}
  [FollowSpeed]{dir="ltr"}      [Float]{dir="ltr"}      [0.1f]{dir="ltr"}

[]{dir="ltr"}

[The path is build out of waypoints. Even though the waypoints have
capsule colliders, these colliders are not used for traversing the path.
The waypoints can be accessed by the index operator
(Path.Instance\[index\]). The progress on the path (between waypoints)
is tracked in a separate Vector3 \_progress member. And will move
towards the waypoint until it's distance is closer than HandoverMargin.
At this point the index will be updated and it will move towards the
next waypoint. The yellow dot in the following image represents the
\_progress.]{dir="ltr"}

[]{dir="ltr"}

![](media/image8.png){width="3.65625in"
height="2.2291666666666665in"}[]{dir="ltr"}

[]{dir="ltr"}

[When the active waypoint is the last waypoint in the path we use the
EndGoalInstance position. This will make the enemies fly into
EndGoalInstance.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private void UpdateProgression()]{dir="ltr"}                         |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var targetWaypoint =                                                |
| Path.Instance\[\_currentWaypointIndex\];]{dir="ltr"}                  |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ if (\_currentWaypointIndex \>=                                      |
| Path.Instance.WaypointCount)]{dir="ltr"}                              |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ targetWaypoint = Path.Instance.EndGoalInstance;]{dir="ltr"}         |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Move path progression point towards next waypoint]{dir="ltr"}    |
|                                                                       |
| [ \_progress = Vector3.MoveTowards(]{dir="ltr"}                       |
|                                                                       |
| [ \_progress,]{dir="ltr"}                                             |
|                                                                       |
| [ targetWaypoint.transform.position,]{dir="ltr"}                      |
|                                                                       |
| [ FollowSpeed \* Time.deltaTime]{dir="ltr"}                           |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var distanceToTargetWaypoint = Vector3.Distance(]{dir="ltr"}        |
|                                                                       |
| [ \_progress,]{dir="ltr"}                                             |
|                                                                       |
| [ targetWaypoint.transform.position]{dir="ltr"}                       |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ if (distanceToTargetWaypoint \<= HandoverMargin)]{dir="ltr"}        |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ \_currentWaypointIndex++;]{dir="ltr"}                               |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[In calculate the vector to \_progress is used as the steering force
like seek.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [public override Vector3 Calculate(GameObject gameObject)]{dir="ltr"} |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ UpdateProgression();]{dir="ltr"}                                    |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Seek to path point]{dir="ltr"}                                   |
|                                                                       |
| [ return \_progress - gameObject.transform.position;]{dir="ltr"}      |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

### [Flocking (Script)]{dir="ltr"}

[The flocking behaviour is a combination of the separation, alignment,
and cohesion steering behaviour. For the flocking behaviour we need the
neighbours of enemy that we want to calculate a steering force for. The
CohesionRadius is used for the overlap sphere radius, the separation
radius is derived from the CohesionRadius. This means that the
CohesionRadius is always larger than the radius of the other
behaviours.]{dir="ltr"}

[]{dir="ltr"}

+------------------------------------------------------+
| [var neighbours = Physics.OverlapSphere(]{dir="ltr"} |
|                                                      |
| [ gameObject.transform.position,]{dir="ltr"}         |
|                                                      |
| [ CohesionRadius,]{dir="ltr"}                        |
|                                                      |
| [ FlockingLayerMask]{dir="ltr"}                      |
|                                                      |
| [);]{dir="ltr"}                                      |
+------------------------------------------------------+

[]{dir="ltr"}

[The CohesionRadius is visualized using the blue sphere gizmo. The
alignment behaviour will use all neighbours in the cohesion sphere. The
separation behaviour is visualized using the yellow sphere
gizmo.]{dir="ltr"}

[]{dir="ltr"}

![](media/image28.png){width="3.119792213473316in"
height="2.416981627296588in"}[]{dir="ltr"}

[]{dir="ltr"}

[We combine these behaviours in one class to greatly reduce the number
of cycles that are required to calculate the flocking steering force.
Instead of looping through each enemy in range for every behaviour we
just need to do it once.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [var cohesionForce = Vector3.zero;]{dir="ltr"}                        |
|                                                                       |
| [var alignmentForce = Vector3.zero;]{dir="ltr"}                       |
|                                                                       |
| [var separationForce = Vector3.zero;]{dir="ltr"}                      |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [var seperationCount = 0;]{dir="ltr"}                                 |
|                                                                       |
| [var neighbourCount = Math.Min(MaxNeighbours, neighbours.Length -     |
| 1);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [for (int i = 0; i \< neighbours.Length; i++)]{dir="ltr"}             |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var neighbour = neighbours\[i\];]{dir="ltr"}                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[In the for loop we only use a maximum amount of enemies to make sure
that calculating the steering force doesn't have too much of an impact.
The neighbours list will also contain the enemy itself, we ignore that
item in the neighbour list.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [// Stop aggregating more forces to minimize performance              |
| impact]{dir="ltr"}                                                    |
|                                                                       |
| [if (i \> MaxNeighbours)]{dir="ltr"}                                  |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ break;]{dir="ltr"}                                                  |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [if (neighbour.transform == gameObject.transform)]{dir="ltr"}         |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ continue;]{dir="ltr"}                                               |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[If we do get past the if statement above we start adding to the
steering forces.]{dir="ltr"}

[]{dir="ltr"}

[The separationForce should only be adjusted when a neighbour is very
close to gameObject. Finding neighbours is by far the heaviest operation
in this steering behaviour, thus we check if neighbours are in range
manually. This is much faster than calling Physics.OverlapSphere
twice.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [// Calculate the direction to the neighbour]{dir="ltr"}              |
|                                                                       |
| [ var toNeighbour =]{dir="ltr"}                                       |
|                                                                       |
| [neighbour.transform.position -]{dir="ltr"}                           |
|                                                                       |
| [gameObject.transform.position;]{dir="ltr"}                           |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Sum neighbour positions to calculate the cohesion                |
| target]{dir="ltr"}                                                    |
|                                                                       |
| [ cohesionForce += neighbour.transform.position;]{dir="ltr"}          |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Sum the normalize direction (heading) for the alignment          |
| behaviour]{dir="ltr"}                                                 |
|                                                                       |
| [ alignmentForce += Vector3.Normalize(toNeighbour);]{dir="ltr"}       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Check whether current neighbour should be considered]{dir="ltr"} |
|                                                                       |
| [// for the separation force]{dir="ltr"}                              |
|                                                                       |
| [ if (toNeighbour.sqrMagnitude \<=                                    |
| \_seperationRadiusSquared)]{dir="ltr"}                                |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // Scale the force inversely proportional to the]{dir="ltr"}        |
|                                                                       |
| [// objects distance from its neighbor]{dir="ltr"}                    |
|                                                                       |
| [ separationForce +=]{dir="ltr"}                                      |
|                                                                       |
| [Vector3.Normalize(-toNeighbour) / toNeighbour.magnitude;]{dir="ltr"} |
|                                                                       |
| [ seperationCount++;]{dir="ltr"}                                      |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[To get the final value for each behaviour we: subtract the position
from the cohesionForce, divide the alignmentForce by the neighbourCount
and divide the separationForce by separationCount.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [// Calculate direction towards average position for the cohesion     |
| force]{dir="ltr"}                                                     |
|                                                                       |
| [cohesionForce = gameObject.transform.position -                      |
| cohesionForce;]{dir="ltr"}                                            |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Calculate average heading]{dir="ltr"}                             |
|                                                                       |
| [alignmentForce /= neighbourCount;]{dir="ltr"}                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Calculate average distance from neighbours that are]{dir="ltr"}   |
|                                                                       |
| [// considered for separation behaviour]{dir="ltr"}                   |
|                                                                       |
| [separationForce /= seperationCount \> 0 ? seperationCount :          |
| 1;]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[When all forces are calculated we calculate a final steering force
using WeightedTruncatedRunningSumWithPrioritization using the configured
weights.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [return                                                               |
| WeightedTruncatedRunningSumWithPrioritization.Calculate(]{dir="ltr"}  |
|                                                                       |
| [ new Tuple\<Vector3, float\>\[\] {]{dir="ltr"}                       |
|                                                                       |
| [ new Tuple\<Vector3, float\>(separationForce,                        |
| SeparationWeight),]{dir="ltr"}                                        |
|                                                                       |
| [ new Tuple\<Vector3, float\>(alignmentForce,                         |
| AlignmentWeight),]{dir="ltr"}                                         |
|                                                                       |
| [ new Tuple\<Vector3, float\>(cohesionForce,                          |
| CohesionWeight),]{dir="ltr"}                                          |
|                                                                       |
| [},]{dir="ltr"}                                                       |
|                                                                       |
| [ MaxSpeed]{dir="ltr"}                                                |
|                                                                       |
| [);]{dir="ltr"}                                                       |
+-----------------------------------------------------------------------+

### [Wander (Script)]{dir="ltr"}

[The wander behaviour works by generating a place (small red gizmo) on
the perimeter of a sphere (teal sphere gizmo) in front of the
gameobject. the gameobject is steered towards this position because this
position changes randomly the gameobject will wander.]{dir="ltr"}

[]{dir="ltr"}

![](media/image8.png){width="3.65625in"
height="2.2291666666666665in"}[]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**     **[Type]{dir="ltr"}**   **[Default value]{dir="ltr"}**
  ----------------------------- ----------------------- --------------------------------
  [WanderRadius]{dir="ltr"}     [Float]{dir="ltr"}      [0.2f]{dir="ltr"}
  [WanderDistance]{dir="ltr"}   [Float]{dir="ltr"}      [0.2f]{dir="ltr"}
  [WanderJitter]{dir="ltr"}     [Float]{dir="ltr"}      [5f]{dir="ltr"}

[]{dir="ltr"}

[Changing the target on the wander sphere. (Red dot on the edge of green
sphere in image).]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [var addToPerimeter = Random.onUnitSphere \* (WanderJitter \*         |
| Time.deltaTime);]{dir="ltr"}                                          |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Update local wander target]{dir="ltr"}                            |
|                                                                       |
| [ \_wanderTarget += addToPerimeter;]{dir="ltr"}                       |
|                                                                       |
| [\_wanderTarget = Vector3.Normalize(\_wanderTarget);]{dir="ltr"}      |
|                                                                       |
| [\_wanderTarget \*= WanderRadiuse;]{dir="ltr"}                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[Placing the sphere in front of the gameObject.]{dir="ltr"}

[]{dir="ltr"}

+---------------------------------------------------------------+
| [var wanderSpherePosition =]{dir="ltr"}                       |
|                                                               |
| [ gameObject.transform.position +]{dir="ltr"}                 |
|                                                               |
| [ gameObject.transform.forward \* WanderDistance;]{dir="ltr"} |
+---------------------------------------------------------------+

[]{dir="ltr"}

[Seek towards the point wander target in world space.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [var worldSpaceTarget = wanderSpherePosition +                        |
| \_wanderTarget;]{dir="ltr"}                                           |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [return worldSpaceTarget - gameObject.transform.position;]{dir="ltr"} |
+-----------------------------------------------------------------------+

[Enemy abilities]{dir="ltr"}
----------------------------

[Other than the regular enemy that have no special behaviour, there are
five other enemy types, each with their own special
abilities.]{dir="ltr"}

[]{dir="ltr"}

  **[Enemy]{dir="ltr"}**         **[Behaviour/Abilities]{dir="ltr"}**
  ------------------------------ ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  [Split Enemy]{dir="ltr"}       [Upon being destroyed, a split enemy splits and creates multiple weaker instances of himself. These instances have a much lower health pool than the original.]{dir="ltr"}
  [AoE Heal Enemy]{dir="ltr"}    [Upon spawning, this enemy will heal all enemies in a small area around him for a small amount every few seconds.]{dir="ltr"}
  [Self Heal Enemy]{dir="ltr"}   [This enemy will heal itself for a substantial amount once its health pool reaches a certain threshold. This heal will incur a large cooldown on itself.]{dir="ltr"}
  [Shooter Enemy]{dir="ltr"}     [Basically a tower for the enemy. It shoots at a random tower every few seconds that's in a specified radius around the enemy. The bullets shot by the enemy can collide with the tower's bullets, destroying both in the process.]{dir="ltr"}
  [Jammer Enemy]{dir="ltr"}      [A jammer enemy shoots bullets at towers that cause the towers to become paralyzed for a few seconds, rendering them unable to shoot.]{dir="ltr"}

[]{dir="ltr"}

[These enemies extend the base enemy, we use small components to add
additional abilities to enemy prefabs. The following scripts are used to
implement additional enemy abilities:]{dir="ltr"}

[]{dir="ltr"}

  **[Script]{dir="ltr"}**             **[Description]{dir="ltr"}**
  ----------------------------------- ------------------------------------------------------------------------------------------------------------------------------
  [EnemyAoEHeal]{dir="ltr"}           [Provides AoE Heal enemies with the HealEnemies ability.]{dir="ltr"}
  [EnemySelfHeal]{dir="ltr"}          [Provides Self Heal enemies with the SelfHeal ability.]{dir="ltr"}
  [EnemySplit]{dir="ltr"}             [Provides Split Enemies with the ability to split themselves upon dying.]{dir="ltr"}
  [FindTarget]{dir="ltr"}             [Gives Shooter and Jammer enemies a way to locate the nearest target within their radius.]{dir="ltr"}
  [ShootAtTarget]{dir="ltr"}          [Gives Shooter and Jammer enemies a way to shoot at the located target within their radius.]{dir="ltr"}
  [ShootMissileAtTarget]{dir="ltr"}   [Specific to the Bugzooka (first boss), this script gives it the ability to fire its missiles in an optimal way.]{dir="ltr"}

[]{dir="ltr"}

[The following chapters will explain these behaviours.]{dir="ltr"}

### [EnemyAoEHeal(Script)]{dir="ltr"}

[Will heal enemies in its radius that this script is attached to. The
following properties can be configured on this component.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**   **[Type]{dir="ltr"}**    **[Default value]{dir="ltr"}**
  --------------------------- ------------------------ --------------------------------
  [HealAmount]{dir="ltr"}     [Float]{dir="ltr"}       [0.1]{dir="ltr"}
  [HealRadius]{dir="ltr"}     [Float]{dir="ltr"}       [0.2]{dir="ltr"}
  [HealInterval]{dir="ltr"}   [Float]{dir="ltr"}       [3]{dir="ltr"}
  [LayerBitMask]{dir="ltr"}   [LayerMask]{dir="ltr"}   [Layer.Enemies]{dir="ltr"}

[]{dir="ltr"}

[When the EnemyAoEHeal script is enabled, HealEnemies gets invoked on
the defined HealInterval.]{dir="ltr"}

[]{dir="ltr"}

+------------------------------------------------------------------+
| [private void OnEnable()]{dir="ltr"}                             |
|                                                                  |
| [{]{dir="ltr"}                                                   |
|                                                                  |
| [ InvokeRepeating(\"HealEnemies\", 0, HealInterval);]{dir="ltr"} |
|                                                                  |
| [}]{dir="ltr"}                                                   |
|                                                                  |
| []{dir="ltr"}                                                    |
|                                                                  |
| [private void OnDisable()]{dir="ltr"}                            |
|                                                                  |
| [{]{dir="ltr"}                                                   |
|                                                                  |
| [ CancelInvoke();]{dir="ltr"}                                    |
|                                                                  |
| [}]{dir="ltr"}                                                   |
+------------------------------------------------------------------+

[]{dir="ltr"}

[When HealEnemies is invoked, all enemies in its range (HealRadius) are
healed by the given HealAmount.]{dir="ltr"}

[]{dir="ltr"}

+--------------------------------------------------------------------+
| [private void HealEnemies()]{dir="ltr"}                            |
|                                                                    |
| [{]{dir="ltr"}                                                     |
|                                                                    |
| [ var enemiesInRange = Physics.OverlapSphere(]{dir="ltr"}          |
|                                                                    |
| [ transform.position,]{dir="ltr"}                                  |
|                                                                    |
| [ HealRadius,]{dir="ltr"}                                          |
|                                                                    |
| [(int)DetectionLayerMask]{dir="ltr"}                               |
|                                                                    |
| [);]{dir="ltr"}                                                    |
|                                                                    |
| []{dir="ltr"}                                                      |
|                                                                    |
| [ foreach (Collider enemy in enemiesInRange)]{dir="ltr"}           |
|                                                                    |
| [{]{dir="ltr"}                                                     |
|                                                                    |
| [ var damageable = Enemy.GetComponent\<Damageable\>();]{dir="ltr"} |
|                                                                    |
| [ damageable.UpdateHealth(HealAmount);]{dir="ltr"}                 |
|                                                                    |
| [}]{dir="ltr"}                                                     |
|                                                                    |
| [}]{dir="ltr"}                                                     |
+--------------------------------------------------------------------+

[]{dir="ltr"}

### [EnemySelfHeal (Script)]{dir="ltr"}

[Will heal enemy that this script is attached to. The following
properties can be configured on this component.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**   **[Type]{dir="ltr"}**   **[Default value]{dir="ltr"}**
  --------------------------- ----------------------- --------------------------------
  [HealCooldown]{dir="ltr"}   [Float]{dir="ltr"}      [5]{dir="ltr"}
  [HealAmount]{dir="ltr"}     [Float]{dir="ltr"}      [0.3]{dir="ltr"}

[]{dir="ltr"}

[Will heal enemy that this script is attached to by configured
HealAmount every HealCooldown. When enemy is not damaged it will try to
heal every second instead.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private void HealWithInterval()]{dir="ltr"}                          |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ if (\_damageable.Health \< \_damageable.MaxHealth -                 |
| HealAmount)]{dir="ltr"}                                               |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ \_damageable.UpdateHealth(HealAmount);]{dir="ltr"}                  |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ Invoke(\"HealWithInterval\", HealCooldown);]{dir="ltr"}             |
|                                                                       |
| [ return;]{dir="ltr"}                                                 |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ Invoke(\"HealWithInterval\", \_checkTimer);]{dir="ltr"}             |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

### [EnemySplit (Script)]{dir="ltr"}

[The OnDie script gets called right before an enemy dies. This script
will spawn instances of an enemy prefab.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**   **[Type]{dir="ltr"}**     **[Default value]{dir="ltr"}**
  --------------------------- ------------------------- --------------------------------
  [NewEnemy]{dir="ltr"}       [GameObject]{dir="ltr"}   [-]{dir="ltr"}
  [SplitAmount]{dir="ltr"}    [Int]{dir="ltr"}          [2]{dir="ltr"}

[]{dir="ltr"}

[It will instantiate new enemies based on the SplitAmount
value.]{dir="ltr"}

[]{dir="ltr"}

+------------------------------------------------------+
| [void OnDie()]{dir="ltr"}                            |
|                                                      |
| [{]{dir="ltr"}                                       |
|                                                      |
| [ for (int i = 0; i \< SplitAmount; i++)]{dir="ltr"} |
|                                                      |
| [{]{dir="ltr"}                                       |
|                                                      |
| [ Instantiate(]{dir="ltr"}                           |
|                                                      |
| [ NewEnemy,]{dir="ltr"}                              |
|                                                      |
| [ transform.position,]{dir="ltr"}                    |
|                                                      |
| [ Random.rotation]{dir="ltr"}                        |
|                                                      |
| [);]{dir="ltr"}                                      |
|                                                      |
| [}]{dir="ltr"}                                       |
|                                                      |
| [}]{dir="ltr"}                                       |
+------------------------------------------------------+

[]{dir="ltr"}

### [FindTarget (Script)]{dir="ltr"}

[Will search each FixedUpdate if there is any tower in range. If there
is it will enable the HasTarget Script while setting the nearest target
in the Target field of that script.]{dir="ltr"}

[ ]{dir="ltr"}

  **[Property]{dir="ltr"}**   **[Type]{dir="ltr"}**         **[Default value]{dir="ltr"}**
  --------------------------- ----------------------------- --------------------------------
  [HasTarget]{dir="ltr"}      [EnableOnTarget]{dir="ltr"}   [-]{dir="ltr"}
  [Radius]{dir="ltr"}         [Float]{dir="ltr"}            [-]{dir="ltr"}
  [LayerBitMask]{dir="ltr"}   [LayerMask]{dir="ltr"}        [Layer.Towers]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [public Collider Find()]{dir="ltr"}                                   |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var TargetsInRange = Physics.OverlapSphere(]{dir="ltr"}             |
|                                                                       |
| [ transform.position,]{dir="ltr"}                                     |
|                                                                       |
| [ Radius,]{dir="ltr"}                                                 |
|                                                                       |
| [ LayerBitMask]{dir="ltr"}                                            |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Abort function when there isn\'t any target in the overlap       |
| sphere]{dir="ltr"}                                                    |
|                                                                       |
| [ if (TargetsInRange.Length \< 1)]{dir="ltr"}                         |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ return null;]{dir="ltr"}                                            |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Find target that is near this game object]{dir="ltr"}            |
|                                                                       |
| [ Collider nearestTarget = null;]{dir="ltr"}                          |
|                                                                       |
| [ float minimalDistance = float.MaxValue;]{dir="ltr"}                 |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ foreach (var target in TargetsInRange)]{dir="ltr"}                  |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var distance = Vector3.Distance(]{dir="ltr"}                        |
|                                                                       |
| [transform.position,]{dir="ltr"}                                      |
|                                                                       |
| [ Target.transform.position]{dir="ltr"}                               |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ if (distance \< minimalDistance)]{dir="ltr"}                        |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ minimalDistance = distance;]{dir="ltr"}                             |
|                                                                       |
| [ nearestTarget = target;]{dir="ltr"}                                 |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ return nearestTarget;]{dir="ltr"}                                   |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

### [ShootAtTarget (Script)]{dir="ltr"}

[Will shoot a bullet at its target every cooldown time. If its target is
no longer in range or dead it will enable FindTarget.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**      **[Type]{dir="ltr"}**     **[Default value]{dir="ltr"}**
  ------------------------------ ------------------------- --------------------------------
  [FindTarget]{dir="ltr"}        [FindTarget]{dir="ltr"}   [-]{dir="ltr"}
  [Projectile]{dir="ltr"}        [Rigidbody]{dir="ltr"}    [-]{dir="ltr"}
  [ProjectileSpawn]{dir="ltr"}   [Transform]{dir="ltr"}    [-]{dir="ltr"}
  [ProjectileSpeed]{dir="ltr"}   [Float]{dir="ltr"}        [1.5]{dir="ltr"}
  [Cooldown]{dir="ltr"}          [Float]{dir="ltr"}        [2]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

+--------------------------------------------------------------------+
| [private IEnumerator Shoot()]{dir="ltr"}                           |
|                                                                    |
| [{]{dir="ltr"}                                                     |
|                                                                    |
| [ if (!TargetInRange())]{dir="ltr"}                                |
|                                                                    |
| [{]{dir="ltr"}                                                     |
|                                                                    |
| [ FindTarget.enabled = true;]{dir="ltr"}                           |
|                                                                    |
| [ enabled = false;]{dir="ltr"}                                     |
|                                                                    |
| [ yield break;]{dir="ltr"}                                         |
|                                                                    |
| [}]{dir="ltr"}                                                     |
|                                                                    |
| []{dir="ltr"}                                                      |
|                                                                    |
| [ var projectile = Instantiate(]{dir="ltr"}                        |
|                                                                    |
| [ Projectile,]{dir="ltr"}                                          |
|                                                                    |
| [ ProjectileSpawn.position,]{dir="ltr"}                            |
|                                                                    |
| [ ProjectileSpawn.rotation]{dir="ltr"}                             |
|                                                                    |
| [);]{dir="ltr"}                                                    |
|                                                                    |
| []{dir="ltr"}                                                      |
|                                                                    |
| [ var direction =]{dir="ltr"}                                      |
|                                                                    |
| [Target.transform.position - ProjectileSpawn.position;]{dir="ltr"} |
|                                                                    |
| []{dir="ltr"}                                                      |
|                                                                    |
| [ projectile.AddForce(]{dir="ltr"}                                 |
|                                                                    |
| [ Vector3.Normalize(direction) \* ProjectileSpeed,]{dir="ltr"}     |
|                                                                    |
| [ ForceMode.VelocityChange]{dir="ltr"}                             |
|                                                                    |
| [);]{dir="ltr"}                                                    |
|                                                                    |
| []{dir="ltr"}                                                      |
|                                                                    |
| [ yield return new WaitForSeconds(Cooldown);]{dir="ltr"}           |
|                                                                    |
| [ yield return Shoot();]{dir="ltr"}                                |
|                                                                    |
| [}]{dir="ltr"}                                                     |
+--------------------------------------------------------------------+

[]{dir="ltr"}

### [ShootMissileAtTarget (Script)]{dir="ltr"}

[Will eject a missile every shootInverval. If its target is no longer in
range or dead it will enable FindTarget.]{dir="ltr"}

[]{dir="ltr"}

  **[Property]{dir="ltr"}**      **[Type]{dir="ltr"}**     **[Default value]{dir="ltr"}**
  ------------------------------ ------------------------- --------------------------------
  [FindTarget]{dir="ltr"}        [FindTarget]{dir="ltr"}   [-]{dir="ltr"}
  [Projectile]{dir="ltr"}        [Rigidbody]{dir="ltr"}    [-]{dir="ltr"}
  [ProjectileSpawn]{dir="ltr"}   [Transform]{dir="ltr"}    [-]{dir="ltr"}
  [ShootSpeed]{dir="ltr"}        [Float]{dir="ltr"}        [1.25]{dir="ltr"}
  [ShootInterval]{dir="ltr"}     [Float]{dir="ltr"}        [4]{dir="ltr"}
  [LoadUpTime]{dir="ltr"}        [Float]{dir="ltr"}        [3]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private IEnumerator Shoot()]{dir="ltr"}                              |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ yield return new WaitForSeconds(LoadUpTime);]{dir="ltr"}            |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var distanceToTarget = Vector3.Distance(]{dir="ltr"}                |
|                                                                       |
| [Target.transform.position,]{dir="ltr"}                               |
|                                                                       |
| [ transform.position]{dir="ltr"}                                      |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Switch to FindTarget when target does not exist                  |
| anymore]{dir="ltr"}                                                   |
|                                                                       |
| [ if (Target == null \|\| distanceToTarget \>                         |
| FindTarget.Radius)]{dir="ltr"}                                        |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ FindTarget.enabled = true;]{dir="ltr"}                              |
|                                                                       |
| [ enabled = false;]{dir="ltr"}                                        |
|                                                                       |
| [ yield break;]{dir="ltr"}                                            |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var newProjectile = Instantiate(]{dir="ltr"}                        |
|                                                                       |
| [ Projectile,]{dir="ltr"}                                             |
|                                                                       |
| [ ProjectileSpawn.position,]{dir="ltr"}                               |
|                                                                       |
| [ ProjectileSpawn.rotation]{dir="ltr"}                                |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ newProjectile.SendMessage(]{dir="ltr"}                              |
|                                                                       |
| [\"OnEject\",]{dir="ltr"}                                             |
|                                                                       |
| [ SendMessageOptions.RequireReceiver]{dir="ltr"}                      |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ newProjectile.velocity = ProjectileSpawn.forward \*                 |
| ShootSpeed;]{dir="ltr"}                                               |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ yield return new WaitForSeconds(ShootInterval);]{dir="ltr"}         |
|                                                                       |
| [ yield return Shoot();]{dir="ltr"}                                   |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[Controls]{dir="ltr"}
=====================

[The player has two HTC controllers. In the current version, the player
can use these controllers to.]{dir="ltr"}

[]{dir="ltr"}

-   [Collect pickups]{dir="ltr"}

-   [Place towers]{dir="ltr"}

-   [Remove towers]{dir="ltr"}

-   [Repair towers]{dir="ltr"}

-   [Open the pause menu]{dir="ltr"}

-   [Interact with the UI]{dir="ltr"}

[SteamVR Input System]{dir="ltr"}[^1][]{dir="ltr"}
--------------------------------------------------

[We use the SteamVR Input System to listen to actions from the
controllers. You need to register actions in the SteamVR Input
window.]{dir="ltr"}

[]{dir="ltr"}

![](media/image17.png){width="4.052083333333333in"
height="2.2916666666666665in"}[]{dir="ltr"}

[]{dir="ltr"}

[You'll see your registered actions when you open this
panel.]{dir="ltr"}

[]{dir="ltr"}

![](media/image39.png){width="6.270833333333333in"
height="4.291666666666667in"}[]{dir="ltr"}

### [Actions]{dir="ltr"}

[Eight actions in this list used by the SteamVR Interaction
System]{dir="ltr"}[^2][. These actions are required and cannot be
removed from the project. We'll talk more about the SteamVR Interaction
System in the next chapter.]{dir="ltr"}

[]{dir="ltr"}

  **[Action]{dir="ltr"}**          **[Type]{dir="ltr"}**   **[Description]{dir="ltr"}**
  -------------------------------- ----------------------- ----------------------------------------------------------------
  [Pose]{dir="ltr"}                [Pose]{dir="ltr"}       [Position, rotation, velocity and angular velocity]{dir="ltr"}
  [SkeletonLeftHand]{dir="ltr"}    [Skeleton]{dir="ltr"}   [Orientations for each bone in a hand]{dir="ltr"}
  [SkeletonRightHand]{dir="ltr"}   [Skeleton]{dir="ltr"}   [Orientations for each bone in a hand]{dir="ltr"}
  [HeadsetOnHead]{dir="ltr"}       [Boolean]{dir="ltr"}    [True or false]{dir="ltr"}
  [InteractWithUI]{dir="ltr"}      [Boolean]{dir="ltr"}    [True or false]{dir="ltr"}
  [GrapGrip]{dir="ltr"}            [Boolean]{dir="ltr"}    [True or false]{dir="ltr"}
  [GrapPinch]{dir="ltr"}           [Boolean]{dir="ltr"}    [True or false]{dir="ltr"}
  [Teleport]{dir="ltr"}            [Boolean]{dir="ltr"}    [True or false]{dir="ltr"}

[]{dir="ltr"}

[We've added 5 custom actions for our own behaviours.]{dir="ltr"}

[]{dir="ltr"}

  **[Action]{dir="ltr"}**        **[Type]{dir="ltr"}**   **[Description]{dir="ltr"}**
  ------------------------------ ----------------------- --------------------------------------------------------------------------
  [Dial]{dir="ltr"}              [Vector2]{dir="ltr"}    [Used to find the active position on the touchpad.]{dir="ltr"}
  [DialClick]{dir="ltr"}         [Boolean]{dir="ltr"}    [Used to detect whether the touchpad is pressed down.]{dir="ltr"}
  [TriggerClick]{dir="ltr"}      [Boolean]{dir="ltr"}    [Used to detect whether the trigger button is fully engaged.]{dir="ltr"}
  [MenuButtonClick]{dir="ltr"}   [Boolean]{dir="ltr"}    [Used to detect whether the menu button is pressed down.]{dir="ltr"}
  [PlayTutorial]{dir="ltr"}      [Boolean]{dir="ltr"}    [Used to detect whether the touchpad is pressed down.]{dir="ltr"}

### [Using an action]{dir="ltr"}

[You can use your actions in your own code after you've registered your
actions. The SteamVR\_Input class has several methods that retrieve
action references.]{dir="ltr"}

[]{dir="ltr"}

  [**Action**]{dir="ltr"}           **[Type]{dir="ltr"}**                     **[Description]{dir="ltr"}**
  --------------------------------- ----------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  [GetBooleanAction]{dir="ltr"}     [SteamVR\_Action\_Boolean]{dir="ltr"}     [Boolean actions are either true or false. There are a variety of helper events included that will fire for the given input source. They\'re prefixed with \"on\".]{dir="ltr"}
  [GetSingleAction]{dir="ltr"}      [SteamVR\_Action\_Single]{dir="ltr"}      [An analog action with a value generally from 0 to 1. Also provides a delta since the last update.]{dir="ltr"}
  [GetVector2Action]{dir="ltr"}     [SteamVR\_Action\_Vector2]{dir="ltr"}     [An analog action with two values generally from -1 to 1. Also provides a delta since the last update.]{dir="ltr"}
  [GetVector3Action]{dir="ltr"}     [SteamVR\_Action\_Vector3]{dir="ltr"}     [An analog action with three values generally from -1 to 1. Also provides a delta since the last update.]{dir="ltr"}
  [GetPoseAction]{dir="ltr"}        [SteamVR\_Action\_Pose]{dir="ltr"}        [Pose actions represent a position, rotation, and velocities inside the tracked space. SteamVR keeps a log of past poses so you can retrieve old poses with GetPoseAtTimeOffset or GetVelocitiesAtTimeOffset. You can also pass in times in the future to these methods for SteamVR\'s best prediction of where the pose will be at that time.]{dir="ltr"}
  [GetSkeletonAction]{dir="ltr"}    [SteamVR\_Action\_Skeleton]{dir="ltr"}    [Skeleton Actions are our best approximation of where your hands are while holding vr controllers and pressing buttons. We give you 31 bones to help you animate hand models.]{dir="ltr"}[^3][]{dir="ltr"}
  [GetVibrationAction]{dir="ltr"}   [SteamVR\_Action\_Vibration]{dir="ltr"}   [Vibration actions are used to trigger haptic feedback in vr controllers.]{dir="ltr"}

[]{dir="ltr"}

[Use any of these methods to get a reference to your registered action
object. Every action object has different properties, you should check
out the api reference documentation]{dir="ltr"}[^4] [for more details
about these actions.]{dir="ltr"}

[]{dir="ltr"}

+--------------------------------------------------------------------+
| [public class MyActionHandlerComponent : MonoBehaviour]{dir="ltr"} |
|                                                                    |
| [{]{dir="ltr"}                                                     |
|                                                                    |
| [ ...]{dir="ltr"}                                                  |
|                                                                    |
| []{dir="ltr"}                                                      |
|                                                                    |
| [public SteamVR\_Action\_Vector2 DialAction =]{dir="ltr"}          |
|                                                                    |
| [SteamVR\_Input.GetBooleanAction(\"Dial\");]{dir="ltr"}            |
|                                                                    |
| []{dir="ltr"}                                                      |
|                                                                    |
| [public SteamVR\_Action\_Boolean TriggerClickAction =]{dir="ltr"}  |
|                                                                    |
| [SteamVR\_Input.GetBooleanAction(\"TriggerClick\");]{dir="ltr"}    |
|                                                                    |
| [ ]{dir="ltr"}                                                     |
|                                                                    |
| [\...]{dir="ltr"}                                                  |
|                                                                    |
| [}]{dir="ltr"}                                                     |
+--------------------------------------------------------------------+

[]{dir="ltr"}

[You can use the action reference to get the state of your action during
the game. The following code fragment is an example of some properties
that can be used as input for your game.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private void Update()]{dir="ltr"}                                    |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // Boolean action]{dir="ltr"}                                       |
|                                                                       |
| [TriggerClickAction.stateDown // True when trigger is pressed         |
| down.]{dir="ltr"}                                                     |
|                                                                       |
| [TriggerClickAction.stateUp // True when trigger is                   |
| released.]{dir="ltr"}                                                 |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Vector2 action]{dir="ltr"}                                        |
|                                                                       |
| [ DialAction.axis // Active position on trackpad, Vector2.zero        |
| otherwise.]{dir="ltr"}                                                |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[SteamVR Interaction System]{dir="ltr"}[^5][]{dir="ltr"}
--------------------------------------------------------

[The Interaction System is a series of scripts, prefabs and other assets
that were the basis of all the minigames and other scenes in The Lab. We
use some of these prefabs and scripts in our own game.]{dir="ltr"}

[]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------+-----------------------------------+
| **[Name]{dir="ltr"}**             | **[Description]{dir="ltr"}**      |
+===================================+===================================+
| [[[Player]{.underline}](https://v | [This prefab arranges the player  |
| alvesoftware.github.io/steamvr_un | and hands in a way to make them   |
| ity_plugin/articles/Interaction-S | all easily                        |
| ystem.html#player-prefab)]{dir="l | accessible.]{dir="ltr"}           |
| tr"}                              |                                   |
|                                   | [It also contains the setup for   |
|                                   | SteamVR and the 2D fallback       |
|                                   | system]{dir="ltr"}                |
+-----------------------------------+-----------------------------------+
| [[[Interactable]{.underline}](htt | [The Interactable class           |
| ps://valvesoftware.github.io/stea | identifies to the Hand that this  |
| mvr_unity_plugin/articles/Interac | object is interactable. Any       |
| tion-System.html#interactable)]{d | object with this component will   |
| ir="ltr"}                         | receive the relevant messages     |
|                                   | from the Hand.]{dir="ltr"}        |
+-----------------------------------+-----------------------------------+
| [[[Button                         | [The hint system shows hints on   |
| hints]{.underline}](https://valve | the controllers. The hints are    |
| software.github.io/steamvr_unity_ | set up in a way where each button |
| plugin/articles/Interaction-Syste | on the controller can be called   |
| m.html#hints)]{dir="ltr"}         | out separately.]{dir="ltr"}       |
+-----------------------------------+-----------------------------------+

### [Player (Prefab)]{dir="ltr"}

[They Player prefab is used for our VR setup. It has all objects and
scripts to make sure that the camera and controllers work in our VR
environment. The Player prefab also has a 2D fallback system that allows
us to develop our game without a VR environment.]{dir="ltr"}

### [Interactable (Script)]{dir="ltr"}

[The Interactable class identifies to the Hand that this object is
interactable. Any object with this component will receive the relevant
messages from the Hand. It is used to detect when a hand is colliding
with an object and to highlight the object that the hand is colliding
with.]{dir="ltr"}

[]{dir="ltr"}

[An object will get three message when the hand is colliding with it
when it has the Interactable script as a component.]{dir="ltr"}

[]{dir="ltr"}

  [OnHandHoverBegin]{dir="ltr"}   [The hand starts colliding with the object]{dir="ltr"}
  ------------------------------- -------------------------------------------------------------
  [OnHandHoverEnd]{dir="ltr"}     [The hand stops colliding with the object]{dir="ltr"}
  [HandHoverUpdate]{dir="ltr"}    [The hand continuously collides with the object]{dir="ltr"}

[]{dir="ltr"}

[These messages are very handy because we use them for several
behaviours. One of the behaviours is repairing the tower.]{dir="ltr"}

[]{dir="ltr"}

+-------------------------------------------------------+
| [private void OnHandHoverBegin(Hand hand)]{dir="ltr"} |
|                                                       |
| [{]{dir="ltr"}                                        |
|                                                       |
| [ StartCoroutine(HealDamageable(hand));]{dir="ltr"}   |
|                                                       |
| [}]{dir="ltr"}                                        |
|                                                       |
| []{dir="ltr"}                                         |
|                                                       |
| [private void OnHandHoverEnd(Hand hand)]{dir="ltr"}   |
|                                                       |
| [{]{dir="ltr"}                                        |
|                                                       |
| [ StopAllCoroutines();]{dir="ltr"}                    |
|                                                       |
| [}]{dir="ltr"}                                        |
+-------------------------------------------------------+

### [Hand (Prefab)]{dir="ltr"}

[The Player prefab has two Hand prefabs, one for your left hand and one
for your right hand. The Hand prefabs represent the Vive controllers in
our game. The Hand prefab has a Hand script. this script contains a lot
options, information and functions for the hand. This information can be
found in the documentation of SteamVR.]{dir="ltr"}

[]{dir="ltr"}

[[[https://valvesoftware.github.io/steamvr\_unity\_plugin/articles/Interaction-System.html\#hand]{.underline}](https://valvesoftware.github.io/steamvr_unity_plugin/articles/Interaction-System.html#hand)]{dir="ltr"}

[[[https://valvesoftware.github.io/steamvr\_unity\_plugin/api/Valve.VR.InteractionSystem.Hand.html]{.underline}](https://valvesoftware.github.io/steamvr_unity_plugin/api/Valve.VR.InteractionSystem.Hand.html)]{dir="ltr"}

#### [Haptic feedback]{dir="ltr"}

[Adding haptic feedback is very valuable for the players of our game. It
provides feedback to the player without the need to actually look at the
controllers. It's very clear for the player that something is happening
when they can feel a vibration when a button is pressed.]{dir="ltr"}

[]{dir="ltr"}

[The following code fragment shows you how to use the TriggerHapticPulse
method from the Hand class.]{dir="ltr"}

[]{dir="ltr"}

+---------------------------------------+
| [hand.TriggerHapticPulse(]{dir="ltr"} |
|                                       |
| [HapticPulseDuration,]{dir="ltr"}     |
|                                       |
| [ HapticPulseFrequency,]{dir="ltr"}   |
|                                       |
| [ HapticPulseAmplitude]{dir="ltr"}    |
|                                       |
| [);]{dir="ltr"}                       |
+---------------------------------------+

[]{dir="ltr"}

#### [Button hints (Prefab)]{dir="ltr"}

[Button hits are floating text meshes with an arrow to the action
source. Showing a button hint is an easy way for players to see what
they can do using the controllers. The screenshot below show what the
button hints in our game look like.]{dir="ltr"}

[]{dir="ltr"}

![](media/image30.png){width="5.395833333333333in"
height="2.7916666666666665in"}[]{dir="ltr"}

[]{dir="ltr"}

[Creating a button hint is very easy. The ControllerButtonHints class
has two static methods to show and hide button hints.]{dir="ltr"}

[]{dir="ltr"}

[You need to pass on three arguments to show a button hint, first the
hand that you want the button hint to appear on, then the action that
the arrow should point at, and finally the text that should be shown in
the button hint. To hide a button hint you only need to pass along the
hand and the action.]{dir="ltr"}

[]{dir="ltr"}

+--------------------------------------------------+
| [ControllerButtonHints.ShowTextHint(]{dir="ltr"} |
|                                                  |
| [ Player.instance.rightHand,]{dir="ltr"}         |
|                                                  |
| [ DialClickActionHint,]{dir="ltr"}               |
|                                                  |
| [ \"Kies een toren\"]{dir="ltr"}                 |
|                                                  |
| [);]{dir="ltr"}                                  |
|                                                  |
| []{dir="ltr"}                                    |
|                                                  |
| [ControllerButtonHints.HideTextHint(]{dir="ltr"} |
|                                                  |
| [ Player.instance.rightHand,]{dir="ltr"}         |
|                                                  |
| [ DialClickAction]{dir="ltr"}                    |
|                                                  |
| [);]{dir="ltr"}                                  |
+--------------------------------------------------+

[Dial (Script)]{dir="ltr"}
--------------------------

[The dial is placed on the touchpad of the controller. With the dial you
can select which tower will be placed. It works like an old phone
dial.]{dir="ltr"}

[]{dir="ltr"}

![](media/image27.png){width="1.640625546806649in"
height="1.640625546806649in"}[]{dir="ltr"}

[]{dir="ltr"}

[You can use your thumb to select an option, when the option is selected
and the dial is pressed down an action is emitted.]{dir="ltr"}

[]{dir="ltr"}

[The dial control has 2 parts, the dial itself and the dial controls.
The default dial control does not have any functionality, it functions
as an abstract class for the different dial options.]{dir="ltr"}

[]{dir="ltr"}

![](media/image24.png){width="5.296875546806649in"
height="3.3408081802274716in"}[]{dir="ltr"}

[]{dir="ltr"}

[When the Dial is created we need to create the DialOption instance.
DialOptions are prefabs with the DialOption component. We set the parent
transform of the DialOptions to the Dial. This means that the DialOption
will follow the given parent Transform, we won't have to update the
position every update.]{dir="ltr"}

[]{dir="ltr"}

[We need to set the DialOption position after we've instantiated once.
The DialOptions are placed along a circle with a radius of the given
DialOptionRadius. We divide 365 by the amount of dial options. This will
give us the degrees that each dial option takes.]{dir="ltr"}

[]{dir="ltr"}

![](media/image44.png){width="5.21250656167979in"
height="2.744792213473316in"}[]{dir="ltr"}

[]{dir="ltr"}

[We use the sine and cosine functions to create a normalized vector for
the direction in which the dial option should be placed. We multiply
this vector with the desired distance, this gives us the local position
of the dial.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [DialOptionInstances = new                                            |
| GameObject\[DialOptions.Length\];]{dir="ltr"}                         |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Create dial option instances]{dir="ltr"}                          |
|                                                                       |
| [for (int i = 0; i \< DialOptions.Length; i++)]{dir="ltr"}            |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ DialOptionInstances\[i\] = Instantiate(]{dir="ltr"}                 |
|                                                                       |
| [ DialOptions\[i\].gameObject,]{dir="ltr"}                            |
|                                                                       |
| [ gameObject.transform]{dir="ltr"}                                    |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var segmentAngle = (365 / DialOptionInstances.Length);]{dir="ltr"}  |
|                                                                       |
| [ var segmentAngleCenter = segmentAngle / 2;]{dir="ltr"}              |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Update position]{dir="ltr"}                                      |
|                                                                       |
| [ var localRotationInRadians =]{dir="ltr"}                            |
|                                                                       |
| [((segmentAngle \* i) + segmentAngleCenter) \*                        |
| Mathf.Deg2Rad;]{dir="ltr"}                                            |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var localPosition = new Vector3(]{dir="ltr"}                        |
|                                                                       |
| [ Mathf.Sin(localRotationInRadians),]{dir="ltr"}                      |
|                                                                       |
| [ 0,]{dir="ltr"}                                                      |
|                                                                       |
| [ Mathf.Cos(localRotationInRadians)]{dir="ltr"}                       |
|                                                                       |
| [) \* DialOptionRadius;]{dir="ltr"}                                   |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Update position by adding local position]{dir="ltr"}             |
|                                                                       |
| [// to world position of right hand]{dir="ltr"}                       |
|                                                                       |
| [ DialOptionInstances\[i\].transform.position =]{dir="ltr"}           |
|                                                                       |
| [ transform.position + (transform.rotation \*                         |
| localPosition);]{dir="ltr"}                                           |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[The following screenshot shows the position of the dial options using
the Dial script.]{dir="ltr"}

[]{dir="ltr"}

![](media/image20.png){width="2.9166666666666665in"
height="2.6145833333333335in"}[]{dir="ltr"}

[]{dir="ltr"}

[We've added the "Dial" action to our SteamVR action bindings, this
action has the SteamVR\_Action\_Vector2 type. In the Update method of
our Dial script we try to find the dial that is selected.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [private void Update()]{dir="ltr"}                                    |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [// Ignore this method if the dial action is not active]{dir="ltr"}   |
|                                                                       |
| [ if (DialAction.axis == Vector2.zero)]{dir="ltr"}                    |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ return;]{dir="ltr"}                                                 |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var dialOption = FindDialOption(DialAction.axis);]{dir="ltr"}       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [// Update dial option IsSelected state to true, this                 |
| property]{dir="ltr"}                                                  |
|                                                                       |
| [// can be used to change the appearance when the dial option is      |
| selected]{dir="ltr"}                                                  |
|                                                                       |
| [ dialOption.IsSelected = true;]{dir="ltr"}                           |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ \...]{dir="ltr"}                                                    |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [private DialOption FindDialOption(Vector2                            |
| positionOnTouchpad)]{dir="ltr"}                                       |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ // We can\'t find a dial option when there are no options           |
| :D]{dir="ltr"}                                                        |
|                                                                       |
| [ if (DialOptionInstances.Length \< 1)]{dir="ltr"}                    |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ return null;]{dir="ltr"}                                            |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Find option index by calculating the angle of the touchpad       |
| position]{dir="ltr"}                                                  |
|                                                                       |
| [ var radians = Mathf.Atan2(]{dir="ltr"}                              |
|                                                                       |
| [positionOnTouchpad.x,]{dir="ltr"}                                    |
|                                                                       |
| [ positionOnTouchpad.y]{dir="ltr"}                                    |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var degrees = (Mathf.Rad2Deg \* radians) -                          |
| DialRotationOffset;]{dir="ltr"}                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // atan2 flips to -Pi after 180 degrees, this means that we must    |
| add]{dir="ltr"}                                                       |
|                                                                       |
| [// 360 degrees to create a full circle when this happens]{dir="ltr"} |
|                                                                       |
| [ if (degrees \< 0)]{dir="ltr"}                                       |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ degrees += 360;]{dir="ltr"}                                         |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var optionDegreeSpan = 360 /                                        |
| DialOptionInstances.Length;]{dir="ltr"}                               |
|                                                                       |
| [ var optionIndex = Mathf.FloorToInt(degrees /                        |
| optionDegreeSpan);]{dir="ltr"}                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Get dial option]{dir="ltr"}                                      |
|                                                                       |
| [ return DialOptionInstances\[optionIndex\];]{dir="ltr"}              |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[Calculating which dial option is selected is basically the inverse
calculation that we use to instantiate dial options. Instead of using
the cosine and sine functions to calculate the angle we use Atan2 to get
the degrees of the position on the touchpad.]{dir="ltr"}

[]{dir="ltr"}

![](media/image35.png){width="6.244792213473316in"
height="2.4559853455818024in"}[]{dir="ltr"}

[]{dir="ltr"}

[We divide the degrees by the amount of degrees that one dial option
spans, to get array index of the active dial option. The active dial
options is highlighted by applying a different material.]{dir="ltr"}

[]{dir="ltr"}

+---------------------------------------------------------------+
| [public abstract class DialOption : MonoBehaviour]{dir="ltr"} |
|                                                               |
| [{]{dir="ltr"}                                                |
|                                                               |
| [ public Material InactiveMaterial;]{dir="ltr"}               |
|                                                               |
| [ public Material ActiveMaterial;]{dir="ltr"}                 |
|                                                               |
| []{dir="ltr"}                                                 |
|                                                               |
| [\[HideInInspector\]]{dir="ltr"}                              |
|                                                               |
| [ public bool IsSelected;]{dir="ltr"}                         |
|                                                               |
| []{dir="ltr"}                                                 |
|                                                               |
| [\...]{dir="ltr"}                                             |
|                                                               |
| []{dir="ltr"}                                                 |
|                                                               |
| [private void Update()]{dir="ltr"}                            |
|                                                               |
| [{]{dir="ltr"}                                                |
|                                                               |
| [ TargetMesh.material = IsSelected]{dir="ltr"}                |
|                                                               |
| [? ActiveMaterial]{dir="ltr"}                                 |
|                                                               |
| [: InactiveMaterial;]{dir="ltr"}                              |
|                                                               |
| [}]{dir="ltr"}                                                |
|                                                               |
| [}]{dir="ltr"}                                                |
+---------------------------------------------------------------+

[]{dir="ltr"}

[The following images show what the dial looks like when a dial option
is active.]{dir="ltr"}

[]{dir="ltr"}

![](media/image46.png){width="6.270833333333333in"
height="2.138888888888889in"}[]{dir="ltr"}

[SpawnDialOption (Script)]{dir="ltr"}
-------------------------------------

[To make a gameobject placeable in the world you need to create its
corresponding SpawnDialOption as a prefab and add this to the Dial
(Script) component in the inspector
(LSDPlayer/SteamVRObjects/RightHand/Dial). The SpawnDialOption has two
additional properties:]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------+-----------------------------------+
| **[Property]{dir="ltr"}**         | **[Description]{dir="ltr"}**      |
+===================================+===================================+
| [Preview]{dir="ltr"}              | [A temporary version of the       |
|                                   | object that you hold while        |
|                                   | placing.]{dir="ltr"}              |
|                                   |                                   |
|                                   | [It's recommended to have this as |
|                                   | a child in the SpawnDialOption    |
|                                   | prefab.]{dir="ltr"}               |
|                                   |                                   |
|                                   | [In case of a tower it should     |
|                                   | have the Renderable Colliders and |
|                                   | World Placeable components (with  |
|                                   | isVisible=true) so that the       |
|                                   | player can see when he can place  |
|                                   | the tower or not.]{dir="ltr"}     |
+-----------------------------------+-----------------------------------+

[]{dir="ltr"}

[When the player presses the dial option, a preview of the active dial
option gets instantiated to the players hand. It will start the OnBuild
coroutine.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                          |
|                                                                       |
| [/// Create new instance of \`prefab\` and attach it to player        |
| hand]{dir="ltr"}                                                      |
|                                                                       |
| [/// \</summary\>]{dir="ltr"}                                         |
|                                                                       |
| [public override void OnPressStart(SteamVR\_Action\_Vector2           |
| action)]{dir="ltr"}                                                   |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ if (\_playerStatistics.Funds \< Preview.Price)]{dir="ltr"}          |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ return;]{dir="ltr"}                                                 |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var hand =                                                          |
| Player.instance.GetHand(action.activeDevice);]{dir="ltr"}             |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ hand.AttachObject(]{dir="ltr"}                                      |
|                                                                       |
| [ Instantiate(Preview.gameObject),]{dir="ltr"}                        |
|                                                                       |
| [ GrabTypes.None]{dir="ltr"}                                          |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ StartCoroutine(OnBuild(hand));]{dir="ltr"}                          |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[When the player is in the process of spawning a new gameobject haptic
feedback will be applied to the controller that is used for the action.
The haptic feedback is continuously applied until the coroutine is
stopped.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                          |
|                                                                       |
| [/// Apply haptic feedback when user is building a tower]{dir="ltr"}  |
|                                                                       |
| [/// \</summary\>]{dir="ltr"}                                         |
|                                                                       |
| [private IEnumerator OnBuild(Hand hand)]{dir="ltr"}                   |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ hand.TriggerHapticPulse(HapticPulseDuration, HapticPulseFrequency,  |
| 1);]{dir="ltr"}                                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ yield return new WaitForSeconds(HapticPulseDuration);]{dir="ltr"}   |
|                                                                       |
| [ yield return OnBuild(hand);]{dir="ltr"}                             |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[When the dial is released the haptic feedback from the OnBuild method
is stopped, the preview is detached from the hand, the onBuild message
is sent to the Preview instance and the preview instance is
destroyed.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                          |
|                                                                       |
| [/// Detach object from hand]{dir="ltr"}                              |
|                                                                       |
| [/// \</summary\>]{dir="ltr"}                                         |
|                                                                       |
| [public override void OnRelease(SteamVR\_Action\_Vector2              |
| action)]{dir="ltr"}                                                   |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ StopAllCoroutines();]{dir="ltr"}                                    |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ var hand =                                                          |
| Player.instance.GetHand(action.activeDevice);]{dir="ltr"}             |
|                                                                       |
| [ var preview = hand.currentAttachedObject;]{dir="ltr"}               |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ if (preview == null)]{dir="ltr"}                                    |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ return;]{dir="ltr"}                                                 |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Destroy preview and replace with \"real\" instance]{dir="ltr"}   |
|                                                                       |
| [ hand.DetachObject(preview);]{dir="ltr"}                             |
|                                                                       |
| [ Destroy(preview);]{dir="ltr"}                                       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Handle buildable logic when component exist]{dir="ltr"}          |
|                                                                       |
| [ var buildable = preview.GetComponent\<Buildable\>();]{dir="ltr"}    |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ if (buildable == null \|\| !buildable.IsPositionValid)]{dir="ltr"}  |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ return;]{dir="ltr"}                                                 |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Create final instance when position is valid]{dir="ltr"}         |
|                                                                       |
| [ buildable.SendMessage(]{dir="ltr"}                                  |
|                                                                       |
| [ \"OnBuild\",]{dir="ltr"}                                            |
|                                                                       |
| [ hand.objectAttachmentPoint,]{dir="ltr"}                             |
|                                                                       |
| [ SendMessageOptions.RequireReceiver]{dir="ltr"}                      |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[The Builldable script will handle the OnBuild message by subtracting a
price from the player statistics and instantiating a final version of
the thing we want to spawn.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [/// \<summary\>]{dir="ltr"}                                          |
|                                                                       |
| [/// Creates a new instance of the Prefab property and]{dir="ltr"}    |
|                                                                       |
| [/// subtracts the given Cost from the players\' funds]{dir="ltr"}    |
|                                                                       |
| [/// \</summary\>]{dir="ltr"}                                         |
|                                                                       |
| [private void OnBuild(Transform transform)]{dir="ltr"}                |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ var playerStatistics =                                              |
| Player.instance.GetComponent\<PlayerStatistics\>();]{dir="ltr"}       |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ // Update player funds]{dir="ltr"}                                  |
|                                                                       |
| [ playerStatistics.UpdateFunds(-Price);]{dir="ltr"}                   |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ Instantiate(]{dir="ltr"}                                            |
|                                                                       |
| [ Prefab,]{dir="ltr"}                                                 |
|                                                                       |
| [ transform.position,]{dir="ltr"}                                     |
|                                                                       |
| [ transform.rotation]{dir="ltr"}                                      |
|                                                                       |
| [);]{dir="ltr"}                                                       |
|                                                                       |
| [}]{dir="ltr"}                                                        |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[Wave System]{dir="ltr"}
========================

[The waves of the game are managed by the Wave States component. This
component is part of the GameManager gameobject.]{dir="ltr"}

[]{dir="ltr"}

  **[Properties]{dir="ltr"}**     **[Description]{dir="ltr"}**
  ------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  [Waves (Wave\[\])]{dir="ltr"}   [A List of Waves. A Wave is a scriptable component which are stored in \\src\\Assets\\LightSkyDefense\\Waves and can be created from the "Project" window.]{dir="ltr"}
  [Wave End Sate]{dir="ltr"}      [The state that will be set when all waves have been finished.]{dir="ltr"}

[]{dir="ltr"}

[A wave is a scriptable object that spawns enemies in order. Every Wave
has only one property, a list of Wave Steps that he will execute in
order.]{dir="ltr"}

[Wave step]{dir="ltr"}
----------------------

[A wave step is the abstract class that functions as a sub component of
wave. It has one coroutine, Run(), that will execute the step. There are
two types of wave steps implemented: a spawn step and a cooldown
step.]{dir="ltr"}

[]{dir="ltr"}

  **[Step]{dir="ltr"}**   [**Task**]{dir="ltr"}
  ----------------------- -----------------------------------------------------------------------
  [Spawn]{dir="ltr"}      [Spawns the object set in the Enemy property]{dir="ltr"}
  [Cooldown]{dir="ltr"}   [Waits the number of seconds set in the cooldown property]{dir="ltr"}

![](media/image21.png){width="4.020833333333333in" height="4.010416666666667in"}[]{dir="ltr"}
---------------------------------------------------------------------------------------------

[Creating a wave]{dir="ltr"}
----------------------------

[Since wave is a scriptable object it doesn't have to be attached to a
prefab to be customized. When you want to add a new wave simple open the
context menu, select create, select waves and finally select
wave.]{dir="ltr"}

[]{dir="ltr"}

![](media/image22.png){width="6.270833333333333in"
height="1.1666666666666667in"}[]{dir="ltr"}

[]{dir="ltr"}

[This will create a new wave asset in the directory where you opened the
context menu.]{dir="ltr"}

[]{dir="ltr"}

![](media/image15.png){width="3.3125in"
height="1.4479166666666667in"}[]{dir="ltr"}

[]{dir="ltr"}

[Clicking on this wave asset will open the inspector where you can edit
this specific asset and create whatever wave you want.]{dir="ltr"}

[]{dir="ltr"}

![](media/image31.png){width="2.90625in"
height="2.9270833333333335in"}[]{dir="ltr"}

[]{dir="ltr"}

[A wave only has one property. A list of Wave Steps. Select a size (you
can resize it later if you selected wrong) and you'll be able to set the
wave steps at each index. These wave steps will be executed (The run
step will be yield returned) in the order that you set them.]{dir="ltr"}

[]{dir="ltr"}

[When you select any of the indexes to set it, you will automatically be
able to choose from all spawn steps that are already made.]{dir="ltr"}

[]{dir="ltr"}

![](media/image1.png){width="5.385416666666667in"
height="1.5in"}[]{dir="ltr"}

[]{dir="ltr"}

[Wave steps are also scriptable objects so in the case the step you want
doesn't exist already you can create it by opening the context menu,
selecting create, selecting waves, and finally selecting either
SpawnStep or CooldownStep.]{dir="ltr"}

[]{dir="ltr"}

![](media/image10.png){width="6.270833333333333in"
height="1.0in"}[]{dir="ltr"}

[]{dir="ltr"}

[The step will appear in the directory you opened the context menu in.
By selecting it you will open the context menu where you can change the
settings however you want.]{dir="ltr"}

[]{dir="ltr"}

![](media/image45.png){width="5.828125546806649in"
height="1.4519542869641295in"}[]{dir="ltr"}

[]{dir="ltr"}

[Once you've done this the newly added steps will be in the list of
wavesteps options and you can add it to your wave.]{dir="ltr"}

 []{dir="ltr"}
=============

[Graphics]{dir="ltr"}
=====================

[Post processing]{dir="ltr"}
----------------------------

[Unity includes a new set of shaders]{dir="ltr"}[^6] [to improve the
visuals of the game. These are not enabled by default in the version of
Unity we use. They can be enabled from Window/Package manager -\> Post
Processing.]{dir="ltr"}

[Intersect Shader]{dir="ltr"}
-----------------------------

[Because the playing field can be bare, it can be difficult for the
player when he's placing towers to know if it's colliding or not. To
remedy this an intersect shader is used on the tower which the player is
trying to place. Unity has a special semantic for retrieving the pixel
position]{dir="ltr"}[^7][, we use this to create a postproces-like
shader which can be used as a normal (surface) shader. This shader is
used for the RenderableColliders component.]{dir="ltr"}

[]{dir="ltr"}

+-----------------------------------------------------------------------+
| [//vpos is Screen space pixel position]{dir="ltr"}                    |
|                                                                       |
| [fixed4 frag(v2f i, UNITY\_VPOS\_TYPE vpos : VPOS) :                  |
| SV\_Target]{dir="ltr"}                                                |
|                                                                       |
| [{]{dir="ltr"}                                                        |
|                                                                       |
| [ //We need uv of whole screen and not just the material the shader   |
| is applied to (we divide by \_ScreenParams to correct for the aspect  |
| ratio)]{dir="ltr"}                                                    |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ float2 screenuv = vpos.xy / \_ScreenParams.xy;]{dir="ltr"}          |
|                                                                       |
| [ //Sample from depth texture (1..0 on DX11+)]{dir="ltr"}             |
|                                                                       |
| [ float screenDepth = Linear01Depth(]{dir="ltr"}                      |
|                                                                       |
| [ tex2D(\_CameraDepthTexture, screenuv));]{dir="ltr"}                 |
|                                                                       |
| [ float diff = screenDepth - Linear01Depth(vpos.z);]{dir="ltr"}       |
|                                                                       |
| [ float intersect = 0;]{dir="ltr"}                                    |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ if (diff \> 0)]{dir="ltr"}                                          |
|                                                                       |
| [ //smoothstep diff between 0 and the farplane, 1 - because the       |
| screendepth range is from 1..0]{dir="ltr"}                            |
|                                                                       |
| [ intersect = 1 - smoothstep(]{dir="ltr"}                             |
|                                                                       |
| [ 0, \_ProjectionParams.w \* \_FadeLength, diff);]{dir="ltr"}         |
|                                                                       |
| []{dir="ltr"}                                                         |
|                                                                       |
| [ //Color the intersection]{dir="ltr"}                                |
|                                                                       |
| [ fixed4 glowColor = \_GlowColor \* intersect;]{dir="ltr"}            |
|                                                                       |
| [ //Remove the transparency when it\'s close to the camera to not     |
| obstruct your view]{dir="ltr"}                                        |
|                                                                       |
| [ fixed4 col = (\_Color \* \_Color.a \* screenDepth) +                |
| glowColor;]{dir="ltr"}                                                |
|                                                                       |
| [ //Make the transparency effect \"further away\" (but still have the |
| same rollin)]{dir="ltr"}                                              |
|                                                                       |
| [ col.a -= vpos.z-0.2;]{dir="ltr"}                                    |
|                                                                       |
| [ col.a = clamp(col.a, 0.0, 1.0);]{dir="ltr"}                         |
|                                                                       |
| [ return col;]{dir="ltr"}                                             |
|                                                                       |
| [}]{dir="ltr"}                                                        |
|                                                                       |
| []{dir="ltr"}                                                         |
+-----------------------------------------------------------------------+

[]{dir="ltr"}

[^1]: [\"Input System \| SteamVR Unity Plugin - GitHub Pages.\"
    [[https://valvesoftware.github.io/steamvr\_unity\_plugin/articles/SteamVR-Input.html]{.underline}](https://valvesoftware.github.io/steamvr_unity_plugin/articles/SteamVR-Input.html).
    Accessed 11 Jun. 2019.]{dir="ltr"}

[^2]: [\"Interaction System from The Lab \| SteamVR Unity Plugin.\"
    [[https://valvesoftware.github.io/steamvr\_unity\_plugin/articles/Interaction-System.html]{.underline}](https://valvesoftware.github.io/steamvr_unity_plugin/articles/Interaction-System.html).
    Accessed 11 Jun. 2019.]{dir="ltr"}

[^3]: [\"Introducing SteamVR Skeletal Input - Steam Community.\" 21 Jun.
    2018,
    [[https://steamcommunity.com/games/250820/announcements/detail/1690421280625220068]{.underline}](https://steamcommunity.com/games/250820/announcements/detail/1690421280625220068).
    Accessed 12 Jun. 2019.]{dir="ltr"}

[^4]: [\"Namespace Valve.VR \| SteamVR Unity Plugin.\"
    [[https://valvesoftware.github.io/steamvr\_unity\_plugin/api/Valve.VR.html]{.underline}](https://valvesoftware.github.io/steamvr_unity_plugin/api/Valve.VR.html).
    Accessed 12 Jun. 2019.]{dir="ltr"}

[^5]: [\"Interaction System from The Lab \| SteamVR Unity Plugin.\"
    [[https://valvesoftware.github.io/steamvr\_unity\_plugin/articles/Interaction-System.html]{.underline}](https://valvesoftware.github.io/steamvr_unity_plugin/articles/Interaction-System.html).
    Accessed 12 Jun. 2019.]{dir="ltr"}

[^6]: [\"Quick-start \| Package Manager UI website - Unity - Manual.\"
    [[https://docs.unity3d.com/Packages/com.unity.postprocessing\@2.0/manual/Quick-start.html]{.underline}](https://docs.unity3d.com/Packages/com.unity.postprocessing@2.0/manual/Quick-start.html).
    Accessed 12 Jun. 2019.]{dir="ltr"}

[^7]: [\"Other special semantics \| Shader semantics\"]{dir="ltr"}

    [[[https://docs.unity3d.com/Manual/SL-ShaderSemantics.html]{.underline}](https://docs.unity3d.com/Manual/SL-ShaderSemantics.html)
    Accessed 12 Jun. 2019.]{dir="ltr"}
