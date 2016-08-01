---
layout: post
title: "Getting a part from SolidWorks to KSP"
description: ""
category: tutorial
tags: [solidworks,ksp,unity]
---

This post will detail the process needed to get from a solidworks model of a park to a basic ksp .mu file

# Required Programs

1. SolidWorks (Tested with 2014, later versions might not apply)
2. [SolidWorks OBJ Conversion Macro](http://www.solidsmack.com/resources/free-solidworks-obj-export-macro/)
3. [Unity 5.3.5f1](https://unity3d.com/get-unity/download/archive)
4. Kerbal Space Program (At least 1.1.3)
5. [Kerbal Part Tools](http://forum.kerbalspaceprogram.com/index.php?/topic/135228-parttools-for-ksp-112/) (It states this is part tools for 1.1.2, it is still valid for 1.1.3)

# Overview

This process can be broken up into two steps. Getting from Soldworks
to Unity, and then getting from Unity to KSP. The first step is simply
a matter of exporting to a file format readable by unity, namely a
.obj file. The second step involves some more detailed work setting up
the part model, textures, and placing relevant coordinate frames.

# Setting up Unity

*This section needs to be verified, steps may have changed*

1. Open Unity, create a new project
2. Copy the entire Part Tools folder into the unity project's assets folder
3. A "Tool" menu should now be available in the unity toolbar. Click Tools>>KSP Part Tools and point to the GameData folder of a vanilla version of KSP (no mods).
4. Go into the Part Tools folder in Assets and double click PartTools_AssetBundles, click import.
5. Should now be set up?

# Getting from SolidWorks to Unity

1. Make a solidworks part.
2. Run the SW OBJ exporter macro
3. Open a unity project that is already set up for KSP
4. Drag the entire obj folder created via the macro into the assets pane (both .obj and .mtl)

# Getting from Unity to KSP

1. Navigate to the material file in Assets and change the shader to a ksp shader
2. Add a texture. As an example: Drag a background texture from Squad/Kspedia/backgrounds to the texture panel of the material file
3. Create an empty game room under Hierarchy
4. Under inspector (with the game room selected) click Add Component>>KSP>>PartTools
5. Add model name and file path to write (example: GitHub/ksp-rocket-model/VADL/Parts/XXX/name)
6. Drag obj model into the game room
7. Select the game object. In the toolbar, go to Component>>Physics>>Mesh Collider.Point the mesh collider's mesh to the model mesh, make sure convec mesh is checked.
8. Click write in Part Tool component to make the .mu file

# Adding coordinate "Transforms"

Some modules require coordinate frames to know where to apply a force or otherwise act. For example, a motor part needs to specify where thrust is applied.

## Adding a thrust transform

1. In unity, add a game object *under* the model's game object (as a child)
2. Set the z direction (blue arrow) pointing in the direction of thrust
3. name the object "thrustTransform" and reference in in the .cfg file's ModuleEngineFX module

## Adding RCS transform

1. make a transform (unity game obj, see above section) for *each* thrust vector of the rcs system.
2. Thrust is in the *positive y* direction.
3. Name all transforms the same name so that onlyh one RCS module is needed in the .cfg file.

# Center of Mass

Set the coordinates of the model object in unity so that the model's Center of Mass is at the origin
