---
permalink: /mechCombat(1)/
layout: single
classes: wide
author_profile: true
title: "Mech Combat Demo"
---

This project is a third-person mech combat demo made in Unity, built around a fast movement system, a dynamic lock-on camera, and a data-driven ranged weapon system. It's inspired by the *Armored Core* style of combat, with a heavier, more grounded feel with dashing and hovering that makes fights fast and vertical. My goal was to prototype a "feel" first and build the systems needed to support it — a state-based movement controller, a Cinemachine-driven camera that can snap between free look and locked-on combat, and weapons authored as data rather than code.

# [Play Here!](link)

<p align="center">
  <video autoplay muted loop playsinline width="800">
    <source src="/assets/images/mechCombat_gameplay.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</p>

<br>

# [GitHub Repository](https://github.com/gbaldassaro/Unity-AC-Demo)

## Movement

The player's movement is handled by a small state machine (`PlayerState`: `Idle`, `Walking`, `Boosting`) layered on top of Unity's `CharacterController`. Rather than relying on rigidbody physics, `PlayerController` computes a desired horizontal velocity every frame from input and camera orientation, then smooths the player's actual velocity toward it with `Vector3.SmoothDamp`. This keeps movement responsive but not twitchy, and lets me tune acceleration separately from top speed for each state.

Horizontal input is interpreted differently depending on the camera: while free-aiming or searching for a lock-on target, movement is relative to the camera's forward and right vectors, but once locked on, it switches to being relative to the player's own forward vector so that strafing stays consistent around a target.

**Dashing and Hovering**

Dashing is an instant burst: if the player has enough energy and is already moving, `horizontalVelocityVector` is snapped directly to `dashSpeed` in the current movement direction, energy is spent, and a short coroutine tracks the dash's cooldown window. While dashing, rotation smoothing is temporarily doubled, so the mech doesn't whip around mid-dash the way it normally would when changing direction.

Vertical movement follows a similar energy-gated pattern. A grounded jump gives a single burst of vertical velocity from the target jump height, but holding the jump input while airborne instead drains energy over time to smoothly ramp toward a hover speed — letting the player extend a jump into a controlled hover as long as energy allows. Energy regenerates after a short delay following its last use, and regenerates twice as fast while grounded, which encourages landing between aggressive dash/hover strings rather than hovering indefinitely.

<img src="/assets/images/mechCombat_movement.gif" style="margin-left: 75px;" alt="Movement" width="400">

> Dashing out of a walk, with rotation smoothing slowed mid-dash

## Camera

The camera is built around three states (`CameraState`: `FreeAim`, `LockOnSearch`, `LockedOn`) and two Cinemachine virtual cameras — a free orbit camera and a dedicated lock-on camera that only activates once a target is acquired.

**Acquiring and holding a lock**

When the player requests a lock, `FindLockOn` gathers nearby colliders with `Physics.OverlapSphere`, filters down to anything tagged `Enemy` within camera view (with a small padding so targets right at the screen edge aren't picked), and confirms each candidate isn't obstructed with a raycast before comparing it against the current best choice. The enemy closest to the center of the screen wins. If the current target is destroyed mid-fight, the camera immediately re-runs this search rather than snapping back to free look, so the transition to a new target feels continuous instead of jarring.

Breaking a lock is handled with a small timer rather than a single input: the player has to hold look input past `lockOnExitTime` before the camera releases the target, which avoids accidentally dropping a lock from a small camera nudge.

**Tilting and sliding**

While locked on, the camera automatically swaps which shoulder it favors based on the player's movement direction relative to the camera — moving right shifts the camera to frame more of what's ahead on that side, and vice versa — smoothed with `SmoothDamp` rather than snapping. On top of that, a "dutch" (roll) tilt is applied to both Cinemachine cameras, scaled by how fast the player is strafing, which adds a bit of physicality to fast lateral movement without needing any hand-animated camera work.

<img src="/assets/images/mechCombat_lockOn.gif" style="margin-left: 75px;" alt="Lock-on camera" width="400">

> Acquiring a lock-on target and the camera's shoulder swap during a strafe

## Weapon System

Weapons are defined as `ScriptableObject` assets (`RangedWeaponData`) rather than hardcoded per-weapon classes — each asset stores its gun model, projectile prefab and speed, fire rate, damage, ammo count, reload time, and spread. `RangedWeaponController` just reads whichever `RangedWeaponData` it's given at runtime, instantiates the correct gun model, and drives firing/reloading off of it. This means adding a new weapon is just a matter of creating a new data asset, with no new code required — one controller script handles every gun in the game.

Each hand runs its own independent `RangedWeaponController`, so the left and right weapons can fire, reload, and run out of ammo on separate timers. Firing is blocked while dashing so the dash burst always reads clearly, and reloading can be triggered manually (holding a modifier while firing) or automatically once a magazine empties.

**Lead prediction**

The more interesting piece is `TrackEnemy()`, which aims each hand's reticle not at the locked-on target's current position but at where it *will* be. Using the target's tracked velocity and the projectile's speed, it solves a quadratic for the time-to-intercept and projects the target's position forward by that amount, so shots actually converge on a moving target instead of trailing behind it.

Projectiles themselves use raycasts each `FixedUpdate` rather than rigidbody collisions, which avoids tunneling through thin geometry at high speed, and each one stores its owner so it can never damage whoever fired it.

<img src="/assets/images/mechCombat_weapons.gif" style="margin-left: 75px;" alt="Weapon firing and reloading" width="400">

> Firing with lead-prediction reticles active, then reloading
