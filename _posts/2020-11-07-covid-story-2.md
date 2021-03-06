---
title: "[COVID-STORY: Unity Game Project] 2. Creating the terrain"
date: 2020-11-07 15:20:32 -0400
categories: Unity
---
## Creating a WORMS-style terrain
For our WORMS-style minigame, the terrain should be able to easily change by bomb explosions.

![Worms Holy Hand Grenade](https://i.makeagif.com/media/1-21-2018/qiLMpA.gif)

In programmer's view, to do this we should observe bomb explosions at runtime, and destroy the pixel objects inside the area.
Therefore the terrain should be:

**1. Controlled pixel-wise.**

**2. Can be destroyed dynamically.**

A Unity ***Tilemap*** is perfect for our terrain.

We first upload our **tile sprites** in our tile assets:

![Tile sprites](/assets/images/covid_story_1_1.png)

Then we open a **Tile Pallete** and create a terrain pallete:

![Tile Pallete](/assets/images/covid_story_1_2.png)

Finally we add our tile sprite on the pallete and start painting our terrain tilemap.

![Painting tilemap](/assets/images/covid_story_1_3.png)

## Adding background and water sprites

We add a background 2D Sprite and set z-position to 1:

![Background](/assets/images/covid_story_1_4.png)

![Background](/assets/images/covid_story_1_6.png)

We also add a water 2D Sprite and set z-position to -1:

![Water](/assets/images/covid_story_1_5.png)

## Adding a Tilemap Collider

We finally add a Tilemap Collider to our terrain.

![Tilemap Collider](/assets/images/covid_story_1_7.png)
