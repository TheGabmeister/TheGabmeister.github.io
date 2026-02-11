---
title: White Tower
description: A fantasy castle I created in Unreal Engine based on a concept by SnowSkadi. My was to learn how to use the fog system. Featured in 80 Level.
date: 2018-12-15 12:00:00 +0800
categories: [3D Environment Art, Unreal Engine]
tags: [3D Modeling, Materials, Shaders, Lighting, Rendering, Unreal Engine]
media_subpath: '/assets/20181215'
image:
  path: /thumbnail-white-tower.jpg
  alt: White Tower
---

In today’s blog, I will share with you the details on how I created the White Tower concept by SnowSkadi in Unreal Engine. Hopefully, you will find new and insightful information that will help you in your future projects with UE4.

![Interface](/interface.jpg)

## Introduction

This was my second time creating a scene in UE4. I’m still in the process of learning the great amount of features this engine can offer you. My primary goal for this project was to learn how to use the fog system in Unreal, and it turned out, there was a lot more to learn. I did not intend to make the scene “game-ready.” I just wanted to explore more UE4’s features, utilize them someday in a future project, and share my findings with the community .

## The Reference: White Tower by SnowSkadi

Looking around, I stumbled upon this magnificent [concept by SnowSkadi](https://www.deviantart.com/snowskadi/art/White-Tower-69872081). The concept art to me looked like it came straight out of a Miyazaki film. I’m a big fan of fantasy RPGs and movies, so I was instantly hooked and wanted to recreate the scene in Unreal.

## Creating the White Tower in Unreal Engine
Here is a quick GIF showing the creation process:

![Work in progress](/wip.gif)

### 1. Blocking / White Boxing
I simply did the standard procedure. First, I setup the camera in a 3D modeling app (Blender in my case). After that, I started with a bunch of cubes, cylinders, and cones, modified their shapes, and moved them to their corresponding positions. I worked with larger shapes first. No need to create the details at this moment.  You can see from the screenshot below that I used basic placeholders so that I could judge early on the scale of each mesh and the relative distances to each other. Afterwards, I adjusted the camera and the meshes until I got something that matches the perspective of the reference image. With 100+ number of individual objects, I needed to combine everything as a single mesh to make auto UV mapping for lightmaps a lot easier. It’s a good thing to create backup files or copies of the original meshes before making drastic actions like this.

![Blocking](/blocking.jpg)

### 2. Importing the Mesh into Unreal Engine
I like to put my block meshes into Unreal as early as possible so that I could spot problematic areas early on. Since most of the castle is white, I could test lighting early which will primarily come from a directional light and a skylight. But before importing the block mesh into Unreal, here are two things I learned that I should have changed during the start of the project:

**First, Min Screen Radius for Lights**

This is a setting found under Edit > Project Settings > Engine > Rendering > Culling. What this does is that real-time lights are culled when they occupy a screen radius less than a given value. This is good for performance. By default, the value I think is 0.03. However, this caused some of the supplementary lights that I added near the end to suddenly disappear because the camera is very far from the light source. Adjusting this value to 0.02 fixed the issue.

![Min Screen Radius for Lights](/min-screen-radius.jpg)

**Second, Engine Scalability Settings**

This one is found under the Settings button in the toolbar. Depending on your type of scene, you might want to change some of these settings to a higher quality. For my case, I changed the “Effects” quality to Cinematic because it produced better reflections coming from the water below.

![Engine Scalability Settings](/engine-scalability.jpg)

As for actually exporting the mesh into Unreal, I used [renderhjs’ FBX Bundle exporter](https://renderhjs.net/fbxbundle/) (for Blender users, outdated as of 2019). This addon has batch export features as well as an Unreal Engine preset which saves a lot of time  and prevents all those rotation and scaling issues. As of this writing, I think this addon has some issues when exporting animation.

### 3. Adding a Sky sphere

The next step is to create the scene’s backdrop. This was a great learning experience since this was my first time setting up the sky, fog, and other stuff in UE4. These elements needed a lot of attention since they played a great deal with the mood of the scene.

I started with a completely empty scene and placed a **BP_Sky_Sphere** which you can get from the Unreal Starter Content. I adjusted the the parameters until the colors felt similar to the reference. An important setting to change here is to disable the **Colors Determined By Sun Position**. You want more control with the sky and sunlight by decoupling these two.

![Sky Sphere](/sky-sphere.jpg)

### 4. Adding a Water Plane

For this one, I used the lake water sample from the Water Planes project by Epic Games which you can download for free. I adjusted the parameters as shown in the screenshot below until I got something that was close to the reference image. Make sure that there’s at least one directional light in your scene, otherwise everything will be dark. The wireframe box you see in the screenshot below is a Lightmass Importance Volume, which in this case is not yet important. I just used it as a placeholder of the castle mesh.

![Water Plane](/water-plane.jpg)

### 5. Putting the tower into place and matching the perspective

I placed the tower into position and moved the **CineCameraActor** and adjusted its settings until I got the proper perspective. 

![Matching perspective](/matching-perspective.jpg)

### 6. Adding Directional light and skylight. Baking lighting.

In terms of the world lightmass paramters, I increased the **Num Indirect Lighting Bounce** and **Num Sky Lighting Bounce**. In a previous project, these parameters had a great impact in the brightness of the scene since light is allowed to bounce around a lot more. Because of this, I decided to do the same here. Additionally, I turned off **Ambient Occlusion** in the beginning, but at the end of the project I decided to turn it on. The effect, however, was not significant since in the end I used a certain degree of SSAO. Furthermore, I disabled **Compress Lightmaps** since this setting, while good for reducing memory and disk size, introduces artifacts. After setting up all these parameters, I added a directional light and skylight, adjusted their colors and intensity until I was satisfied, set both to **Static**, set **Lighting Quality** to **Production**, added a **Lightmass Importance Volume** over the entire mesh, and baked the lights. There were some hiccups in matching the direction of the sun light with the one in the reference image.

![Lightmass](/lightmass.jpg)

### 7. Adding an Exponential Height Fog

This one is a very cool feature. I knew from the beginning that volumetric fog was essential in making the ambience of the scene feel distant, mysterious, and magical. The nice thing about this is that I only had to adjust two settings from default: changed the color to sky blue and turned on the **Volumetric Fog** setting. The fog might look subtle at first glance, but when you combine this with particles and Post FX, you get an amazing effect.

![Exponential Height Fog](/exponential-height-fog.jpg)

### 8. Adding some clouds

Adding only the exponential height fog though was not enough to create the thick fog in the reference. So, I scouted for cloud particles and stumbled upon these cloud planes from the Unreal FPS sample projects. I just adjusted their colors, size, and location. As for the other parameters, I haven’t studied them in great detail. The funny thing about these clouds is that they’re constantly animated, and I’m still unaware on how to make them stop moving. This is the reason why the clouds in the screenshots keep on changing.

![Clouds](/clouds.jpg)

### 9. Creating the detailed mesh, adding materials, UV mapping. Rebaking lights in UE4.

Modeling all those small details took the largest chunk of the total time it took to finish this project. There’s no shortcut here. Just pushing and and pulling vertices for several days. Because of the large number of objects, I combined meshes until I had only 14 large meshes. At this scale, having fewer objects make it easier to do a couple of things: auto UV mapping, managing materials, exporting to Unreal, and adjusting lightmaps. I created two UV maps for each object: one for the textures, and one for the lightmaps.

![Detailed mesh](/detailed-mesh.jpg)

**Materials**

This is an aspect of the project that I believe could be improved a lot. My texturing skills are not quite a good as my modeling and lighting skills. The materials are very basic: a wall and a roof material with tiling textures,  two glass materials with a roughness map taken straight from the default Cloud texture in Photoshop, an ornate texture I created in Substance Painter, and a couple of different colored flat materials. I will discuss at the end of this blogs the things that can be improved with the materials.

![Materials](/materials-scene.jpg)

### 10. Adding decals, reflection capture, and supplementary lights
This was my first foray with Unreal’s decal system which is another interesting feature. There are a bunch of tutorials online on how to create UE4 decals. For my case, I downloaded some stained moss textures online and edited them in Photoshop. Afterwards, I made a decal material and placed a big DecalActor right on top of the water to simulate the moss on the bottom of the castle. I also sprinkled a few more DecalActors here and there just to add variety. Good thing there’s a handy checkbox **Receives Decals** which prevented decals from appearing on the water plane.

![Decals](/decal.jpg)

![Decals](/decal-1.jpg)

I could add a lot more decals to make the castle walls look more interesting, but at this point the project was already taking more time than what I had alloted for it. After decals, I added one big reflection capture so that all those windows could display some reflections on them. I also placed a couple of point lights and spotlights to places I found to be too dark.

![Supplementary Lighting](/supplementary-lighting.jpg)

### 11. Adding Screen Space Ambient Occlusion (SSAO) Post FX

For Post-processing, I did not use a post-processing volume. Instead, I edited the settings within the camera. No reason specifically. 

Screen space Ambient Occlusion (SSAO) is something I added but not quite sure whether it was the right move or not. This allows you to add contrast and show more of those details that look flat due to lights.

![SSAO](/ssao.gif)

### 12. Lastly, adding Bloom and Depth of Field Post FX

This was a tough one. I spent a lot of time adjusting these parameters since these were essential in making the scene feel like it’s far away and mystical. Shown below are the settings I used. I feel that there’s room for improvement in this regard. There were multiple instances with the DoF effect where the intensity I was seeing from the viewport was very different from the one I got from the built-in High Res Screenshot feature. I still don’t know why this happened.

![Bloom and DoF](/bloom-dof.gif)

## Conclusion

This was a great learning experience. In reality, there was a lot of back-and-forth in terms of adjusting the parameters of the materials, lights, and post FX until I arrived at the final image.

**Areas to improve in the scene**

In terms of rooms for improvement, I believe that the materials can still be enhanced. My texture creation skill is currently lagging behind my modeling and lighting skills. You might notice that the castle walls look a bit too plain, and some of the window mirrors don’t look convincing. Moreover, it’s possible to add more decals to the castle walls to show more deterioration and make it look like it stood the test of time. Creating convincing materials, however, is an area that I haven’t studied thoroughly yet. Maybe in a future project I will dedicate more time in improving the materials.

Secondly, the Bloom effect did not turn out as I expected it to be. I really wanted the top most tower to have a more intense Bloom; however, this didn’t work. I suspect that my lighting setup did not allow me to achieve this effect. Intensifying the Bloom effect made the other parts of the tower overblown.

Thirdly, some of the wall edges appear too sharp. I’ve seen some decal techniques in Polycount and ArtStation that can mitigate this issue. I’ll definitely do some more research regarding this.

Lastly, there is an issue with the reflection on the lower left of the final image:

![Broken reflections](/broken-reflections.jpg)

I’m not entirely sure why there is a mismatch in the reflection and the mesh above the water . If you have an idea on how to fix this, let me know!

## 80 Level Article
Huge thanks to 80 Level for featuring this! Check out the article [here](https://80.lv/articles/creating-a-miyazaki-style-tower-in-ue4/).

![80 Level Featured](/80-lv-featured.jpg)