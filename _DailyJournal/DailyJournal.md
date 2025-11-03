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
  <source src="/assets/img/portfolio/DailyJournal/ObjectDetectVideo.mp4" type="video/mp4">
</video>

## 10/9/2025

Today, I continued working on the electrical testing setup I started on 10/7/2025. I acquired 4 more Wago connectors from my robotics team and finished up the connections between the motor and the motor controller using them. After finishing the main power setup, I started building the control wiring setup. I used a 5V power supply to power a remote control plane reciever which outputs a pwm signal, used to control the motor. Then I attached the pwm signal and ground to the reciever on the throttle port of the reciever and was able to control the motor using a RC Plane Remote.

### Video of motor moving

<video width="320" height="240" controls>
  <source src="/assets/img/portfolio/DailyJournal/Motor moving.mp4" type="video/mp4">
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



### Experienced User Workflow - By: [Angelina Yang](https://fabacademy.org/2024/labs/charlotte/students/angelina-yang/about/)

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



