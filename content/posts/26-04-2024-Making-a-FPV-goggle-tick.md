---
title: "Making a FPV goggle tick."
date: 2024-04-26T20:02:01+03:00
slug: 2024-04-26-Making-a-FPV-goggle-tick
type: posts
draft: false
categories:
  - hardware
  - FPV
tags:
  - hardware
  - FPV
---

# The inspiration
Ever since I started flying FPV drones back in 2018 the one thing that bothered me, was how our FPV goggles were the only thing that the FPV community treated as a black box. We buy a pair of them, we do our thing and thats it! Maybe a DVR mod or power switch mod here and there and not much more! 

In 2021 during the lockdown I didn't have anything better to do, so I started investigating what makes these goggles tick and how could I design a pair. After looking at many teardowns of both FPV and VR headsets I found that the entire headset design revolves around the displays. For FPV headsets there exist two categories:

+ slim goggles containing two microdisplays and pancake optics. While these are the most light-weight option, they have a high cost, with the bulk of it being the microdisplays. A well designed headset of this category is the Orqa v1/v2 by Orqa FPV.

+ box goggles. Usually they come with a single 4-5 inch display and a fresnel lens. They are big and cumbersome, but they are also cheap and can accommodate pilots with bad eyesight by having the space for glasses. A well designed headset of this category, is the eachine/skyzone cobra goggles.

Actually I kinda lied, some headsets belong in neither of those categories. Headsets like the DJI FPV goggles v1/v2, Topsky prime 1s/2s, SJ RG01 contain two small displays and simpler fresnel lenses. They combine a relatively smaller frame with the big FOV that box goggles offer. 

+ The DJI goggles are typically well received. They use two 2.5' displays and they crop them in order to simulate a 1' display, in order to bring the FOV to a managable level. Their main disadvantage is that they work well only with the first gen DJI FPV system. They lack HDMI inputs and 
+ The same cannot be said about the topsky and SJ headsets. They both lack the same thing, high resolution displays, and this is the reason why they both didn't succeed commercially.

What if I could combine these 3 headsets?

What if I could make a headset with high resolution cropped displays like the DJI v1, while having the form factor of the Topsky prime, AND being open source for anyone to tinker with?

# The research

After some digging in Aliexpress I found some cheap VR display kits. These contain 2 displays, a gerneric driver board and a specialized board to bridge the displays to the driver board. These kits are not what I want. My requirements for the electronics are:
+ analog video input
+ hdmi video input
+ the ability to scale down the display, to simulate a smaller one
+ OSD and menu support

After more searching, no kits come close to fulfiling any of those requirements. This means that I will have to design my own video processing system and display driver. 

# The displays

As I mentioned previously, the main component of any FPV ot VR headset are the displays, so I will first need to select a panel and then design the rest of the headset around them. 

A major limiting factor when it comes to choosing suitable displays is my current living status as a uni student and my geographical location. I do not live in China nor do I have the appropriate reputation inside the VR industry to communicate directly with display manufacturers or sellers. The sistuation becomes even worse when many of the manufacturers purposely exclude critical data from their datasheets(display initalization code). 

After a LOT of web scouring for datasheets and inialization code, I stumbled across Project Northstar, an open source AR project that uses the BOE VS035ZSM 3.5' panel, with a resolution of 1440x1600. While it is kinda big for my needs, it is relatively easily accessible on Aliexpress and for an ok price. For now I can create a proof of concept and then jump to a better display if needed.

One thing that most of the VR panels that I came across have is their communication interface. It is called MIPI(Mobile Industry Processor Interface) DSI(Display Serial Interface), it is proprietary and requires an annual fee to view it's specification. A quick google search can solve this issue. This common interface makes it also easy form me to switch to a different panel on the future.

# The video processing

I havent been able to find an off the shelf ASIC that is readily available to me and can fulfil my requirements, so a FPGA will probably be needed to scale the video and add a OSD menu. As for the video inputs(analog and HDMI), off the shelf ICs will be used. 

# The drivers

MIPI DSI is not a very well known protocol, thus few FPGAs support it, and those that do and offer adequite speeds come with a high cost. My solution for this, is the ssd2828 ASIC that converts parallel rgb video to MIPI DSI. This ASIC is readily available, and there is lots of info about it on the web.



