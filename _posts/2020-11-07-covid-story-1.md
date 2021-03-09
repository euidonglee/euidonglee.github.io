---
title: "[COVID-STORY: Unity Game Project] 1. Requirement Engineering"
date: 2020-11-07 14:20:32 -0400
categories: Unity
---
## Requirement Engineering
For our **Mini-Game 2: Turn-Based Strategy Game**, the functional requirements are as follows.

### 1. Game Logics
1. **Player turn** and **enemy turn** is given alternately.
2. Each turn has **time limits.**
3. Player and enemy has **HP bars**. If it goes 0, character dies.
4. If all of the opponent's team dies, that team wins the game. 

### 2. Player turn
1. Can switch to **"Move mode"** or **"Attack mode"** by pressing **Q**.
2. When it's **"Move mode"**, can **move horizontally by pressing A, D** or **jump by pressing space.**
3. When it's **"Move mode"**, can **explore the map by click & dragging the mouse.**
4. When it's **"Attack mode"**, can **shoot bombs by setting direction and power with the mouse.**

### 3. Enemy turn
1. **Shoots bombs to players with AI algorithms.**

### 4. Terrain
1. The terrain can be changed(destroyed) by bombs.
