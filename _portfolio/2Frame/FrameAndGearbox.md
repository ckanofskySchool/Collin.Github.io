---
title: Frame & Gearbox
subtitle: CAD & Construction of Chassis
image: assets/img/portfolio/Mechanical/8020FrameStart.png
alt: Shirts on a hanger

caption:
  title: Frame & Gearbox
  subtitle: CAD & Construction of Chassis
  thumbnail: assets/img/portfolio/Mechanical/8020FrameStart.png
---

## Mechanical Planning

### Frame Planning
I started off my mechanical planning for the RoboPack by thinking about the frame of the robot. From my previous robotics experience, I have learned that modularity and the ability to re-use and adjust is essential. To ensure optimal flexability, I chose to use 80/20 tubing, specifically the 1530 & 1515 series tubing shown below for robust structure, flexible attachment points, and premade strong brackets that can be re-used and moved.

1530 Tubing                                                       15 Series Bracket                      
<img src="assets/img/portfolio/DailyJournal/MechanicalPhotos/8020_1530.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
<img src="assets/img/portfolio/DailyJournal/MechanicalPhotos/4350_90Bracket.png" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">

### Style Planning
After getting all the functionality down, I plan to use a nice wood to give a natural style to the robot. Another idea I have is to get plastic parts that can cover the machine as well, but that might be more complicated and expensive.

## The Build Journey

This is a journey through the build proccess describing how the Robopack's mechanical aspects came to be and the steps taken to get there, Enjoy!

### Setting Off - Brainstorming and Drawing

When I first set out to create the Robopack as a junior, I started by sketching out the ideal Robopack which I envisioned. With a stylist, my ipad, and noteability, I set out to draw how the robopack would look, roughly diagram out how the electronics would work, and get a general idea of the features I wanted.

#### Robopack Design Stage

Do Note that this was my first brainstormings and not everything was realistic/stayed the same.
<img src="assets/img/portfolio/Mechanical/FullConceptSketch.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
Note that while the fram was originally going to be made out of 2x4s for cost efficiency, I later chose to use 8020 due to higher rigidity and versatility for future projects.
<img src="assets/img/portfolio/Mechanical/Frame_BucketSketch.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
<img src="assets/img/portfolio/Mechanical/WheelDesignSketch.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">

### Starting up the computer - CADing the Robopack

I used the CAD software Solidworks to design and model the Robopack. The reason I chose Solidworks over softwares like fusion360 or onshape is due to the organization of Solidworks having a seperate assembly vs part studio, being pretty profficient in the software as I do have a proffesion certification in Solidworks(CSWP), and because of the powerfull features solidworks provides such as rendering and more.

Before diving into the CAD, Solidworks is very picky about how and where files are saved and stored, so I started by creating a file system on a github repo to store all my files neatly and easily. The layout is shown below

Github
└── Robopack/
    ├── DocumentationPhotos
    ├── Electrical/
    │   └── Seeed Control Board Files
    ├── Mechanical/
    │   ├── Asthetic Panels
    │   ├── Drive Gearbox
    │   ├── Frame
    │   ├── FrontWheelBoxes
    │   └── Main Assembly.sldasm
    └── Programming/
        └── V1RobopackCode/
            ├── RaspberryPi Code
            └── Seeed Code


## Current Build State
<img src="assets/img/portfolio/Mechanical/8020FrameStart.png" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
## Frame Build Notes (From Daily Journal)

- 11/10/2025: Cut 80/20 to length (2x 27" and 2x 17") using aluminum chop saw, then deburred.
- 11/15/2025: 80/20 hardware arrived and the main frame was assembled.
- Next mechanical step: cut vertical supports and finalize gearbox mounting hardware.

<!-- TODO: add photo of 80/20 being cut on the chop saw -->
<img src="https://placehold.co/1200x675/png?text=80%2F20+Cutting+Photo" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
<!-- TODO: add photo of assembled bare 80/20 frame -->
<img src="https://placehold.co/1200x675/png?text=Assembled+80%2F20+Frame" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
## Gearbox Iterations

The first gearbox test worked but was extremely loud. I tried lubrication, then adjusted the motor-to-gear spacing. That helped a lot. The final fix was increasing the spacing correctly in CAD (I initially measured diameter instead of radius), which required a couple reprints but brought noise down to an acceptable level.

<!-- TODO: add photo of gearbox side plates and gear train -->
<img src="https://placehold.co/1200x675/png?text=Gearbox+Assembly" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">






