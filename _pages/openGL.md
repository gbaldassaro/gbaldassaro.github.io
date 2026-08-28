---
permalink: /openGL/
layout: single
classes: wide
author_profile: true
title: "OpenGL Renderer"
---

This project is a real-time renderer built from scratch in C++ and OpenGL, following along with the [Learn OpenGL](https://learnopengl.com/) course chapter by chapter, implementing features such as model loading, custom lighting, and a full post-processing stack. 

# [GitHub Repository](https://github.com/gbaldassaro/OpenGL-Renderer)

To build my renderer beyond a simple test scene, I wanted to recreate one of my favorite environments in any video game: the **Firelink Altar and Kiln of the First Flame** from *Dark Souls*, which offers several interesting effects that make the environment beautiful. The altar room is dark and lit sparsely by torches, the staircase has an ethereal white haze, and the sun bleeds through the sky into the dark Kiln. This gave me a concrete visual target to chase and forced me to apply the topics I learned practically. Below are some comparisons of my renderer with screenshots of *Dark Souls* I took myself.

<p align="center">
    <table>
    <tr>
        <td>
        <img src="/assets/images/openGL/altar_renderer.png" alt="altar renderer" width="450">
        <br>
        <p align="center">
            <em>Firelink Altar, my renderer</em>
        </p>
        </td>
        <td>
        <img src="/assets/images/openGL/altar.jpeg" alt="altar" width="450">
        <br>
        <p align="center">
            <em>Firelink Altar, Dark Souls</em>
        </p>
        </td>
    </tr>
    </table>
</p>

<p align="center">
    <table>
    <tr>
        <td>
        <img src="/assets/images/openGL/stair_renderer.png" alt="stair renderer" width="450">
        <br>
        <p align="center">
            <em>Staircase, my renderer</em>
        </p>
        </td>
        <td>
        <img src="/assets/images/openGL/stair.jpeg" alt="stair" width="450">
        <br>
        <p align="center">
            <em>Staircase, Dark Souls</em>
        </p>
        </td>
    </tr>
    </table>
</p>

<p align="center">
    <table>
    <tr>
        <td>
        <img src="/assets/images/openGL/kiln_renderer.png" alt="kiln renderer" width="450">
        <br>
        <p align="center">
            <em>Kiln of the First Flame, my renderer</em>
        </p>
        </td>
        <td>
        <img src="/assets/images/openGL/kiln.jpeg" alt="kiln" width="450">
        <br>
        <p align="center">
            <em>Kiln of the First Flame, Dark Souls</em>
        </p>
        </td>
    </tr>
    </table>
</p>

While not exactly the same as in the game, I am proud of the effects I was able to implement. I loved working on this environment in particular because as I learned more techniques, the environment continuously became more interesting. *Dark Souls* definitely uses more advanced techniques to render its scenes, and I'm excited to learn more about them in the future to hopefully bring my renderer up to and possibly past *Dark Souls*'s visual quality. 

## Topics Learned

**Rendering Pipeline**

This project was a great way for me to learn the details of each step of the graphics pipeline. I learned to appreciate and understand the function and possible creative uses of each step of the pipeline. Implementing coordinate transormation matrices, color buffers, model loading, and more from the ground up gave me valuable experience and familiarity with the data structures and low level management that each use. I especially liked seeing the transformation matrices in action, as I was finally able to practically apply what I learned in my Linear Algebra course at Rice. 

**Shaders & Lighting**

I was pleasantly surprised to see how clean and relatively simple Blinn-Phong lighting is, since I was intimidated by the thought of implementing lighting beforehand. Once I understood the graphics pipeline and how shaders fit into it, the vector math for coloring fragments made complete sense. Adding functionality for loading materials with custom textures and normal maps brought the fidelity of the environment up substantially, and learning the tangent and bitangent math used for normal mapping was a tough but satisfying task. I played with several lighting and material effects to make the scene, such as differing light attenuation for different light sources and allowing for materials to be unlit, such as the white surroundings in the staircase.

**Post-Processing**

I also implemented the ability to choose post-processing effects at runtime using the number keys, including an effect for inversion, grayscale, sharpen, blur, edge-detection, bloom, and depth. All of this happens in one shader, sampling a texture that a framebuffer renders the scene to initially. 

<p align="center">
  <video autoplay muted loop playsinline width="800">
    <source src="/assets/images/openGL/effects.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</p>

## Next steps

There are plenty of things left to add to make this renderer more advanced. There are clear differences in the screenshot comparisons above that I want to address, such as the flickering lighting in the *Firelink Altar*, the rolling fog along the staircase floor, and the sun bleeding through the clouds in the *Kiln of the First Flame*. *Dark Souls* has such a distinct visual identity that I adore, so to be able to make something close to it is exciting. I've also already begun experimenting with shadow mapping, so I'd like to implement that to bring more life to the scene. Additionally, I want to learn more about physically-based rendering and raytracing, which are the topics of more modern real-time renderers. 
