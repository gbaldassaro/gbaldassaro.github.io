---
permalink: /mario/
layout: single
classes: wide
author_profile: true
title: "Java Super Mario Bros."
---

This project is a recreation of level 1-1 from Super Mario Bros. made in Java, featuring custom physics, entity and element systems, and real-time rendering. I worked on this project towards the end of my AP Computer Science A course in high school. After following [this guide](https://kevinsguides.com/guides/code/java/javaprojs/simple-2d-pong/) to make Pong in Java, I felt like challenging myself to create something much more complex yet approachable. The code is exactly as I left it when I stopped working on it in high school, and I think that it's fun to see how far I've come and how I still use many of the same coding practices in my work today. 

# [GitHub Repository](https://github.com/gbaldassaro/Mario-Java)

## Physics
For this project, I implemented a custom physics engine that has gravity, acceleration-based movement, friction, and axis-aligned bounding box collisions.

When the level is created, every moving object (including Mario and Goombas) is added to an `ArrayList` of type `Entity`, and every static element (including the ground, blocks, pipes, etc.) is added to an `ArrayList` of type `Element`. Every frame, each element in the `movingObjectList` checks for collisions, is moved, and then has its grounded state set to false as default. Likewise, each element in the `elementList` checks for collisions with objects in `movingObjectList` every frame.  

> (see ["Entity and Element System"](/#entity-and-element-system) section below for more on `Entity` and `Element` classes)

**Collisions**

Collisions between `Entity` objects are handled using axis-aligned bounding boxes (AABB). Every frame, every `Entity` checks for collisions with every other `Entity`. Thanks to the small scope of this project, there is negligible performance cost for all of these checks.

<div align="center">
	<img src="/assets/images/mario_collisions.gif" alt="Collisions" width="400">
</div>

<div align="center">
	> (example of collisions with Mario and `Element` objects)
</div>

Each `Entity` is 32 by 32 pixels. A collision is about to happen if one `Entity`ss collision box will be moved into another `Entity`ss collision box on the next frame. Collision logic runs for all four sides of each `Entity` objects' AABB, executing custom defined reactions to collisions. Additionally, collision checks between `Element` objects and `Entity` objects are done in the same way, checking if an `Entity`ss speed will move it into one of the four sides of an `Element`. If so, the `Entity` is placed on the edge of the `Element` with other reactions based on `Entity` type and which side it collided with (for example, `Entity` grounded states are set to true if the top side is collided with). 

**Movement**

After collisions are determined, each `Entity` object is moved. Each `Entity` has X and Y speed variables that are applied to their X and Y positions. These speed variables are determined by a constantly applied gravity force, collision reactions, and the player's input (for Mario). Horizontal motion accounts for screen scroll speed so that `Entity` objects are not moved incorrectly while the screen moves. 

Mario's movement is acceleration based, with different acceleration and top speed magnitudes based on if the player is holding the run button. Additionally, Mario's deceleration is dependent on if he is grounded or not, making him slow down and change directions much slower when in the air.

## Entity and Element System
every entity extends from mario
need element system for running collision checks between moving objects and elements

## Rendering
Each sprite is stored as a 2D array of integers, with animated sprites being stored in 3D arrays. The integers in the array correspond to predefined colors from a color pallet. Every frame, each pixel of the sprite is colored in according to its integer array. A benefit of this is that color pallets can easily be changed to recolor objects, such as recoloring Mario to give his Fire-Flower look. 

Animated sprites are stored in 3D arrays so that animation frames can be chosen via an index. For example, when Mario moves along the ground, a `double` is increased based on his speed, which is then cast as an `int` and divided modulo `n`, where `n` is the number of frames in the animation, and finally used to index through the 3D animation array. This cycles through each frame of the animation and gave me tight control of animation timings. This method was applicable to the Goomba's walk cycle as well.  

<div align="center">
	<img src="/assets/images/mario_animations.gif" alt="Collisions" width="400">
</div>

<div align="center">
	> (example of Mario and Goomba's animations working)
</div>

## Reflections
I made this project before I was exposed to many fundamental concepts in game development like delta-time, data-driven design, or file structure. Because of the lack of delta-time corrections, Mario's movement is dependent on framerate. While the code targets a specific framerate, slowdowns in framerate still lead to slowdowns in physics and gameplay. Additionally, I hardcoded the entire level and every sprite, which is functional but not at all scaleable. Pipelines for loading external level files and sprites would improve this project a ton. Finally, every script used in the project is unorganized in a single directory, so having file organization and using packages to communicate between files would work much better.

While not flawless, I still greatly appreciate the learning experience that this project provided me. 

## Special Thanks
My older brother Blake was my AP Computer Science A teacher, and he was the first person I'd show my progress on this project to. He would always give suggestions on how to solve problems I was facing and let me figure out how to apply the course concepts to solve them. 

I grew up playing countless games on the GameCube, Wii, and PS4 with him. Some of my favorite memories come from us 100% completing Mario Kart: Double Dash or playing Super Smash Bros. for hours together, and I think I love video games so much because of that. I wouldn't be the person I am today without him. 

Thank you, Blake.
