---
title: Planning
subtitle: General Project Planning
image: assets/img/portfolio/01-full.jpg
alt: Shirts on a hanger

caption:
  title: Planning
  subtitle: General Project Planning
  thumbnail: assets/img/portfolio/01-thumbnail.jpg
---

# Robopack - A backpack-carrying robot

*Note that this planning was made at the start of the year and might not be 100% accurate to the current path lol, the project goals on the home page of the site itself is the most accurate and up to date

Goal: The goal of my project is to create a robot that can carry backpacks for those who cannot carry them themselves due to a medical condition or are parked very far away, and it would help them with their shoulders. I can also use this in college Yay

Concept: I want to create a robot that can autonomously drive across campus, helping students carry their bags. Ideally, I want the robot to navigate the campus using its navigation and have a touchscreen where students can select a location for the robot to travel to. I plan to also have a stationary charging station that works as both a recharge station, a zero point for autonomous travel, and a main computer that holds live update info on different robots. While I might sacrifice some aesthetics, I want the robot to be as cost-effective and robust as possible so it can continue for many years, even in harsh weather conditions, to help carry backpacks.

Main Engineering Goals

- A robot that can drive around with a storage area for a backpack
- A navigation system that allows autonomous driving
- Self-correction for if students mess around a bit?
- Touchscreen for easy location selection. Maybe raises or folds out
- Weather protection?
- Stationary Charging station to charge and zero the robots
- Robot live update feed?
- Start with a 1-robot charging station, but make it expandable for up to 10?
- Cost-effective yet robust robot design
- It would be nice to still have an impressive look, though
- Longevity of the system, meaning weatherproofed robot & charger


## Brainstorming

### Frame
When I first got the idea for this project, I was super excited because I felt that this project took my previous skills from my robotics background and applied them into a helpful solution for my community. My first thought was to build the frame out of the same aluminium extrusion 2x1 from West Coast Products I used on my robotics team. However, I quickly realized that the price would be too much if I used metal as my frame. My next thought was to CNC ½ in. thick plywood as I could accurately assign holes while keeping a lower cost. While this idea was appealing at first, the cost of ½ in plywood was also pretty expensive when I looked at Lowe's, and I decided that maybe I needed to go a bit simpler to get my best cost-benefit ratio out of my build. At the core of the robot, the robot simply needs 2 motors attached to the back and then steering in the front, and finally, something for those components to attach to. I chose to use 2 CIM motors because they are extremely cheap, and I have some experience with them from robotics. Back to the frame question, I fell upon CNCing ½” Plywood for the base and using Aluminium for high strength areas such as gearboxes. I plan to order plastic casing to make it look super nice and professional in the end

### Motor

As I mentioned before in the Frame section, I plan to use two CIM motors from Vex because they are only $17 per motor and they are easy to use. Additionally, because these motors have died out in FRC, my robotics competition, my team has a bunch of CIM motors they gave me for free, making it an extremely cost-effective motor for me.

To power and control the motor, my first thought was a simple mosfet board as the motors can literally be attached straight to a battery *DON'T DO AT HOME! DANGEROUS*. However, I was fortunate enough to come across some motor shields at my school that can handle the specs of the motor. The IBT_2 board has a max Amp rating of 47, which should be plenty for my application, and can take in a 12V power source.

NM, USED THIS [MOTOR CONTROLLER](https://www.aliexpress.us/item/2251832642642974.html?src=bing&aff_short_key=UneMJZVf&aff_platform=true&isdl=y&albch=shopping&acnt=135095331&isdl=y&albcp=555356487&albag=1302922493711159&slnk=&trgt=pla-4585032216153170&plac=&crea=81432715238453&netw=o&device=c&mtctp=e&utm_source=Bing&utm_medium=shopping&utm_campaign=PA_Bing_US_PLA_PC_toys_TROAS_20250320_AESupply&utm_content=toys&utm_term=brushed%20speed%20controller%2012V%2040Amps&msclkid=30d702b19a99109eb82f6a1639702b6f&gatewayAdapt=glo2usa)

Another thought I had was that I would need to gear down the CIM because it outputs a high rpm, low torque rotation. According to the stats of the CIM Motor from Vex, the output is 5,300 RPM at 21.33 in-lb stall torque so by gearing the motor down, I can get a more reasonable speed and make sure I have enough torque for any circumstance. To determine the ratio, I played around with dividing that 5,300 by a gear ratio and then converted to feet/sec in order to visualize the speed. My final result I decided upon was a 1:15 ratio made up of a 1:5 and a 1:3 in order to give a theoretical 12.404 ft/sec at full power with an 8” Diameter wheel and also a stall torque of 319.95 lbs/in.

Lipo with a low C-Rating means longer battery life
I’m not sure how I will steer, but I will probably use some sort of Caster wheels for the front and be able to use tank steering or something.
Power and Safety
When thinking of power, my first thought is the MK ES17-12 12V SLA Battery, which I use in my robotics. These batteries are essentially car batteries and are less dangerous than Lipo batteries, which is good when I am planning to let these robots autonomously drive around without me. I might still experiment with other batteries because of the high cost these batteries come at.

However, with such a powerful battery, I want to make sure safety is my utmost concern. My plan to ensure the safe operations of the robot is to install 40-amp breakers on both the connection between the battery and the motor board, and the connection between the motor board and the motor. This should ensure I don’t fry any electronics and keep myself from being fried. Additionally, I want to add this surface mount 120 Amp breaker between the battery and the board to allow easy turn-off, but force intentional turn-on. The downside is that the 120-amp limit is too high for safety, so that is why I found a breaker on Amazon with a 40-amp limit. Lastly, in robotics, we have a big orange warning light that turns on when the robot is on, called an RSL. I plan to implement this as well to alert both me and users when the robot is powered on.

One important way I will ensure safety is through a test run of the code. I plan to use the official motor drivers, but instead of using the super powerful battery, use a 9V battery and a smaller DC motor, which will allow a lower-stakes testing environment for shorts, mistakes, and such. Once I have created and set up the wiring with the more powerful battery and motor, I plan to visit a mentor who is a professional technician and get his approval.
Software
While I’m not a super softwar techy guy, I do know Java pretty well and have some decent experience programming in C++. Additionally, with the advancements of AI in the coding realm, I think I can produce the code I desire with the help of AI and my understanding of the code it produces.

My plan for the software side is to use an Arduino as my starting place with a breadboard and then convert to a custom-made board, hopefully using an XIAO RP2040 Seeed if it has the capabilities I need, as I am experienced with using this board. I plan to use a touchscreen that shows a map of campus and allows users to choose and confirm a point that the robot should head to. At any time, the user should be able to cancel their trip. 

For the autonomous part of the software, I have a few possible Ideas, each with its benefits and drawbacks. One possible idea is to have the robot follow lines on the ground. While this solution would be pretty surefire, this would require permanently painted ground, which isn’t great, and the weather could wash away the paint. Another idea, while more difficult, is to use an encoder on the robot and then do the math to simulate the position of the robot. I currently plan to try using an encoder and see whether the accuracy is enough. Another method I could use would be attaching April tags, aka, QR code-like panels that the robot can identify through vision and then recognize its location. The upside of this method is instant and accurate localization when a tag is seen, but it is complicated and requires April tags throughout campus. Lastly, I am curious as to whether I can use satellite GPS to navigate the bot, but I don't know enough about that technology yet.

April Tag localization sounds best but vslam is also an option

When it comes to charging station software, I want to be able to access it through wifi or something, where each robot is going, and ensure no collisions occur between robots like a control tower operator at an airport.

Other sensors such as Radar or such for object in the way
Maybe a board remote that the robot follows


