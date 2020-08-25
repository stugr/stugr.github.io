---
layout: post
title:  Automating venetian blinds with Arduino
author: nickstugr
---

### Summary

To obtain optimal light blockage from my apartments poorly designed micro venetian blinds, I have flipped the blinds around so the tilt rod and raise/lower strings are now on the window side and hard to use.

This results in the bedroom blinds being left closed most of the time, and as my kitchen gets sunlight through the bedroom door, it leads to a dreary disposition.

This sounded like a perfect opportunity to improve my working from home experience through the use of technology by adding a button driven mechanical process to open and close my blinds, and I'd get to play with Arduino and do some more programming.

[Code](#code)  
[Wiring diagram](#wiring-diagram)  
[Finished product](#finished-product)  
[Parts required](#parts-required)  
[Decisions I had to make](#decisions-i-had-to-make)  
[Features](#features)  
[Todo](#todo)

{% include youtube.html id='7HNcTJCMwQM' %}

### Code

Code is on [github here](https://github.com/Stugr/blinds-automation)

[![code](/assets/posts/blinds/code.png)](https://github.com/Stugr/blinds-automation "click to view code on github")

### Wiring diagram

![wiring diagram](/assets/posts/blinds/schematic.png "wiring diagram")

### Finished product

Breadboard with open and close buttons attached to the wall. Lego tower behind the blinds, with the stepper motor stuck to the top. Shaft coupler connecting to the tilt rot.

![layout](/assets/posts/blinds/layout.jpg "layout")

Random angry birds lego tower I had in the cupboard that was basically the correct height. One day I may replace it

![layout2](/assets/posts/blinds/layout2.jpg)

### Parts required

If you don't have any equipment yet, a [$40 kit at jaycar](https://www.jaycar.com.au/duinotech-arduino-starter-kit/p/XC3902) containing an UNO Arduino-Compatible board, breadboard, usb cable, jumper wires etc might be a good start and may be cheaper than buying some of the parts individually

- Arduino Uno - [$33 at rs-online](https://au.rs-online.com/web/p/processor-microcontroller-development-kits/7697409/)
- USB A to B cable - [$5 at officeworks](https://www.officeworks.com.au/shop/officeworks/p/keji-2m-usb-type-a-to-type-b-cable-cou2pc02) or [$6 at rs-online](https://au.rs-online.com/web/p/usb-cables/8158450) or [$10 at jaycar](https://www.jaycar.com.au/usb-2-0-a-to-b-cable-1-8m/p/WC7700) - you may have one lying around from an old printer
- Stepper motor and driver board - [$10 at rs-online](https://au.rs-online.com/web/p/motor-control-development-kits/1845109)
- Shaft coupler - [$12 at jaycar](https://www.jaycar.com.au/solid-shaft-couplers-female-type-i/p/YG2600)
- Basic electronics components:
    - Breadboard - [$8 at jaycar](https://www.jaycar.com.au/arduino-compatible-breadboard-with-400-tie-points/p/PB8820)
    - Jumper wires:
        - Plug to plug - [$6 at jaycar](https://www.jaycar.com.au/150mm-plug-to-plug-jumper-leads-40-piece/p/WC6024)
        - Plug to socket - [$6 at jaycar](https://www.jaycar.com.au/150mm-plug-to-socket-jumper-leads-40-pieces/p/WC6028) - connects the driver board to the breadboard/arduino
    - 2x Buttons - [95c each at jaycar](https://www.jaycar.com.au/1-4mm-spst-micro-tactile-switch/p/SP0601)
- Something to house the motor at the right height. I used a bunch of Lego I had lying around and double sided mounting tape which I already had in my toolkit [$11 at bunnings](https://www.bunnings.com.au/scotch-2-5cm-x-1-5m-extreme-double-sided-mounting-tape_p3961938)

### Decisions I had to make

#### Tilt or raise/lower

Turning the tilt mechanism using either a servo or a stepper motor seemed like the best approach as then I wouldn't have to figure out how to interface with the raise and lower string which is the kind you have to pull on an angle.

![controls](/assets/posts/blinds/controls.jpg "controls")

#### Where to attach to the tilt mechanism

I took down the blinds and experimented with housing a motor in the top rail connected to the horizontal metal rod that is responsible for the tilt, but there's not much room up there (at either end), and I decided that a modification that could be removed easily to return the blinds to manual control was preferred. This meant that I was going to have to attach a motor to the tilt rod that a human normally turns.

![housing](/assets/posts/blinds/housing.jpg "housing")

#### Servo or stepper motor

A stepper motor allowed me to program in a set number of rotations between open and closed.

#### Manual controls or automated

Keep it simple to start with, and use buttons to open and close on-demand instead of trying to get fancy with automatic function based on time of day, light levels, or voice control

### Features

- 2x buttons - open and close
- Open state is blinds horizontal, close state is blinds vertical
- Stepper does 6 full rotations (so tilt rod turns 6x) between open and closed state
- Pretty quiet! (it turns pretty slowly, ~30 seconds to change states)
- Stores open or closed state in EEPROM to resume operation after power outages  
    _NB. To limit the number of writes to the finite EEPROM I am only storing the 2 major states of open or closed. This mean that in the rare case that power is lost while the blind is somewhere between open and closed state then a manual turning of the rod to reset state would be required_
- While blinds are turning, press either button to cancel current operation. Press the appropriate button to either continue or return to previous state.
- Using 5V from Arduino to power stepper directly as well - will change this at some point
- Using the Arduino builtin pullup resistors for the buttons to simplify the wiring

### Todo
- Play more with a light sensor - living in the city means there is a lot of light bleed from nearby buildings and so I didn't collect enough samples to decide on a light threshold for automatic opening and closing
- Add a [Real-time clock (RTC) module](https://www.jaycar.com.au/arduino-compatible-real-time-clock-module/p/XC4450) for automatic opening and closing based on time of day
- Wifi for voice control via Alexa/Google home - I maybe should have bought an [Uno with wifi](https://www.jaycar.com.au/uno-with-wi-fi/p/XC4411) instead of needing to add an [ESP8266 board](https://www.jaycar.com.au/wifi-mini-esp8266-main-board/p/XC3802)
- Play with hardware instead of software button de-bouncing
- Find something other than matchsticks to pad out the shaft coupler


