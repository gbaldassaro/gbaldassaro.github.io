---
permalink: /openGL(1)/
layout: single
classes: wide
author_profile: true
title: "OpenGL Renderer"
---

This project is a real-time renderer built from scratch in C++ and OpenGL, following along with a rendering course/guide chapter by chapter and then pushing past it into shadow mapping, custom lighting, and a full post-processing stack. Instead of a generic test scene, I built the whole thing around a single shrine-like altar scene lit by torches and a hazy directional sun, which gave me a concrete visual target to chase rather than just checking boxes off a technique list.

# [GitHub Repository](https://github.com/gbaldassaro/OpenGL-Renderer)

## Screenshots/Comparisons

Because so much of this project was about matching a mood rather than just implementing a technique, I kept reference screenshots from *Dark Souls* open the whole time I was tuning the lighting — especially the warm, flickering torchlight against cool fog and a soft directional sun. Here are a couple of side-by-sides between renders from this project and the reference shots that guided them.

<img src="/assets/images/openGL_compare_firelight.jpg" style="margin-left: 75px;" alt="Firelight comparison" width="700">

> My render (left) next to a Dark Souls reference (right) — warm point-light falloff around the altar's torches

<img src="/assets/images/openGL_compare_fog.jpg" style="margin-left: 75px;" alt="Fog and sky comparison" width="700">

> Comparing the fogged-out background and directional sun lighting against the reference

## Topics learned

**Rendering Pipeline & Engine Structure**

Each frame runs in three passes: a depth-only pass that renders the scene from the light's point of view into a 4096x4096 depth texture (for shadow mapping), the main scene pass rendered off-screen into a framebuffer with two color attachments — one for the normal scene, one that isolates bright pixels for bloom — and finally a post-processing pass that draws a single full-screen quad textured with the result. Wrapping GLFW/GLAD boilerplate, model loading, and shaders into their own `Shader`, `Camera`, and `Model` classes meant the main loop mostly just sets uniforms and calls `Draw()`, instead of hand-managing raw OpenGL state everywhere. Post-processing effects and a shadow-map debug view can be swapped live at runtime with number keys, which made it easy to compare effects side-by-side while tuning them.

**Shaders & Lighting**

Lighting is a forward-rendered Blinn-Phong model, calculated per-fragment for one directional light and nine point lights every frame. Surface detail comes from normal mapping — tangent and bitangent vectors are built in the vertex shader into a TBN matrix, which the fragment shader uses to convert a normal map's texture-space normals into world space, adding detail to the altar's surfaces without more geometry. One custom piece I'm fairly happy with is the directional "sun" light: instead of lighting the whole scene uniformly, its intensity fades out between an inner and outer radius from a defined center point, so it only lights the area right around the shrine, like sunlight breaking through a canopy rather than a flat, scene-wide light. The sky and clouds get their own cheap animation trick too — the cloud shader scrolls its texture coordinates over time and fades opacity out near the horizon, giving the sky a sense of drifting motion without any actual geometry animation.

The shadow-mapping pipeline itself is fully built — the depth pass, the light-space transform, and even a dedicated debug view for visualizing the raw depth buffer — but the actual shadow lookup in the main lighting shader is still disabled while I finish tuning the depth bias, so shadows aren't visibly cast in the current build yet.

**Post-Processing**

All post-processing runs through the same single shader, sampling the scene's rendered-to-texture framebuffer and branching on an integer effect ID rather than needing a separate shader per effect. Sharpen, blur, and edge-detection are all the same 3x3 convolution kernel sampled over neighboring texels with different weights, while grayscale, inversion, and brightness thresholding are simple single-sample effects. Bloom is the most involved: the bright-pass buffer from the main render is blurred across several horizontal and vertical passes using a pair of small, alternating ("ping-pong") framebuffers, and the blurred result is added back on top of the original scene.

## Next steps

The next big addition I want to make is real-time raytracing, likely as a separate rendering path I can toggle against the current rasterized one for comparison. Before that, though, I want to finish wiring the shadow-mapping pipeline all the way into the main lighting calculation, since most of the infrastructure for it is already there.
