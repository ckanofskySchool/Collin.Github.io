---
title: Topographical Map Project
subtitle: CNC Topo Map Project
image: assets/img/portfolio/TopoMap/CutTopo.jpg
alt: 

caption:
  title: Topographical Map Project
  subtitle: CNC Topo Map Project
  thumbnail: assets/img/portfolio/TopoMap/TopoMap.png
---

# Project Description

The goal of this project was to take an area of the world, some geographical feature and CNC the geography on the Carvera CNC machine.

# Final Files

[STL Topographical File](assets/Files/TopoMap/TopoSTLterrain.stl)
[Aspire crv3d File](assets/Files/TopoMap/CollinKMountainCuts.crv3d)
[.cnc Cut File](assets/Files/TopoMap/CollinKMountainCuts.cnc)

# Daily Step Progression


### 11/12/2025 

Today I used the software https://jthatch.com/Terrain2STL/ to create a 3D topography map for a quick detour porject teaching 3D depth CNC cutting. For my model, I chose to make the topography of a mountain called Cammleback in Pheonix, Arizona as a birthday gift to my grandma who would always take us hiking there. I 3D printed the model when it was done and looked good.

<img src="/assets/img/portfolio/TopoMap/Topo3Dprinted.JPG" width="320" height="320">

### 11/17/2025

Today, I worked some more on my Topography Map of Cammleback Mountain, specifically, I learned using an example file how to create and export the toolpaths for this project using the CAM software "Aspire". At the end of the day, all I had time for was importing the stl and I will make the toolpaths soon. To do the setup, I followed this super helpful [workflow](assets/Files/TopoMap/TopoMapWorkflow.pdf) by Dr. Taylor.

### 11/20/2025

Today, I finished my Aspire topography toolpaths and updated my documentation to have more info on the CNC milling proccess and update some of the past days which were a bit behind on documentation.

### 11/11/2025

Quick update from yesterday, I finally cut out my topography file as shown below, I was a bit concerened about the CNC breaking due to the depth of which my file went and the vaccumm shoe being in the way, but Dr. Taylor helped me lock the Vaccumm shoe higher up which made the cut a succsess. 

<img src="/assets/img/portfolio/TopoMap/CutTopo.jpg" width="320" height="320">

# Project Summary

This project was really cool because I learned more about the Carvera CNC machines and how to use Aspire in conjunction with Carvera to cut the topo map. I also learned about the importance behind toolpath roughing vs finishing and how one can durrastically reduce time needed by using a roughing pass first and a finishing pass at the end.

One of the main challenges I encountered while completing this project was the Vaccuum broom being too low and having the potential to break due to the g-code. The broom has a spring to go up but nothing in the case of which the brumm metal slide is hit on the side. To fix this, Dr. Taylor bravely risked his life to lift the vaccum brush for me when the machine was paused. We later discovered that the M5 and M3 manual g-code commands would let us stop and start the spindle, which the pause button did not do.

In the future, I want to try using a longer milling bit and make a deeper cut topo because while the mountain looks cool, its width and length are forced to be small because of the depth limit. If I use a longer bit, I would be able to reach deeper and make the base larger making the whole piece more impressive looking.