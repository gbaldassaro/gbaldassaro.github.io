---
permalink: /realTimeRendering/
layout: single
classes: wide
author_profile: true
title: "Real-Time Rendering Course Projects"
---

I made these projects for the course ELEC 328: Real-Time Rendering Systems at Rice University. The course explores the real-time graphics pipeline with projects that give experience in game engines, shader programming, XR (AR/VR), projection mapping, and virtual production. The projects run in Unity using Rice's Reality Roost, a multi-user VR facility featuring a rumbling haptic floor, a room-scale directional fan system, and more. 

## Enchanted VR Flashlight

{% include video id="wMIgVbSv_6o" provider="youtube" %}

For this project, I created a magical underwater scene with fish, plants, treasure, and rocks that all change in appearance when a flashlight is shining on them. The fish and plants are invisible when outside the flashlight’s light but appear when inside, the treasure chest reveals hidden coins when in the flashlight’s light, and the rocks reveal they are made of gold when inside the flashlight’s light. I focused a lot on the ambience of the scene, so I created waves above the player, sand with a changing texture to simulate light coming from above, fog that obscures far away objects, and a blue tint to sell the underwater effect. I also made the fish and plants move using a shader to simulate swimming and swaying in the current, respectively.

<p align="center">
    <table>
    <tr>
        <td>
        <img src="/assets/media/realTimeRendering/scene.png" alt="full scene" width="450">
        <br>
        <p align="center">
        </p>
        </td>
        <td>
        <img src="/assets/media/realTimeRendering/rock.png" alt="rock" width="450">
        <br>
        <p align="center">
        </p>
        </td>
    </tr>
    </table>
</p>

I really enjoyed making my scene and creating the underwater feeling that I was looking for, and I think that it came out quite well. It took a fair bit of trial and error to get the flashlight effects working like I intended, but it was easy enough to iteratively work through solutions to find what worked. I found Shader Graph to be very handy, though I found that my graph got a bit convoluted as I added more logic and checks. Since it was so quick and easy to make changes, I found myself making impulsive changes that I forgot how to justify later. I think that if I had more time, I’d organize my graphs to be more readable, as well as add in notes so that I can easily remember what I was thinking when I made it. I gained a lot of valuable experience making a coherent scene as well, and this was the first time that I used shaders, postprocessing, and audio to create a complete 3D scene.  

# [GitHub Repository](https://github.com/gbaldassaro/gage-baldassaro-elec328-f26-assn2)

## Procedural Mesh Interactions

{% include video id="wROO8AlNQCU" provider="youtube" %}

For this project, I created a system for cutting meshes reminiscent of Beat Saber and Fruit Ninja. My project contains three procedural mesh interactions. The first is the system to generate new meshes of an object’s two halves when it is cut. The second is a continuous ripple effect to convey that a cut half is about to disappear. The third is a white trail that follows the player’s sword to convey its speed and direction. Each of these deformations and interactions posed unique problems for me to solve, and I am very happy with how they came out. 

<p align="center">
    <table>
    <tr>
        <td>
        <img src="/assets/media/realTimeRendering/cut.png" alt="cut" width="450">
        <br>
        <p align="center">
        </p>
        </td>
        <td>
        <img src="/assets/media/realTimeRendering/trail.png" alt="trail" width="450">
        <br>
        <p align="center">
        </p>
        </td>
    </tr>
    </table>
</p>

I am extremely happy with how well the mesh cutting system I made ended up working. The cutting is satisfying to do in VR and looks great visually. It does have some flaws, such as the sword’s cut plane being based on its orientation and not its movement, making your cut angle not always match the direction you sliced. I also think that my mesh cutting script could use some work, as I continually added things to a single function instead of dividing things up. This made the file somewhat hard to navigate once it got long. 

One thing I attempted to explore but ultimately abandoned was making the fruit halves also cuttable. However, this proved difficult since I was creating two meshes (the outside and the inside of the fruit) per fruit half, so I couldn’t simply drag-and-drop my existing script onto the halves. If I were to continue this project, that is where I would continue from. One thing I learned doing this project is how strict the mesh property arrays are, as simple off-by-one errors led to vastly incorrect results. I also learned new ways to handle getting rid of objects. Since the fruit halves were children of the original fruit, I couldn’t simply delete the fruit. So, I made a script that hides the original fruit far away, which added the benefit of being able to easily bring it back when the player slices the reset apple.

# [GitHub Repository](https://github.com/gbaldassaro/gage-baldassaro-elec328-f26-assn1)
