---
permalink: /mario/
layout: single
classes: wide
author_profile: true
title: "2D Platformer Engine"
---

This project is a recreation of level 1-1 from Super Mario Bros. made in Java, featuring custom physics, entity and element systems, and real-time rendering. I worked on this project towards the end of my AP Computer Science A course in high school. After following [this guide](https://kevinsguides.com/guides/code/java/javaprojs/simple-2d-pong/) to make Pong in Java, I felt like challenging myself to create something much more complex yet approachable. The code is exactly as I left it when I stopped working on it in high school, and I think that it's fun to see how far I've come and how I still use many of the same coding practices in my work today. 

## Physics
For this project, I implemented a custom physics engine that has gravity, acceleration-based movement, friction, and axis-aligned bounding box collisions.

The game logic is executed as follows:

```Java
public  void  gameLogic(){
	for(Entity  o  : movingObjectList){
		o.collide();
		o.motion();
		o.moveMario(key);
		o.setGrounded(false);
	}
	for(Element  e  : elementList){
		e.collide(movingObjectList);
	}
}
```

When the level is created, every moving object (including Mario and Goombas) is added to an `ArrayList` of type `Entity`, and every static element (including the ground, blocks, pipes, etc.) is added to an `ArrayList` of type `Element`. Every frame, each element in the `movingObjectList` checks for collisions, is moved, and then has its grounded state set to false as default. Likewise, each element in the `elementList` checks for collisions with objects in `movingObjectList` every frame.  

> (see "Entity and Element System" section below for more on `Entity` and `Element` classes).

**Collisions**
Collisions between `Entity` objects are handled using axis-aligned bounding boxes (AABB). Every frame, every `Entity` checks for collisions with every other `Entity`. Thanks to the small scope of this project, there is negligible performance cost for all of these checks. For example, here is the collision check for `Entity` objects falling onto each other:

```Java
//collision between entities
public  void  collide(){
	ArrayList<Entity> objects  =  MarioGame.getMovingObjectList();
	for(Entity  o  :  objects){
		//check upper collision
		if (o.getY() < y - (32  * (int)o.getPowerupState()) &&
		o.getY() -  o.getSpeedY() >= y -  32  - (32  * (int)o.getPowerupState()) &&
		o.getX() > x -  32  &&
		o.getX() < x +  32){
			if(o.getType().equals("mario") &&  this.getAlive()){
				if(KeyReader.getJumpState()){
					o.speedY  = JUMP;
				}
				else{
					o.speedY  =  10;
				}
				this.setAlive(false);
			}
			else  if(this.getType().equals("mario") &&  this.getAlive()){
				for(Entity  e  :  objects){
					e.respawn();
				}
			}
		}
			
//rest of collision logic...

	}
}
```

Each `Entity` is 32 by 32 pixels. A collision is about to happen if one `Entity`'s collision box will be moved into another `Entity`'s collision box on the next frame. In this example, if true, it then checks if one of the `Entity` objects is Mario. If Mario is above, the other `Entity` will be stomped on, so Mario receives upwards velocity and the other `Entity` is defeated. If Mario is below, Mario will be hit, so Mario is defeated and all `Entity` objects are reset. 

> The "`(int)o.getPowerupState()`" is a vestige of the unfinished powerup system, where Mario's height is determined by his powerup state.

Similar logic runs for the other three sides of each `Entity` objects' AABB, executing custom defined reactions to collisions. Additionally, collision checks between `Element` objects and `Entity` objects are done in the same way, checking if an `Entity`'s speed will move it into one of the four sides of an `Element`. If so, the `Entity` is placed on the edge of the `Element` with other reactions based on `Entity` type and which side it collided with (for example, `Entity` grounded states are set to true if the top side is collided with). 

**Movement**
After collisions are determined, each `Entity` object is moved. Each `Entity` has X and Y speed variables that are applied to their X and Y positions. These speed variables are determined by a constantly applied gravity force, collision reactions, and the player's input for Mario only. Horizontal motion accounts for screen scroll speed so that `Entity` objects are not moved incorrectly while the screen moves. 

## Entity and Element System
every entity extends from mario
need element system for running collision checks between moving objects and elements

## Rendering
color pallet switching for different enemies, run animation using index counter 

## Input Handling

## Reflections
would change delta time, level and sprite loading, add level end, add koopa sprite, add super mario
I made this project before I was exposed to many fundamental concepts in game development like delta-time, data-driven design, or package structure. If I were to continue polishing this project, 

## Special Thanks
My older brother Blake was my AP Computer Science A teacher, and he was the first person I'd show my progress on this project to. He would always give suggestions on how to solve problems I was facing and let me figure out how to apply the course concepts to solve them. 

I grew up playing countless games on the GameCube, Wii, and PS4 with him. Some of my favorite memories come from us 100%ing Mario Kart: Double Dash or playing Super Smash Bros. for hours together, and I think I love video games so much because of that. I wouldn't be the person I am today without him. 

Thank you, Blake.
