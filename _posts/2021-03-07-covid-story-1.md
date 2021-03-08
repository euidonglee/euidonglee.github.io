---
title: "[COVID-STORY: Unity Game Project] 1. Creating the terrain"
date: 2021-03-07 14:20:32 -0400
categories: Unity
---
## Creating a WORMS-style terrain
For our WORMS-style minigame, the terrain should be able to easily change by bomb explosions.

![Worms Holy Hand Grenade](https://i.makeagif.com/media/1-21-2018/qiLMpA.gif)

In programmer's view, to do this we should observe bomb explosions at runtime, and destroy the pixel objects inside the area.
Therefore the terrain should be:

1. Controlled pixel-wise.
2. Can be destroyed dynamically.

A Unity ***Tilemap*** is perfect for our terrain.

