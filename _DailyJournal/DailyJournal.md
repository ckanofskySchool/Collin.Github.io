---
title: Daily Journal
subtitle: Everyday documentation of robot creation
image: assets/img/portfolio/04-full.jpg
alt: 
---

## 9/16/2025
Today, we learned about Git, Github, Github desktop, and the browser github as well. Specifically, we focused on how changes are stored in Git and the ability to create branches of the repo for development without affecting the original branch.

## 9/17/2025
Today, we practiced with Git and created shared Repo's with each other to practice with collaborator settings on github.

## 9/24/2025
Today, I decided to order the essential electronics for my project, this included the motor controllers, the batteries, and the safety mechanisms.

## 9/25/2025
Today, we did a soldering activity where we practiced surface mount and through hole soldering using a halloween owl kit which lights up when you touch it. The surface mount soldering, internal components, and the finished product are shown in pictures below.

## 9/26/2025
Outside of class today, I used the AI Claude with the Opus4.1 model to generate some starting code for my project. The programs I had made were a RP2040 Seeed program which will control the motors, and a RaspberryPi + AI Camera program which will hand the tracking aspects and math of the camera. I am using the AI formulated code as a launchpad in order to be more efficient and spend more time on customization and troubleshooting, than have to spend that time on writing the base code myself. However, to ensure I understood the code and would be able to edit and modify it, I made sure to go through each line and ensure that I understood why and what the line did, and how modifing that line of code would impact the overall preformance of the program. The initial prompts I inputed are shown below.

### Prompt for RP2040 Seeed Code

```write me an arduino code for a RP2040 Seeed which will take input from the usb serial coming off a raspberry pi and move 2 drive motors on my robot using pwm(like how servos are controlled) according to the directions. For input directions, there should be a heading which if is outside the middle range set by me in a variable, then the program should turn until the the heading is once again within acceptable values. Another input will be the distance from the target which also should be kept within acceptable range set by me in a variable. Lastly, have a serial monitor where I can input values for left motor and right motors to test if I want. One last thing is whenever motor powers are set, send back what has been set across the usb serial for the Pi to recieve.```


### Prompt for Raspberry Pi Code

```can you now right me a raspberry pi program in pythong prob. that will provide these inputs needed based off a raspberry pi 5 and a raspberry pi AI camera attachment. Also have the raspberry pi host a webserver that allows the user to see the motor power assignements and camera feed. there should be a simple but neat UI for that web page pls.```

## 10/6/2025

I have done some thinking and realized that I was going too big on the first code attempt, so I backtracked a bit back to the Raspberry Pi AI Camera Documentation, and completed the instructions as well as ran the example program:
``` rpicam-hello -t 0s --post-process-file /usr/share/rpi-camera-assets/imx500_mobilenet_ssd.json --viewfinder-width 1920 --viewfinder-height 1080 --framerate 30 ```

This program worked amazing and highlighted me with a bounding box as well as providing a percentage of certanty which seemed to stay above 50% consistantly.

## 10/7/2025

### Layouts
Today, I worked on the electrical for the robot, mainly trying to get power and control to 1 motor for testing. The layout of main power in my electrical testing setup went as such: 
Battery(12v 15000AmH) --> Breaker(30A) --> MotorController(Koors40 Brushed DC Motor Controller) 
--> Motor(CIM Motor) 

Then, I had to have a seperate control setup which would tell the motor controller how to move the motor. I also had to wire up a 5V power source into the receiver because the PWM output from the motor controller didn't provide a 5V port, just signal and ground wires. The layout of the control setup went as such:
Controller(Wireless RC Plane Controller) --> Reciever(RC Plane Reciever) --> MotorControllerPWM(Koors40 Brushed DC Motor Controller)

I haven't built this part yet because I chose to focus on the main power layout.

### Making Connections

The main focus I had when setting up the hardware was modularity. I want to be able to replace components if I need to, or swap them out wihtout having to unsolder or cut wires, so I decided to use Wago connectors which take to unsoldered raw wire ends, and clamp down a metal plate on the wires forming a connection between the wires on both sides. These connectors worked great, but I sadly only snagged 2 from my robotics team, and I inevitablly ended up needing 4, so I put the final wiring on hold until I could get some more Wago connectors.

I also had to do some soldering to connect the battery to the whole system. I bought some EC5 Connectors off of amazon and soldered them on to 12Ga wire which allowed me to connect the battery to the breaker and the whole system.

## 10/8/2025

Today, I found the github repo for the rpicam-hello program and ran it as a python file which allowed me to make edits to the code itself. I then used the AI ChatGPT from OpenAI to add in code for outputing where the human is in the serial terminal. I chose to use ChatGPT because Claude didn't seem great at modifing the codes without re-writing everything and that would always end up casuing errors. ChatGPT had me edit one of the functions and everything else stayed the same so when I tested the new code, it worked great and I was able to route the output to my computer terminal temporarily, so I could see the output without another device on the other side of the serial connection.

Here is the original code: [rpicam-hello](https://github.com/raspberrypi/picamera2/blob/main/examples/imx500/imx500_object_detection_demo_mp.py)

Here is the updated code which I developed with AI assistance: [modified rpicam-hello]()

This code worked amazing and had very simular functions to the example, but a few key modifications to make it more effective and accurate for my use:
- Added a stabalizing function
    - The data would update every frame and the bounding box would move after every update, still correct, but I don't need that much precision of 5 pixel shift to the left or whatever, so I insterted a snipet of code which only changes the detection bounding box if the center point has moved by >25 pixels, or the size has changed by 20%. This helped smooth the detection and give a more stable instruction which will be easier for the robot to follow.
    
- Added output to Serial for the Seeed board to receive instructions
    - In order to tell the Seeed board where the person is, I outputed a formated set of data which gives the centerpoint x and y position, as well as the width and height of the bounding box. The format goes like such: x,y,w,l
    - By changing the serial port from 3 to 1, which is the computers port, I was able to visualize the output on the terminal and confirm that the code was working correctly.

### Video of the Code Functioning

<video width="320" height="240" controls>
  <source src="assets/img/portfolio/DailyJournal/ObjectDetectVideo.mp4" type="video/mp4">
</video>

## 10/9/2025

Today, I continued working on the electrical testing setup I started on 10/7/2025. I acquired 4 more Wago connectors from my robotics team and finished up the connections between the motor and the motor controller using them. After finishing the main power setup, I started building the control wiring setup. I used a 5V power supply to power a remote control plane reciever which outputs a pwm signal, used to control the motor. Then I attached the pwm signal and ground to the reciever on the throttle port of the reciever and was able to control the motor using a RC Plane Remote.

### Video of motor moving

<video width="320" height="240" controls>
  <source src="assets/img/portfolio/DailyJournal/Motor moving.mp4" type="video/mp4">
</video>

## 10/10/2025

Today, I made some orders to make sure my final product would be safer. This order mainly included anderson connectors for modular connections, 12Ga Wire spool, Power Distribution bus that will protect the 12v battery power from contacting anything, but also distribute it to 5 other places.

## 10/13-24/2025

Over the course of this week, I have moved my website from my boring markdown base template to a Jekyll agency template which is more stylish, cleaner, and has interactive buttons and such to make navigation simpler. 

I also ordered some gears from Vex so I can test my gearbox next, now that I can control the motors. Not sure when they will get here but I plan to 3D print the gearbox panels for now and eventually make them out of aluminium with a CNC machine.

## 10/28/2025

Today, we learned how to use MakeraCAM, a CAM software to take PCB designs and mill them out on Carvera milling machine in our lab. Luckly, I had already designed myself a PCB board for the robopack where the Seeed RP2040 would be, so I decided to take a risk and make my first project my final one. 

The main challenge I had to consider with creating my final project board was that the board itself was a two sided board, meaning I would need to do two seperate cut files, but align them perfectly for the PCB to work. I overcame this challenge through making the back a mirror at the same position as the front. What this means is that when I cut, I will cut the front parts first, then flip the board and the back cuts will align with the front hopefully, due to the "mirror line" which is set at the boards width(127mm) divided by 2, which means the two mirrored paths should align.

## 10/29/2025

Today, I finished up my CAM files and cut the board for the robopack. Sadly, I encountered a few issues with cutting double sided boards that I didn't anticipate:

- The biggest issue I ran into was the challenge of getting the board to be in the same position when flipped. If the board is even .1mm off, the holes might not align and the board needs to be remade. This was a challenge I ecountered on my first try, but was able to conquer through focusing the placement and pressure on only the bottom face, which actually revealed that the PCB material was not cut at an exact perpindicular angle.

- Another issue I ran into on my first try was that after the front side had cut, when the back side file was auto-leveling, it applied enough force where the material bent and caused a section of the depth on the board to be incorrect. This resulted in a portion of the board missing cut areas due to incorrect height. To fix this, I increased only the back side trace file depth by .05 which in theory, should account for the false reading in leveling, and during my second board attempt, guess what?! it still failed(I was sad). Out of time as well so I shall continue the struggle tommorow. FIGHT ON!!!

## 10/30-31/2025

### Created Workflow - By: Collin Kanofsky & Aaron Dunnigan

File Setup:
- Open the MakeraCAM
- In the top left, under “File”, click “import PCB”
- Select one of the files and click open
- Open a many files as you need D=F
- The file might appear outside the work area

Toolpath Creation:
- Select everything, then deselect the outer edgecut line by zooming in and holding shift while clicking
- For 2D paths like edge cuts, create a 2D pocket
- Use example tools, .8mm corn and .2 30Deg Engraver(metal) on pcb
- Calculate at the bottom
- For 2Dpaths create 2D drilling
- Use example tools, 0.8mm corn 
- Calculate at the bottom 
- For 2D paths create 2D counter 
- Select 0.8mm corn 
- Go to tabs
- Click custom 
- Select where you want your tabs
- Calculate at the bottom
- Preview and Export:
- Click the preview button 
- Select all toolpaths 
- Watch the preview
- Alter the speed of the preview for what works best for you
- Exit preview 
- Click export 
- Select all tool paths 
- Export file 
- Name the file "lastname first initial resistor.nc” 
  - Ex: DoeJresitor.nc
- Make sure it is an nc file or g-code



### Experienced User Workflow - Credit: [Angelina Yang](https://fabacademy.org/2024/labs/charlotte/students/angelina-yang/about/)

#### Key notes:
- .8mm Corn flat-end bit is used to remove the bulk of the material
- The .2mm*30ºEngraving(Metal) engraving bit will be used to cut out the copper traces
- The Makera Milling machine affixes the FR4 using clamps, rather than adhesive, so tabs are necessary to keep the PCB in place.
- 2D contour is used for edge cuts 
- 2D pocket is used for copper traces
- 2D drilling is used for drill holes  

#### PCB Toolpath Workflow on MakeraCAM
- Open MakeraCAM on your desktop.
- Select the “3-AXIS” option on the welcome screen.
- Edit the “Stock” settings in the top right corner
- For Material, select “PCB”
- For Length(X), adjust the value to 127mm
- For Width(Y), adjust the value to 101mm
- For Height(Z), adjust the value to 1.7mm (thickness of FR4)
- In the top toolbar, click the icon titled “import PCB” and individually insert all Gerber files into the workspace.
- The imported gerbers will likely populate outside of the workspace, so select all 2D layers, hover over the “Adjust object” and “Transform” drop-down menu and select the  “Move” tool
- When layers are dotted, that indicates that they are selected; when layers are solid, that indicates that they are unselected
- Select the bottom left corner as the anchor point
- Set both the X and Y location values to 6 mm, which positions the file in the bottom right corner of the workspace
- Keeping all layers selected, hold the shift key and deselect the outer edge of the Edge_cuts
- Toggle the visibility such that only the “F_Cu” and the “Edge_cuts” layer are visible 
- In the top toolbar, hover over the “2D Path” drop-down menu and select the “2D Pocket” option
- In the dialogue box, adjust the “End Depth” value to .05mm
- Under “Tools,” click the “Add Tool” button, select “.8mm Corn tool” and click “Choose”
- Click “Add Tool” again, select the “.2mm*30ºEngraving(Metal),“ and click “Choose”
- Ensure that the material selected is “PCB”
- Click “Calculate”; you should see a “2D Pocket” toolpath fall under the Path dropdown in the hierarchy
- If you have drill files, untoggle the visibility for all “F_Cu” and “Edge_cuts” layers and toggle visibility for all drill files 
- In the top toolbar, hover over the “2D Path” drop-down menu and select the “2D Drilling” option
- In the dialogue box, adjust the “Drill Tip End Depth” value to 1.7mm
- Under “Tools,” click the “Add Tool” button, select “.8mm Corn tool” and click “Choose”
- Click “Calculate”; you should see a “2D Drilling” toolpath fall under the Path dropdown in the hierarchy
- To design a toolpath for the edge cuts, untoggle the visibility for all drill files and toggle visibility for solely the “Edge_cuts” layer
- Select the inner outline of the “Edge_cuts” layer
- In the top toolbar, hover over the “2D Path” drop-down menu and select the “2D Contour” option (synonymous with a “Pocket” cut)
- In the dialogue box, adjust the “End Depth” value to 1.7mm
- Under “Tools,” click the “Add Tool” button, select “.8mm Corn tool” and click “Choose”
- Under “Strategy,” select “Outside”
- Under “Tabs,” select “Custom,” and click “Add”
- Add appropriate tabs around the selected “Edge_cuts” layer (typically, 3 will be sufficient)
- Tip: Ensure that these tabs are staggered and not directly across from one another
- Click “Calculate”; you should see a “2D Contour” toolpath fall under the Path dropdown in the hierarchy
- In the top toolbar, click the icon “Preview Toolpaths,” and select all toolpaths in the pop-up dialogue box
- Click “Preview” and press the play button to view a simulation of the toolpaths 
- In the top toolbar, click the “Export” button, ensure all toolpaths are selected, and click “Export”
- Rename the .nc file to your last name, your first initial, and your project name, followed by “gcode”


#### PCB Milling on Carvera Controller
- Open the Carvera Controller software on the desktop
- In the top toolbar, click on the button with the status “N/A disconnected”
- Select the appropriate COM port to connect the Carvera to the computer (if the COM port is already connected, leave it as is)
- In the menu in the top right corner, click “Switch to display manual control interface” followed by the “Home” button 
- Under “Tool Status and Control,” ensure that the probe is charged to at least 3.6V (this ensures the machine operates in the z-axis as intended)
- In the bottom left corner, open the G-code from your files 
- Before starting the mill, open the menu in the top right corner and click the “Switch to display file preview interface” to preview the toolpaths 
- Click “Config and run,” and ensure that both the “auto vacuum” and “auto leveling” options are on.
- Once all settings are verified, click “Run”

## 11/3/2025

TODAY I FIXED MY WEBSITE!!! This exact page of daily journal documentation is finally visible and working, i have been struggling with getting it all working and with the help of AI and persistence, I finally fixed the formatting and got it all working. Lets goooooo!

## 11/4/2025

Today, I printed the doublesided PCB mount I had designed a bit ago and ordered the bearings for my senior project, I also reviewed what left I had to do before ordering the bulk of the cost, the structure. Below is the list I made:

- Test Gearbox Design
- Cut Custom 2Sided PCB
- Integrate Seeed with Pi to communicate with eachother
- Design fancy side plates/Decide material

Pretty chill day for me

[PCB Board Design Files](assets/Files/Electrical/SeeedControlBoard.zip)
[PCB Board CAM files](assets/Files/Electrical/SeeedControlBoard-F_Cu.zip)

## 11/6/2025

Yay! The board succsesfully cut using the 3D printed Jig I made. By securing both sides of the board, the cut was much more acurate. However, there was still some inacuracy and for some weird reason, a part of the board got hollowed out a bunch. Not sure why but it will still work!

## 11/7/2025

Uh oh... so I might have tried to attach the 2 sides of the board with solder alone... and might have somehow burned the traces off the face of the earth on one side... so today I'm re-milling the board and next time, I am going to use pins in the holes and then trim the ends to connect the sides.

## 11/8/2025

I once again set out to remill my board, and this time, the holes lined up even closer than before. After milling, I used pin headers through the holes, which were a perfect friction fit to keep the pins held in place without holding them, and soldered on the open side where the pin and pad were available. Once soldered, I flipped the borad over and cut off the plastic and extra end parts of the pin header, then soldered those ends onto the board completing the connection. Whle this method had a slight flaw that the circular pads around the hold which weren't alligned with the holes would have the risk of tearing the hole, I got lucky and while some tracers almost disconnected, all of them remained relatively intact. Below is the board and the step I went through to make the soldering work:


<img src="assets/img/portfolio/DailyJournal/SeeedBackPinsPlaced.jpg" width="320" height="240">

<img src="assets/img/portfolio/DailyJournal/SeeedFrontPinsSoldered.jpg" width="320" height="240">

<img src="assets/img/portfolio/DailyJournal/SeeedFrontSoldered.jpg" width="320" height="240">

<img src="assets/img/portfolio/DailyJournal/SeeedBackSoldered.jpg" width="320" height="240">

## 11/13/2025

Today I tried to program the seeed board but I think I started to complicated because while the program itself seemed to run great, none of the LED indicators on the board were lighting up to show that power was succsesfully being outputed to the motors. 

## 11/14/2025

Today, I focused on obtaining the gears for the drive gearbox of my capstone project. I asked Mr. Dubick to order these through VEX and they came within 3 days of ordering. Once ordered, I then worked on 3D printing the sideplates of my gearbox for testing the gear ratio in the future.

## 11/18/2025

Today, I took the 3D printed side plates for the gearbox and installed 1/2" hex falnged bearings into them to be ready for assembly. Sadly even though the gears arrived, I was unable to assemble the gearbox due to not having the correct hardware and shafts, so I instead made a BOM for what I still needed to get this gearbox up and running.

## 11/18/2025 Outside of School

After my robotics, I used a chop saw to cut 1/2

## 11/20/2025

Today, I finished my Aspire topography toolpaths and updated my documentation to have more info on the CNC milling proccess and update some of the past days which were a bit behind on documentation.

## 11/25-12/9/2025

I will admit that I have not been doing day to day documentation recently, but this is because I am trying to make the most of my class time working on my capstone project. 

More specifically, I have been working on the gearbox for the Robopack. I bought the hardware I needed and put together my first gearbox test to see if the gearbox worked. Good news and bad news... The good news is that the gearbox works, and that the output looks like it will be the right. The bad news is that the gearbox was really loud, and at full speed it hurt my ears to be near it, so I definetly need to fix this.

The first solution I tried was to lubricate  the gearbox, because lubricating the gearbox should  quiet the interaction of the gears and make everything run a bit more smoothly. This solution might have had an effect, but it was hard to tell because the gearbox was still really loud.

When lubrication didn't have the effect I wanted, I started to suspect that maybe the gear spacing was too close, and the extra forced contact was creating the extra noise. After some fidgeting and experimentation where I pushed the CIM motor away from the gearto add a small bit of space between the drive gear and the 1st gear, I noticed a significant decrease in noise. I tested this a few times, and decided to add some slop in the gearbox to relieve the tension between the gears.

This added space between the gears significantly reduced the noise of the gearbox, but sadly when I made the change in CAD, I made the rookie mistake of not double the spacing due to a diamater dimension, not a radius dimension. Therefore, after two reprints and designs, I had finally gotten the noise of my gearbox to a low and acceptable level.

I have also been working on the electical crimping on the side when I have nothing to do but wait for my 3D print to finish. Sadly though, the crimps I bought wouldn't fit into the connector. I'm not sure if this is a result of buying cheap crimps off of amazon, or if it's because I didn't have the correct crimping tool and was just improvising with some pliers and a slightly too small crimper. Either way, I have ordered a new set of crimps that come with a crimping tool. oops though bc I sent it to my sister in georgia... she's coming home friday so I will get them from her then. 

## 11/10/2025

Today, we had a very sad loss...

<img src="assets/img/portfolio/DailyJournal/MechanicalPhotos/Order.png" width="320" height="240">

The reason for which I have dropped 200$ on the harware for my project is because yesterday at robotics, I cut the 80-20 Material for the base of the Robopack, which was the 2 27" pieces and 2 17" pieces. I cut these using a Aluminium Chop Saw, and then used a grinder, debur tool, file, and some helpfull teamates to clean up the edges and make them smooth. Shout out to my robotics mentor Ray Kimble for overseeing the machining proccess and making sure I was cutting safely and correctly!

<!-- Pictures from phone of machinery here. Also video of me cutting tonight-->

At school today, I cleaned up the dust and residue from the 80-20 which sat in storage for probably like 10 years with my friend aaron, and then sent the order to 80-20 for the hardware shown at the start.

Due to waiting on the 8020 order & the crimping materials to come in, I will be focusing on the power connection soldering, and the programming in the next coming days.

## 11/11/2025

Quick update from yesterday, I finally cut out my topography file as shown below, I was a bit concerned about the CNC breaking due to the depth of which my file went and the vacuum shoe being in the way, but Dr. Taylor helped me lock the vacuum shoe higher up which made the cut a success. 

<img src="assets/img/portfolio/DailyJournal/2x4topo.png" width="320" height="240">


Today, as I wait for my crimps and the 80-20 materials to come in, I decided to fix the slight issue in the gearbox CAD which made only one of the 4 holes on the motor align, and then started printing a gearbox for both the left and right side so I am ready to attach when the 80-20 hardware arrives. My hope is that when the 80-20 stuff arrives, I can quickly build the main aspects of the robot and have the spacing and layouts to do my electrical wiring. Things are looking good so far!

## 11/15/2025

PARTS ARE HERE!!! my 80-20 hardware arrived and I assembled the main frame of the Robopack shown below. Sadly I have yet to cut the vertical supports which I plan to do Tuesday night.

I also ran into another tragedy. I ordered all the hardware needed for the brackets I bought, but I forgot to order hardware for the gearbox plates. The saving grace is that I releazed that I not only need gearbox hardware, but also hardware for electrical panels, side panels, ect. so I decided that my next mechanical step was to step up and finish the CAD model completely, ensuring I account for all needed hardware in my next order (rip the additional 20$ shipping fee bc I didn't order these in the 1st delivery).

## 11/16/2025

Today, I worked on organizing my documentation a little bit as well as documenting the topography project. I now show side projects in an appropriate portfolio page instead of being spread within my main daily journal.

I also started the fun task of taking the loads of info within this journal, and putting them in organized sections and formats of the portfolio. So far, I have updated the timeline, the goals, and the hardware section(images fixed in workflow too)

## 1/6/2026

So... I have no idea where the rest of my previous journals went, I might have deleted them by accident but I couldn't find them. As an update, I have assembled my 8020 frame(not including gearboxs sadly), ran a successful test of both motors using the seeed manual control, and tommorow I plan to order more 8020 hardware for the motor mounting, side panel mounting, and electrical mounting.

Today, I mainly focused on building a [Task Manager spreadsheet](https://docs.google.com/spreadsheets/d/1YxHaPIC824Zfynl9WW5YS0Lov2N1IcNKjxM6ScYHs5I/edit?usp=sharing) which keeps track of my tasks, and also helps me manage my time wisely with the Gantt Chart section. One cool feature is that their is a simple tab that only has tasks and their state of completion, and then those states auto update onto the Gantt chart in the second tab for more advanced tracking.

## 1/7/2026

Today I finally laser cut my electrical panel. When I had tried in the past, I kept running into the issue of the logo being a bunch of lines that wouldn't allow me to fill the inside. To combat this issue, I instead used inkscape to take the dxf and fill the inside because I am more familiar with the inkscape software and then exported a png image of the filled-in logo. This allowed me to bring it into correl draw and trace a logo bitmap of the image, which I then centered with the dxf file of the plate and sent it to the laser cutter.

One slight bump that I hit after laser cutting was that I left the tape sheet on the acrylic to prevent burn marks on the final result. However, I should have only left the tape on the bottom of the acrylic because when the engraving happened, each pixel of the engrave became separated from the main tape sheet, meaning when I pulled off the sheet, the letters were still filled with the tape. To fix this without damaging the acrylic, I used a chisel to scrape the tape out of the letters but it did take me an additional 10 minutes of work.

After the panel was all good and correct, I applied 2 17 inches velcro strips across the top and bottom of the panel for electronics to mount, and added velcro onto all the other electronic components to attach them all together on the panel.

The only components that still need attachment are the breaker, because I want to make it stand straight up, and the seeed RP2040 board because it requires a special case to allow velcro attachment, which I have yet to CAD.

## 1/8/2026 - 1/12/2026

During these days, I finished up the wiring on the Velcro electronics board and made a special 90-degree 3D printed mount to attach my breaker to. I ended up just screwing the breaker into the 3D printed part which worked surprisingly well so I left it at that for now. Below is the final electronics setup minus the Seeed RP2040 board bc I wanted that on the side for programming the aiming algorithm.

## 1/13/2026 - 1/15/2026

During these days, I got the Raspberry Pi and Seeed RP2040 working together and had a decent(I hope) tracking algorithm that seemed to respond to my position in relation to the camera. It was kinda hard to tell how well the algorithm worked without the camera moving on the robot itself, so I decided my next priority would be to make the wheels for the robot.

## 1/16/2026

Today I molded the wheel, which was one heck of an experience. I started off with my 4 part 3D printed mold, which then has a center hex peg and a bottom cover/clamp to hold all 4 sections together and under pressure. Then, a 3D printed hex shaft hub was inserted which would eventually be the wheel. To start the molding process, I mixed 100 milliliters of Reoplex 30A part A and 100 milliliters of Reoplex 30A part B, and proceeded to started to pour slowly into the small crack of the wheel mold. I had to poor pretty dang slowly because my opening wasn't very big and if I poured too fast, the material would start to overflow out of the mold.

As I was pouring in the material, a critical issue appeared. The material was leaking out of the bottom of the mold, at the small gap between the wheel hub and the bottom of the mold. Unsure of what to do, I tried to push down on the wheel hub and pour at the same time, which seemed to stop the leak, but my arm was starting to get tired. Suddenly, I had a genius idea, I asked a friend to grab me the 30lbs toolbox full of wrenches and wratches, a roll of ducktape to give space to pour and built myself a on the fly weight press to stop the leaking mold material. It worked too!

With my issue fixed I proceeded to pour into the mold, with each minute the mold material got less fluid and harder to move, but I was able to fill the mold just in time and put a nice extra coat on top to ensure even if the level of mold went down a bit, the wheel would still have adequate material.

<img src="assets/img/portfolio/Mechanical/WheelMolding/MoldingSetup.jpg" width="320" height="240">

<img src="assets/img/portfolio/Mechanical/WheelMolding/AfterPourChaos.jpg" width="320" height="240">


## 1/20/2026

I spent today removing the molded wheel from the mold itself, which was a bit of a struggle, but not terrible. Once I got the wheel out, I used a wire snip and knife to clean up the edges and the wheel turned out great. Based on the result, I decided to use this molded wheel technique for all 4 wheels.

<img src="assets/img/portfolio/Mechanical/WheelMolding/MoldCured.jpg" width="320" height="240">

<img src="assets/img/portfolio/Mechanical/WheelMolding/25Wheel.jpg" width="320" height="240">

<img src="assets/img/portfolio/Mechanical/WheelMolding/FinishedWheel.jpg" width="320" height="240">

However, before I mold the rest of my wheels, I had a few improvements I wanted to make to the molding process. The first modification I plan to do is hotglue along the bottom of the wheel hub to help solve the leaking issues. Second, I 3D printed a top clamp which also has a funnel so that I have a bigger area to pour into and any extra buildup is contained and funneled back into the mold once the level has decreased.

## 1/21/2026

Today, I molded my second wheel using the new top clamp, which also acts as a funnel, and oh boy, did it make a difference. With the new part, I was able to mold my wheel from mixing to drying in around 20 minutes, compared to the hour and a half I spent on the last wheel. This was largely possible due to the funnel allowing me to dump a large quantity of molding materials on each side and let gravity pull it down into the mold, whereas the old version required me to slowly pour to make sure no overflow occurred.

## 1/27/2026

Today after school, I was able to get the Robopack to follow me!!! I used a rough algorithem on the seeedRP2040 which takes in the output from the vision system on the Raspberry Pi, the output being "A T## D##". The A stands for automatic, T for translational meaning the angle from center, and D for distance though it is actual area, not distance being outputed(ik I need to fix this later). The seeedRP2040 takes in this data and does a simple deadzone response, where if the values are outside of a chosen range of Distance & Angle, then the robot responds by setting the motor powers to a single response value until the values are back within the deadzone range. For example: If the human is detected to be to the left of the deadzone, the robot will set motor powers to -10% left motor and +10% right motor until the human is in the center again.

<video width="320" height="240" controls>
  <source src="assets/img/portfolio/DailyJournal/" type="video/mp4">
</video>

While this works great for an initial test and proof of concept, this approach has a few critical flaws. For example: If the human gets further away from the robot, the robot will never speed up beyond its static algorithem which simply says "if human far, go to 10% speed forward" and the same thing happens with turning as well. If you try to mitigate this issue by increasing the max speed, the robot then overshoots its target and shakes back and forth.

## 1/28/2026

Today, I came in with a plan built from my robotics experience. In robotics, we use a method of controlling mechanisms called a PID Controller. The purpose of a PID Controller is to compare where something is, to where it should be, and correct an appropriate amount to reach the desired destination. This proccess is continuously looped until the the desired destination is reached. Lets break down how the PID works and what PID stands for! 

Full credit to RoboFTC for the Following PID Explanantion:

A **PID controller** is one of the most common control algorithms in robotics. It helps mechanisms reach and hold positions or velocities accurately by adjusting motor power based on feedback.

PIDF stands for:

- **P** — Proportional
- **I** — Integral
- **D** — Derivative

---

### 🔧 What is PID?

A PID controller constantly compares a **target value (setpoint)** to the **current value (measured)** and calculates how much power to apply.

#### Basic Formula:

```text
output = (P * error) + (I * accumulatedError) + (D * errorRate)
```

#### ✔️ Each term has a role:

- **Proportional (P)** — Corrects based on the current error. Bigger error = bigger correction.
- **Integral (I)** — Corrects accumulated past errors to eliminate drift or steady-state error.
- **Derivative (D)** — Predicts future error by reacting to how quickly the error is changing.

---

### 🧠 How PIDF Works (Step-by-Step)

 **1. Calculate Error**

```text
error = targetPosition - currentPosition
```

 **2. Compute Terms**

```text
P = kP * error
I = kI * totalAccumulatedError
D = kD * (error - lastError) / deltaTime
```

 **3. Calculate Output**

```text
output = P + I + D
```

 **4. Apply Output**

```text
setPower(output)
```

### How to Tune PID constants

1. Proportional (P) — Makes the mechanism respond to error. Raise it until it moves toward the target quickly but doesn’t overshoot too much.
2. Integral (I) — Use to eliminate small, constant errors that P can’t fix (e.g., due to friction). Be careful: too much I causes wind-up and instability.
3. Derivative (D) — Add if the mechanism overshoots or oscillates. D slows down the motion as it approaches the target.


### How I implemented PID in the Robopack

For the robopack program I tested today, I chose to only implement a P-Controller to start. The reason for this is due to the margin of error I am allowing which is a lot and the simplicity of a P-Controller. In the future I plan to implement I and D to get more precise motions but currenty am very content with the Robopack following ability.