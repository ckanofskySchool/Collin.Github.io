---
title: Frame & Gearbox
subtitle: CAD & Construction of Chassis
image: assets/img/portfolio/Mechanical/8020FrameStart.png
alt: 80/20 aluminum frame assembly

caption:
  title: Frame & Gearbox
  subtitle: CAD & Construction of Chassis
  thumbnail: assets/img/portfolio/Mechanical/8020FrameStart.png
---

## Mechanical Planning

### Frame Planning
I started off my mechanical planning for the Robopack by thinking about the frame of the robot. From my previous robotics experience, I have learned that modularity and the ability to re-use and adjust is essential. To ensure optimal flexibility, I chose to use 80/20 tubing, specifically the 1530 & 1515 series tubing shown below for robust structure, flexible attachment points, and premade strong brackets that can be re-used and moved.

1530 Tubing                                                       15 Series Bracket                      
<img src="assets/img/portfolio/DailyJournal/MechanicalPhotos/8020_1530.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
<img src="assets/img/portfolio/DailyJournal/MechanicalPhotos/4350_90Bracket.png" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">

### Style Planning
After getting all the functionality down, I plan to use a nice wood to give a natural style to the robot. Another idea I have is to get plastic parts that can cover the machine as well, but that might be more complicated and expensive.

## The Build Journey

This is a journey through the build process describing how the Robopack's mechanical aspects came to be and the steps taken to get there. Enjoy!

### Setting Off — Brainstorming and Drawing

When I first set out to create the Robopack as a junior, I started by sketching out the ideal Robopack which I envisioned. With a stylus, my iPad, and Notability, I set out to draw how the Robopack would look, roughly diagram out how the electronics would work, and get a general idea of the features I wanted.

#### Robopack Design Stage

Do note that this was my first brainstorming and not everything was realistic or stayed the same.
<img src="assets/img/portfolio/Mechanical/FullConceptSketch.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
Note that while the frame was originally going to be made out of 2x4s for cost efficiency, I later chose to use 80/20 due to higher rigidity and versatility for future projects.
<img src="assets/img/portfolio/Mechanical/Frame_BucketSketch.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
<img src="assets/img/portfolio/Mechanical/WheelDesignSketch.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">

### Starting Up the Computer — CADing the Robopack

I used the CAD software Solidworks to design and model the Robopack. The reason I chose Solidworks over software like Fusion 360 or Onshape is due to the organization of Solidworks having a separate assembly vs. part studio, being proficient in the software as I do have a professional certification in Solidworks (CSWP), and because of the powerful features Solidworks provides such as rendering and more.

Before diving into the CAD, Solidworks is very picky about how and where files are saved and stored, so I started by creating a file system on a GitHub repo to store all my files neatly and easily. The layout is shown below:

GitHub
└── Robopack/
    ├── DocumentationPhotos
    ├── Electrical/
    │   └── Seeed Control Board Files
    ├── Mechanical/
    │   ├── Aesthetic Panels
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

<!-- Photos needed: 80/20 being cut on the chop saw, assembled bare 80/20 frame -->

## Gearbox Iterations

The first gearbox test worked but was extremely loud. I tried lubrication, then adjusted the motor-to-gear spacing. That helped a lot. The final fix was increasing the spacing correctly in CAD (I initially measured diameter instead of radius), which required a couple reprints but brought noise down to an acceptable level.

<!-- Photo needed: gearbox side plates and gear train -->


## Making the Wood Outer Frame

Here are the steps and workflow on how I bought, cut, prepared, stained, and finished my wood:

### Determining the Wood I Wanted

When I was searching for a nice outer shell wood for my Robopack, I had two main factors in mind. I wanted a wood that would look nice and presentable, but I also didn't want to spend an insane amount of money on the wood. After looking through some different woods on Lowe's website and talking with Dr. Taylor, one of the engineering teachers who is well versed in woodworking, I decided upon using Poplar Wood. Poplar wood is a lighter hardwood which is easy to work with and has a nice grain pattern. The one downside was that it has a greenish tint but the stain should get rid of that green completely.

At Lowe's, I bought 5 boards of 12"x24" .75" thick Poplar and 1 board of 8"x24" .75" thick Poplar. This worked out nicely because my Robopack's dimensions are 20"x24"x12" (w,l,h) so I would have less cutting to do. The total price of the wood order was ~$130.

I also proceeded to buy some stains in order to make the wood look darker and nicer. I bought Minwax Pre-Stain and 3 different Minwax stains to try some different colors and choose my favorite. I bought the smallest volumes they had but in the end it turned out to be the perfect amount.

### Cutting the Poplar Wood to Size & Adding Gaps

Once I had bought all the wood, I planned out where each piece would go and the size of which I would need to cut it to. Before buying I had done some planning to ensure I bought enough wood, but I double checked and laid out the wood before I started cutting it all. Here's the plan:

Left & Right Sides: 24"x12" (3"x3" gap in bottom corners for the 80/20 mounting plate) 
Front & Back Sides: 21.5"x12" (1/4" bevel on one side only on 12" edge to give less boxy aesthetic)
Bottom Sides: 24"x12" + 24"x8" to fill the 20" space (3"x1.5" gap in outer side corners for 80/20 tubing)

I also had to cut an 1/8" off several of the so-called 24" pieces because they were actually 24.125".

To make all the general shape cuts such as cutting the 24"x12" to 21.5" for the front and back, or correcting the 24.125" to be 24", I used the table saw.

To cut the gaps in the wood, I chose to use the bandsaw instead because the table saw would leave a circular groove in the wood, whereas the bandsaw cuts vertically so I can cut perpendicular angles. The downside was that the clamp on the bandsaw wasn't super straight, but I tried my best and got the cuts relatively straight.

### Prepping the Wood — Sanding

Once all the boards were cut and I ensured that they would fit their intended area, I began my long and difficult sanding journey to get the boards ready for staining. According to the instructions on the Minwax stain, I used 220 grit sandpaper on an orbital sander on every face of each piece — yes, even the edge faces and gap faces. It took like 2 hours in total I think. 

After using the orbital sander, I started sanding the edges & corners of each piece by hand, ensuring everything was nice and smooth, also a massive pain. I think I spent an hour or so and my hand was hurting a bit by the end, but I'm probably fine.

Finally, I used the router table and a 1/4" bevel bit to make the roundings on the front and back panels. While they turned out a little bit rough, I used the orbital sander with the 220 grit sandpaper and cleaned up the round making it look nice.

### Prepping the Wood — Pre-Stain

Once all my pieces were sanded, I brought them outside to do the final steps before staining the wood. I started by using a brush to get any big chunks of dust or woodchips off the boards. Then, I used a damp paper towel and wiped down each board to try and get all the sawdust off the boards leaving the board ready for the Pre-Stain coat.

Once the wood had dried from the damp towel, I applied the Minwax Pre-Stain I bought on the wood using a foam brush, and set a 10 minute timer. Once the timer finishes, it will be time to stain.

### Staining the Wood

After the Pre-Stain coat dried for 10 minutes, I applied my Minwax 232 Red Chestnut Stain which I had chosen as the color to stain my wood. I once again used a foam brush (different one than the pre-stain!) to apply a coat of stain on the wood. I tried to not make it too thick but I'm not sure if I did it that well, but I tried lol. After staining, I let the piece dry for a day.

### Finishing the Wood

Due to time constraints, I did not apply a polyurethane finish to the wood. The stain alone provides color but no protective layer — this is something I plan to address in a future iteration.
