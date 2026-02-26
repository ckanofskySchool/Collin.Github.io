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

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
  <source src="assets/img/portfolio/DailyJournal/ObjectDetectVideo.mp4" type="video/mp4">
</video>

## 10/9/2025

Today, I continued working on the electrical testing setup I started on 10/7/2025. I acquired 4 more Wago connectors from my robotics team and finished up the connections between the motor and the motor controller using them. After finishing the main power setup, I started building the control wiring setup. I used a 5V power supply to power a remote control plane reciever which outputs a pwm signal, used to control the motor. Then I attached the pwm signal and ground to the reciever on the throttle port of the reciever and was able to control the motor using a RC Plane Remote.

### Video of motor moving

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
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


<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/DailyJournal/SeeedBackPinsPlaced.jpg">

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/DailyJournal/SeeedFrontPinsSoldered.jpg">

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/DailyJournal/SeeedFrontSoldered.jpg">

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/DailyJournal/SeeedBackSoldered.jpg">

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

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/DailyJournal/MechanicalPhotos/Order.png">

The reason for which I have dropped 200$ on the harware for my project is because yesterday at robotics, I cut the 80-20 Material for the base of the Robopack, which was the 2 27" pieces and 2 17" pieces. I cut these using a Aluminium Chop Saw, and then used a grinder, debur tool, file, and some helpfull teamates to clean up the edges and make them smooth. Shout out to my robotics mentor Ray Kimble for overseeing the machining proccess and making sure I was cutting safely and correctly!

<!-- Pictures from phone of machinery here. Also video of me cutting tonight-->

At school today, I cleaned up the dust and residue from the 80-20 which sat in storage for probably like 10 years with my friend aaron, and then sent the order to 80-20 for the hardware shown at the start.

Due to waiting on the 8020 order & the crimping materials to come in, I will be focusing on the power connection soldering, and the programming in the next coming days.

## 11/11/2025

Quick update from yesterday, I finally cut out my topography file as shown below, I was a bit concerned about the CNC breaking due to the depth of which my file went and the vacuum shoe being in the way, but Dr. Taylor helped me lock the vacuum shoe higher up which made the cut a success. 

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/DailyJournal/2x4topo.png">


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

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/Mechanical/WheelMolding/MoldingSetup.jpg">

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/Mechanical/WheelMolding/AfterPourChaos.jpg">


## 1/20/2026

I spent today removing the molded wheel from the mold itself, which was a bit of a struggle, but not terrible. Once I got the wheel out, I used a wire snip and knife to clean up the edges and the wheel turned out great. Based on the result, I decided to use this molded wheel technique for all 4 wheels.

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/Mechanical/WheelMolding/MoldCured.jpg">

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/Mechanical/WheelMolding/25Wheel.jpg">

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/Mechanical/WheelMolding/FinishedWheel.jpg">

However, before I mold the rest of my wheels, I had a few improvements I wanted to make to the molding process. The first modification I plan to do is hotglue along the bottom of the wheel hub to help solve the leaking issues. Second, I 3D printed a top clamp which also has a funnel so that I have a bigger area to pour into and any extra buildup is contained and funneled back into the mold once the level has decreased.

## 1/21/2026

Today, I molded my second wheel using the new top clamp, which also acts as a funnel, and oh boy, did it make a difference. With the new part, I was able to mold my wheel from mixing to drying in around 20 minutes, compared to the hour and a half I spent on the last wheel. This was largely possible due to the funnel allowing me to dump a large quantity of molding materials on each side and let gravity pull it down into the mold, whereas the old version required me to slowly pour to make sure no overflow occurred.

## 1/27/2026

Today after school, I was able to get the Robopack to follow me!!! I used a rough algorithem on the seeedRP2040 which takes in the output from the vision system on the Raspberry Pi, the output being "A T## D##". The A stands for automatic, T for translational meaning the angle from center, and D for distance though it is actual area, not distance being outputed(ik I need to fix this later). The seeedRP2040 takes in this data and does a simple deadzone response, where if the values are outside of a chosen range of Distance & Angle, then the robot responds by setting the motor powers to a single response value until the values are back within the deadzone range. For example: If the human is detected to be to the left of the deadzone, the robot will set motor powers to -10% left motor and +10% right motor until the human is in the center again.

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
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

For the robopack program I tested today, I chose to only implement a P-Controller to start. The reason for this is due to the margin of error I am allowing which is a lot and the simplicity of a P-Controller. In the future I plan to implement I and D to get more precise motions but currenty am very content with the Robopack following ability. Below is a video of my tuned P-Controller on the robot.

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
  <source src="assets/img/portfolio/DailyJournal/" type="video/mp4">
</video>


## 1/29/2026

Yesterday I spent around 4 hours on a new human tracker that could track one person and ensure that even if a bigger(larger area) target or a different target went in front of the desired target, the program will still track the original person. While I was able to get the raspberry Pi tracking code to work, I was unable to get the Seeed RP2040 code to make the robot follow me. I was super flustered bc I had spent so much time and nothing was working, so I set it to the side and would pick back up tommorow.

## 1/30/2026

Welp, I found the issue today. The Raspberry Pi Tracking code had an absolute value symbol on the heading angle so there were never negative angles outputed, resulting in the robot infinitly spinning. 

After I fixed this, I also did a revamp of the RP2040 Code by turning the code into a full PID loop rather than just a P loop, and I also went through and made sure all motor movements and directions were as intended. This took a bit but was definatly worth it in the end.

As I was testing, I ran into a new issue when my teacher Mr. Budzichowski walked in front of the robot and the robot decided that it didn't like him, so it charged full speed at him...

Ya, bit of a safety issue... luckly he was fast on his feet and was able to skilfully dodge the robot, but I do need a solution for this. A few solutions come to mind. The first is a soft bumper on the outside so that instead of a semi-sharp alluminium frame hitting something or someone, a softer pool noodle impacts instead. Secondly, I want to implement a safety mechanism which shuts off the motor power if a super sudden change in desired power is called for. Lastly, maybe a limit switch on the front of the robot so if an object is hit, then it instantly stops no matter what.

Also I ordered my front wheels but they got delayed to next sunday due to weather...

## 2/4/2026

We have been snowed in for the past 2 days of school, so I have been focused on cleaning up documentation and getting my documentation fully updated. I started by re-organizing the portfolio pages to highlight key parts of the Robopack's journey, then I used ChatGPT in VSCode for the first time, which allowed the AI to see and change all files in my repo, which was super usefull. I used this tool to change the size of all my images to 1080 width with auto chosen height, which really cleaned up my website. I also had the AI grab documentation from my daily journal and sort the info into each portfolio page which will make my documenting easier because everything I want to referance is consolodated for me.

I also implemented a safety stop into my RP2040 Seeed code where if the change in any one motor is more than a certain threshold, which I set to 30, then the motors are set instantly to stop until the values are within a safe range again. The code is shown below:

```cpp
if (abs(LP - lastLP) > maxAllowedPowerChange) {
    RP = 90;
    LP = 90;
}
if (abs(RP - lastRP) > maxAllowedPowerChange) {
    RP = 90;
    LP = 90;
}
```
## 2/6/2026

Today, I focused on getting all the programming testing updated and documented. I had a bunch of videos on my phone from testing and I used today to transfer them to my repo, and shrink the file sizes to be more manageable. As a quick update on progress, I have been told by Mr. Dubick that we have 12 days of class left till the projects are due, but I'm feeling pretty good because all I have left currently is to attach the front castor wheels which come this weekend, test some safety features, make the rasoberry Pi auto boot to my program, buy cut finish and attach the fancy outer wood(poplar wood) to the robopack. Lastly, I want to make a nice reveal video for the Robopack and finish up the portfolio nicely.

For what I got done today, like I said prior I did some documenting, but I also focused on the Raspberry Pi program running on startup, which would allow the robot to run and follow a person simply by turning on the power. 

In order to do this, I tried two methods from this [tutorial](https://www.dexterindustries.com/howto/run-a-program-on-your-raspberry-pi-at-startup/) that I found.

The first methods I tried was to run the program using .bashrc. This seemed like a good option due to it

## 2/10/2026
Today I woke up early to go to lowes, bought the nice fancy outer wood, and cut it to the sizes I need. I chose to make a blunt joint at the corner of the wood so that meant for my 24x20 robopack frame, I cut 2x 24" pannels, and two 21.5" pannels which account for the extra wood width on the corners. I plan to use a router to make a .75" bevel on the ends giving a smoother asthetic to the robot. I also cut 4 extra small scrap pieces so that I can experiment with different types of finishes and choose my favorite. I plan to do that tommorow along with drilling the mounting holes into the wood, which I designed a 3D printed template in CAD to help me accuratly drill.

## 2/11/2026
Today I went and cut out indents in my wood to allow the wood to go around the 80-20 brackets. I used the bandsaw and a square to get my cuts accurate. 

I also got my front wheels today and attached them to the robopack. I got lucky with the hole placement and was able to simply print some washers and attach 3 of the four holes into the bottom of the robopack securing the wheels. 

Lastly, I went and bought some wood stains yesterday to try them out and see if they would make the wood look nicer. I decided to try 3 different stains on my tester pieces and choose my favorite. To explain how staining works, you first have to sand with 220 grit sandpaper, then you apply a coat of pre-stain, and after waiting 15-20 mins, you can paint on a stain into the wood. After staining, you should put on an polyuerthane or polyacrylic finish to protect the wood. 

## 2/12/2026
Today, I didn't have engineering class but during the second half of my free period, I went over and sanded down the cutouts on my wood frame to ensure everything fit where it was supposed to. Next I plan to drill some mounting holes using my 3D printed templates, as well as some wiring holes so that I can eventually move all my electronics to underneath the robopack wood

## 2/17/2026
Today, school was off but the lab was open so I came in to work on the most time consuming part of my project, staining the wood. Here are the steps and workflow on how I bought, cut, preppared, stained, and finished my wood:

### Determining the wood I wanted:

When I was searching for a nice outer shell wood for my robopack, I had to main factors in mind. I wanted a wood that would look nice and presentable, but I also didn't want to spend an insane amount of money on the wood. After looking through some different woods on Lowes website and talking with Dr. Taylor, one of the engineering teachers who is well versed in woodworking, I decided upon using Poplar Wood. Poplar wood is a lighter hardwood which is easy to work with and has a nice grain pattern. The one downside was that it has a greenish tint but the stain should get rid of that green completely.

At Lowes, I bought 5 boards of 12"x24" .75" thick Poplar and 1 board of 8"x24" .75" thick Poplar. This worked out nicely because my robopacks dimensions are 20"x24"x12"(w,l,h) so I would have less cutting to do. The total price of the wood order was ~$130.

I also proceeded to buy some stains in order to make the wood look darker and nicer. I bought Minwax Pre-Stain and 3 different Minwax stains to try some different colors and choose my favorite. I bought the smallest volumes they had but in the end it turned out to be the perfect amount.

### Cutting the Poplar Wood to size, drilling holes, & adding gaps

Once I had bought all the wood, I planned out where each piece would go and the size of which I would need to cut it too. Before buying I had done some planning to ensure I bought enough wood, but I double checked and layed out the wood before I started cutting it all. Heres the plan:

Left & Right Sides: 24"x12" (3"x3" gap in bottom corners for the 8020 mounting plate) 
Front & Back Sides: 21.5"x12" ( 1/4" bevel on one side only on 12" edge to give less boxy asthetic)
Bottom Sides: 24"x12" + 24"x8" to fill the 20" space (3"x1.5" gap in outer side corners for 8020 tubing)

I also had to cut an 1/8 off several of the so called 24" pieces because they were actually 24.125"

To make all the general shape cuts such as cutting the 24"x12" to 21.5" for the front and back, or correcting the 24.125" to be 24", I used the table saw as seen below in the short video:

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
  <source src="assets/img/portfolio/DailyJournal/" type="video/mp4">
</video>

To cut the gaps in the wood, I chose to use the bandsaw instead because the table saw would leave a circular grove in the wood, whereas the bandsaw cuts veritically so I can cut perpindicular angles. The downside was that the clamp on the bandsaw wasn't super straight, but I tried my best and got the cuts relatively straight. Below is all the wood after cutting:

Then, I used some 3D printed drilling templates I made previously to help me drill consistent holes for mounting the boards. I didn't have a metal busing in the templates so I had to be carefull to drill through the template, not drill the template itself.

### Prepping the wood - Sanding

Once all the boards were cut and I ensured that they would fit their intended area, I began my long and difficult sanding journey to get the boards ready for staining. According to the instructions on the Minwax stain, I used  220 grit sandpaper on a orbital sander on every face of each piece, yes, even the edge faces and gap faces. It took like 2 hours in total I think. 

After using the orbital sander, I started sanding the edges & corners of each piece by hand, ensuring everything was nice and smooth, also a massive pain. I think I spent an hour or so and my hand was hurting a bit by the end, but I'm probably fine.

Finally, I used the router table and a 1/4" Bevel bit to make the roundings on the front and back panels. While they turned out a little bit rough, I used the orbital sander with the 220 grit sandpaper and cleaned up the round making it look nice.

### Prepping the wood - Pre-Stain

Once all my pieces were sanded, I brought them outside to do the final steps before staining the wood. I started by using a brush to get any big chunks of dust or woodchips off the boards. Then, I used a damp paper towel and wiped down each board to try and get all the sawdust off the boards leaving the board ready for the Pre-Stain coat.

Once the wood had dried from the damp towel, I applied the Minwax Pre-Stain I bought on the wood using a foam brush, and set a 10 minute timer. Once the timer finishes, it will be time to stain.

### Staining the wood

After the Pre-Stain coat dried for 10 minutes, I applied my Minwax 232 Red Chestnut Stain which I had chosen as the color to stain my wood. I once again used a foam brush(different one than the pre-stain!) to apply a coat of stain on the wood. I tried to not make it too thick but I'm not sure if I did it that well, but I tried lol. After staining, I let the piece dry for a day.

I then came back the next day and stained the back of the board. However I was unable to do a second coat because I ran out of stain and was just barely able to finish my first coat at the expense of getting stained a bit myself.

### Finishing the wood

Due to time constraints, I did not apply a finish to the wood.

### Final Results

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/Mechanical/">

## 2/18-26/2026 

### Attaching the wood frame

Once the frame was all done, I went to attach my wood boards to the frame, but I ran into a bit of an issue. My bolts were too short to reach through the wood and into the nut below. To fix this issue, I had to buy a 25 pack of 1.125" bolts from McMasterCar because apparently local hardware stores don't sell this size(I tried...).

Once the screws arrived, I attached the boards to the metal chassis, aligned the top of the boards on the side, and below is how it turned out.

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/Mechanical/">

### Re-Attaching Wiring to wooden frame

I decided to swap my wiring from the acrylic piece I cut that was a bit too small to fit everything onto the wood. To do so, I used a lot of velcro so everything could be adjustable and nothing truly permanent or needing holes in the wood. I attached most of the electronics on the underside of the robot, but I placed the main breaker and battery on the top so they would be easy to access to turn off the robot and to swap battery's. I also ended up putting my raspberry pi and camera at the top becasue they are packaged together and by attaching with velcro on the inner front wall, the camera has a good view of the user. Below is my final wiring:

#### Bottom Wiring:

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/Mechanical/">

#### Front Side Wiring:

<img style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" src="assets/img/portfolio/Mechanical/">



### Tuning the motion of the robopack

Once the robopack had all the proper electronics attached and the final frame on, I went back into my code to refine the tuning. Originally I tried to tune multiple variables at once, such as tuning Kp and Kd at the same time but I was not getting great results from this and decided to take a step back to think about my approach.

After taking some time to think, I decided to start simple and only use the P of the PID controller to have just direct error based correction. I started with a distance Kp of 0.04 which I had used in previous PID testing trials and a angular Kp of 0.75 which I also once used in previous tests. The first test run was immensely better than any of the other trials I had run before and I knew I was on the right track.

#### Trial #1 - Distance(Kp=0.04, Ki=0.00, Kd=0.00) & Angular Kp(Kp=0.75, Ki=0.00, Kd=0.00)
Distance Results: 
- Smooth forward motion and reactions, bit jerky when it comes to stopping but pretty good

Angle Results: 
- Decent turning but overshooting and not correcting small/mid error, not as reactive to big turns

#### Trial #2 - Distance(Kp=0.04, Ki=0.00, Kd=0.00) & Angular Kp(Kp=1, Ki=0.00, Kd=0.00)
Distance Results: 
- No changes applied from trial #1

Angle Results: 
- Now corrects small/mid errors, but is overshooting back and forth.

#### Trial #3 - Distance(Kp=0.06, Ki=0.00, Kd=0.02) & Angular Kp(Kp=1, Ki=0.00, Kd=0.1)
Distance Results: 
- NEED TESTING

Angle Results: 
- Looking amazing, accurate yet smooth with the Kd dampening helping when on target to not oscilate.