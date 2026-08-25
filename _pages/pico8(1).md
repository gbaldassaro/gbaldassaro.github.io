---
permalink: /pico8(1)/
layout: single
classes: wide
author_profile: true
title: "PICO-8 Mini-Demos"
---

PICO-8 is one of my favorite tools for prototyping — its tiny 128x128 canvas and stripped-down Lua API make it impossible to over-scope an idea, so I end up spending my time on the one mechanic I actually wanted to test instead of engine setup. These are a handful of small demos I made to try out a single idea each: a physics interaction, a control scheme, a game feel, or just a vibe.

## Demos

**Deathball**

A local two-player "game" where players bounce a ball around using simple velocity transfer on collision, all under constant gravity and wall bounces. This one was about hand-rolling elastic-feeling collision response without any physics engine to lean on.

<img src="/assets/images/pico8_deathball.gif" style="margin-left: 75px;" alt="Deathball demo" width="256">

**HLD-Inspired Bullet Hell**

A top-down survival demo with a dashing player (complete with i-frames and a particle trail), enemies that spawn in and shoot at the player's position, and a simple score-on-kill loop. The interesting part was building the whole feel — aiming, dashing, dying — out of just `atan2`, a couple of timers, and a state flag.

<img src="/assets/images/pico8_hld.gif" style="margin-left: 75px;" alt="HLD-inspired bullet hell demo" width="256">

**Conway's Game of Life**

A fully interactive implementation of Conway's Game of Life, with a cursor to toggle cells and a pause/play toggle to step the simulation. Instead of a separate grid array, this one reads and writes cell states directly to and from the screen with `pget`/`pset`, treating the framebuffer itself as the data structure.

<img src="/assets/images/pico8_life.gif" style="margin-left: 75px;" alt="Conway's Game of Life demo" width="256">

**Pop the Lock**

A tiny reflex game where a needle sweeps around a circle and you have to tap right as it crosses a target zone, which then relocates and speeds the needle up. It's a good example of how little scope a game actually needs once you've got a tight core loop and an escalating difficulty curve.

<img src="/assets/images/pico8_lock.gif" style="margin-left: 75px;" alt="Pop the Lock demo" width="256">

**Rain**

Less a game than a small mood piece — a character holding an umbrella you move around under falling rain, with a button to toggle between a light drizzle and a downpour. This one was really just an excuse to spawn and cull a lot of small particles cheaply, every frame, without the screen bogging down.

<img src="/assets/images/pico8_rain.gif" style="margin-left: 75px;" alt="Rain demo" width="256">

**Roller**

A tilt-maze demo where you don't control the ball directly — instead you raise and lower each end of a bar independently, and the ball's acceleration is derived from the angle between them, like a physical labyrinth toy. I wanted to see how different an indirect, physically-derived control scheme feels compared to just moving something directly.

<img src="/assets/images/pico8_roller.gif" style="margin-left: 75px;" alt="Roller demo" width="256">

**Space**

A momentum-based, Asteroids-style thruster toy: thrust is applied relative to the ship's current heading rather than directly setting velocity, so the ship drifts and has to be corrected for rather than just stopping on a dime. Small thruster particles and a starfield backdrop went a long way toward selling the drift.

<img src="/assets/images/pico8_space.gif" style="margin-left: 75px;" alt="Space demo" width="256">
