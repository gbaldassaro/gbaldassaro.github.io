---
permalink: /parfait/
layout: single
classes: wide
author_profile: true
title: "Planetary Parfait"
---

*Planetary Parfait* is a scientific visualization platform that immerses users in custom 3D terrain and data layers while offering cross-platform collaboration between desktop and VR users. My work helped to integrate the [Cesium Moon](https://cesium.com/platform/cesium-ion/content/cesium-moon/) with the project, allowing for coregistration of imported JMARS layer data and the Cesium Moon terrain data. I also contributed to shipping bug fixes and features to the public Steam build, available to download for free.

# [Steam Page](https://store.steampowered.com/app/2721860/Planetary_Parfait/)

<p align="center">
    <img src="/assets/images/parfait/banner.png" alt="banner" class="center-image" width="800">
</p>

# [GitHub Repository](https://github.com/asu-meteorstudio/PlanetaryParfait)

## Cesium Integration

The core challenge of this project was getting scientific data layers to line up correctly with the Cesium Moon terrain. JMARS data layers and Cesium each define latitude differently, so placing a data layer naively onto the terrain would leave it misaligned. Building a proper co-registration system meant reconciling those two coordinate systems precisely so that scientists could trust what they were looking at.

**Co-Registration**

Placing a JMARS data layer onto the terrain means converting every vertex of its procedurally generated mesh from geodetic to geocentric latitude, so that Cesium's own coordinate functions can correctly map the vertex to the correct coordinate on the terrain surface. These vertices are then raycast and placed onto the Cesium Moon mesh, effectively superimposing the JMARS data layer on the Cesium Moon. Reading data back out, for example when sampling a per-pixel data values at a point the user is looking at, runs the same coordinate conversion in reverse, letting users probe their imported data at per-pixel levels.

<p align="center">
    <img src="/assets/images/parfait/cesium_layers.png" alt="layers" class="center-image" width="800">
</p>

With this in place, scientists can view their own custom data layers directly on physically accurate lunar terrain rather than a rough approximation, and combine that with JMARS API access to get GIS-like analysis inside an intuitive desktop or VR environment. That combination is what makes *Planetary Parfait* useful for more than visualization alone. It's a tool for real mission planning work, like scouting and site analysis for missions under the Artemis program.

**Validation**

Scientists can only work effectively with *Planetary Parfait* if they trust the data it's showing them. To verify that, we built out a set of deliberate test cases confirming that per-pixel data transfers accurately from JMARS into *Planetary Parfait* and that imported layers align coherently with the underlying Cesium terrain. That means scientists can do precise work directly in *Planetary Parfait* without needing to separately re-validate the data on their own first.

**Publication**

I coathored an upcoming publication on the integration of the Cesium Moon terrain data with *Planetary Parfait*, and will include a link to the publication when it is released. Stay tuned! 

## Contributions

Working on the public build of *Planetary Parfait* I fixed and shipped over 20+ bug fixes, addressing issues reported by users on GitHub and issues found by our team's regular testing. I also implemented new features, such as giving desktop users the ability to wave and allowing VR users to toggle between having menus attached to their wrist or hovering in front of them.

*Planetary Parfait* gave me irreplaceable experience in Unity development and working on a large project with a team. It also  gave me experience with the SteamWorks software suite and the workflow of maintaining software on Steam. I am extremely grateful for my time on the project, and cannot wait to see where the project goes next.
