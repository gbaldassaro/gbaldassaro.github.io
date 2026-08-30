---
permalink: /mechCombat/
layout: single
classes: wide
author_profile: true
title: "Mech Combat Demo"
---

This project is a third-person mech combat demo made in Unity, built around a fast movement system, a dynamic lock-on camera, and a data-driven ranged weapon system. It's inspired by *Armored Core VI: Fires of Rubicon*, with a heavier, more grounded feel with dashing and hovering that makes fights fast and vertical. My goal was to prototype a feel similar to *Armored Core VI*, with a state-based movement controller, a camera system supporting free aim and locked-on combat, and data-based weapons.

# <span style="color: #e60000;">Playable Build Coming Soon!</span>

<p align="center">
  <video autoplay muted loop playsinline width="800">
    <source src="/assets/images/mechCombat/gameplay.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</p>

<br>

# [GitHub Repository](https://github.com/gbaldassaro/Unity-AC-Demo)

## Movement

The player's movement is handled by a small state machine (`PlayerState`: `Idle`, `Walking`, `Boosting`) layered on top of Unity's `CharacterController`. Rather than relying on rigidbody physics, `PlayerController` computes a desired horizontal velocity every frame from input and camera orientation, then smooths the player's actual velocity toward it with `Vector3.SmoothDamp`. This keeps movement responsive but not twitchy, and lets me tune acceleration separately from top speed for each state.

Horizontal input is interpreted differently depending on the camera. While free-aiming or searching for a lock-on target, movement is relative to the camera's forward and right vectors, but once locked on, it switches to being relative to the player's own forward vector so that strafing stays consistent around a target.

**Dashing and Hovering**

Dashing is an instant burst: if the player has enough energy and is already moving, `horizontalVelocityVector` is snapped directly to `dashSpeed` in the current movement direction, energy is spent, and a short coroutine tracks the dash's cooldown window. While dashing, rotation smoothing is temporarily doubled, so the mech doesn't whip around mid-dash the way it normally would when changing direction.

Vertical movement follows a similar energy-gated pattern. A grounded jump gives a single burst of vertical velocity from the target jump height, but holding the jump input while airborne instead drains energy over time to smoothly ramp toward a hover speed, letting the player extend a jump into a controlled hover as long as energy allows. Energy regenerates after a short delay following its last use, and regenerates twice as fast while grounded, which encourages landing between aggressive dash/hover strings rather than hovering indefinitely.

<p align="center">
  <video autoplay muted loop playsinline width="800">
    <source src="/assets/images/mechCombat/movement.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</p>

## Camera

The camera is built around three states and two Cinemachine virtual cameras: a free orbit camera and a dedicated lock-on camera that only activates once a target is acquired.

**Lock-On System**

When the player requests a lock, `FindLockOn` gathers nearby colliders with `Physics.OverlapSphere`, filters down to anything tagged `Enemy` within camera view, and confirms each candidate isn't obstructed with a raycast before comparing it against the current best choice. The enemy closest to the center of the screen wins. If the current target is destroyed mid-fight, the camera immediately re-runs this search rather than snapping back to free look, so the transition to a new target is continuous instead of jarring.

<p align="center">
  <video autoplay muted loop playsinline width="800">
    <source src="/assets/images/mechCombat/lockOn.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</p>

Breaking a lock is handled with a small timer rather than a single input: the player has to hold look input past `lockOnExitTime` before the camera releases the target, which avoids accidentally dropping a lock from a small camera nudge.

**Camera Movement**

While locked on, the camera automatically swaps which shoulder it favors based on the player's movement direction relative to the camera. Moving right shifts the camera to frame more of what's ahead on that side, and vice versa. On top of that, a roll tilt is applied to both Cinemachine cameras, scaled by how fast the player is strafing, which adds a bit of physicality to fast lateral movement.

<p align="center">
  <video autoplay muted loop playsinline width="800">
    <source src="/assets/images/mechCombat/strafing.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</p>

## Weapon System

Weapons are defined as `ScriptableObject` assets (`RangedWeaponData`) rather than hardcoded per-weapon classes. Each asset stores its gun model, projectile prefab and speed, fire rate, damage, ammo count, reload time, and spread. `RangedWeaponController` just reads whichever `RangedWeaponData` it's given at runtime, instantiates the correct gun model, and drives firing/reloading off of it. This means adding a new weapon is just a matter of creating a new data asset, with no new code required. Additionally, each hand runs its own independent `RangedWeaponController`, so the left and right weapons can fire, reload, and run out of ammo on separate timers.

<p align="center">
  <video autoplay muted loop playsinline width="800">
    <source src="/assets/images/mechCombat/shooting.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</p>

Projectiles themselves use raycasts each `FixedUpdate` rather than rigidbody collisions, which avoids tunneling through thin geometry at high speed, and each one stores its owner so it can never damage whoever fired it.

**Enemy Tracking**

In *Armored Core VI: Fires of Rubicon*, enemies are always moving rapidly. The player's Armored Core contains tracking systems that help to lead weapon fire ahead of moving enemies. In my project, I implemented this using the current enemy's tracked velocity and the player's projectile's speed, solving a quadratic for the time the projectile would intercept the enemy. The player's weapon is aimed towards the position of the interception, so shots actually converge on a moving enemy instead of trailing behind it.

<p align="center">
  <video autoplay muted loop playsinline width="800">
    <source src="/assets/images/mechCombat/tracking.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</p>

## Next Steps

This project gave me invaluable experience on managing complex, interlinked systems and fine-tuning behaviors to build the best feeling player experience. Now that the base systems for player movement, camera, and weapons are implemented, I want to include a few levels and different weapons to choose between, then release this for anyone to try out. Afterwards, I may work on simple enemy AI that can track the player, decide when to shoot, and move. 
