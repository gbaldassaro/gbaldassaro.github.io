---
permalink: /pico8/
layout: single
classes: wide
author_profile: true
title: "PICO-8 Mini-Demos"
---

PICO-8 is one of my favorite tools for prototyping small ideas, as its 128x128 canvas and stripped-down Lua API make intentional and careful code necessary. These are a handful of small demos I made to try out a single idea each: a physics interaction, simple arcade games, feeling, etc.

## Pop the Lock

A recreation of one of my favorite arcade games, *Pop the Lock*. This is a small reflex game where a needle sweeps around a circle and you have to tap right as it crosses a target zone, which then relocates and speeds the needle up. It's a good example of how little scope a game actually needs to still be fun.

<p align="center">
    <img src="/assets/images/pico8/lock.gif" style="margin-left: 75px;" alt="Pop the Lock demo" width="300">
</p>

## Roller

A recreation of the classic mechanical arcade game *Ice Cold Beer*. This is a tilt-maze game where you raise and lower each end of a bar independently, controlling a ball rolling on top of it and avoiding holes to reach a goal. I wanted to experiment with a physics based control scheme with a simple goal, and I'm happy with the result.

<p align="center">
    <img src="/assets/images/pico8/roller.gif" style="margin-left: 75px;" alt="Roller demo" width="300">
</p>

## Conway's Game of Life

A fully interactive implementation of Conway's Game of Life, with a cursor to toggle cells and a pause/play toggle to step the simulation. Instead of a separate grid array, this one reads and writes cell states directly to and from the screen with `pget`/`pset`, treating the framebuffer itself as the data structure.

<p align="center">
    <img src="/assets/images/pico8/life.gif" style="margin-left: 75px;" alt="Conway's Game of Life demo" width="300">
</p>

## Bullet Hell

A top-down survival demo where the player dashes to dodge bullets and defeat enemies. Enemies spawn and shoot at random intervals at the player's position. I am proud of the trail effect that follows the player's dash, which was inspired by *Hyper Light Drifter*.

<p align="center">
    <img src="/assets/images/pico8/bullet.gif" style="margin-left: 75px;" alt="HLD-inspired bullet hell demo" width="300">
</p>

## Space

A momentum-based, Asteroids-style thruster toy, where thrust is applied relative to the ship's current direction rather than directly setting its velocity. I experimented with particles in this demo, and the small effects like the thruster particles and a starfield background helped make the feeling I wanted to convey.

<p align="center">
    <img src="/assets/images/pico8/space.gif" style="margin-left: 75px;" alt="Space demo" width="300">
</p>

## Rain

A small particle test with a character holding an umbrella that you move around under falling rain, with a button to toggle between a light drizzle and a downpour. This was a test to see how many particles could realistically be handled, and how these particles can be used to convey a specific environmental feeling.

<p align="center">
    <img src="/assets/images/pico8/rain.gif" style="margin-left: 75px;" alt="Rain demo" width="300">
</p>

## Deathball

A prototype recreation of the arcade game *Death Ball*, a local two-player game where players bounce a ball around using simple velocity transfer on collision, all under constant gravity and wall bounces. It was difficult but rewarding to implement the custom physics, including collisions, bouncing, and friction.

    <p align="center">
</p><img src="/assets/images/pico8/deathball.gif" style="margin-left: 75px;" alt="Deathball demo" width="300">
