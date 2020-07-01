---
layout: post
title:  Fix Xbox One controller stick drift
author: nickstugr
---

I had an Xbox One Elite Controller exhibiting stick drift on the left thumbstick - that is, when it was centred in the neutral position, the controller would act like you were pushing slightly up, causing characters in games to walk forwards constantly.

You can troubleshoot this kind of behaviour using the Xbox Accessories application on either Windows 10 or the Xbox One itself.

You can see in the bottom right of the screenshot below that the thumbstick is not centered at rest:

![stick drift](/assets/posts/xbox/stick-drift.jpg)

The elite controller retails for AU$200 and was out of warranty so fixing it myself seemed like a worthwhile project. I did a bunch of research, and while most fixes suggested that you would need to replace the whole analog joystick housing which would involve soldering, I figured out that there were 2 small sensors, each responsible for the x and y axis, that could be replaced instead and may fix my issue.

I ordered replacement parts from [AliExpress](https://www.aliexpress.com/item/32897474633.html) (but didn't wait for them to arrive). The whole housing looks like this:

![analog joystick](/assets/posts/xbox/analog-joystick.jpg)

The sensor inside which is the bit we want can actually be found in 360/PS3 controllers too and looks like this (left one is the damaged sensor from the Xbox One Elite Controller and the right one was taken out of an old Xbox 360 controller):

![sensor](/assets/posts/xbox/sensor.jpg)

You'll need T6 and T8 security torx screwdrivers to pull apart the controller. Note the [security torx variant](https://en.wikipedia.org/wiki/Torx#Variants) of the screwdriver has a hole in the middle due to the screws having a post in the center of the head. (You can also snap off this post with a small flathead screwdriver to enable a normal torx to fit, and I also discovered if you have the correct width flathead screwdriver you can then also turn the screw using this) 

I used this video for help with dissassembling and reassembling the elite controller:
<iframe width="700" height="394" src="https://www.youtube.com/embed/tLt7lPE6bXI" frameborder="0" allowfullscreen></iframe>

I used this video (starting at 4:32) for the sensor replacement:
<iframe width="700" height="394" src="https://www.youtube.com/embed/MoqW3PukDK0?start=272" frameborder="0" allowfullscreen></iframe>

Xbox 360 controller:

![xbox 360 controller](/assets/posts/xbox/xbox-360.jpg)

Xbox One controller:

![xbox one controller](/assets/posts/xbox/xbox-one.jpg)

Now my Elite controller is working perfectly again, at least for now!

_Note: Upon reassembly, the elite controller has a ribbon cable that controls the paddles on the back. If you re-connect this upside down the paddles will still work but the top left will now be the bottom right and so on._