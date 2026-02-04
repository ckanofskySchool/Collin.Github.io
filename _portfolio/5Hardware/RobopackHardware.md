---
title: Robopack Hardware
subtitle: Electronics for the Robopack
image: assets/img/portfolio/DailyJournal/ElectronicsSetup.jpg
alt: 

caption:
  title: Robopack Hardware
  subtitle: Electronics for the Robopack
  thumbnail: assets/img/portfolio/DailyJournal/ElectronicsSetup.jpg
---

## Hardware Planning

### Motors & Motor Controller
I am planning to use CIM motors which I've learned from my robotics teams, the reason for using a CIM motor is due to low costs sitting at around 17$, and the output it produces being 5,330 RPM, with 21.33 in/lbs of torque which is pretty powerfull and should definatly ensure a job well done.

## Electrical Layout (From Daily Journal)

### Main Power Path
Battery (12V 15000mAh) -> Breaker (30A) -> Motor Controller (Koors40 Brushed DC) -> CIM Motor

### Control Path
RC Controller -> RC Receiver -> Motor Controller PWM

This early test setup helped me validate the motor controller and wiring before the robot frame was complete.

## Wiring and Modularity

I used Wago lever connectors so I could swap components without re-soldering. For the battery connections, I soldered EC5 connectors onto 12 AWG wire. Later, I ordered Anderson connectors, extra 12 AWG wire, and a power distribution bus for cleaner, safer routing.

<!-- TODO: add photo of wiring layout on the bench -->
<img src="https://placehold.co/1200x675/png?text=Bench+Wiring+Layout" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
## Electronics Panel (1/7/2026 - 1/12/2026)

I laser cut an acrylic electronics panel and added velcro strips for mounting components. I also designed a 90-degree 3D printed mount for the breaker so it could stand upright. Most components are now mounted, with the Seeed RP2040 kept off to the side for easy programming.

<!-- TODO: add photo of the laser-cut electronics panel with components attached -->
<img src="https://placehold.co/1200x675/png?text=Electronics+Panel" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">
## Integration Status

- Motor power test successful using RC control
- Wiring is modular and serviceable
- Electronics panel assembled and ready for final mounting








