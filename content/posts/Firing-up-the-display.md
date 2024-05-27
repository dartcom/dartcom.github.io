---
title: "Firing Up the Display"
date: 2024-05-27T13:30:15+03:00
slug: 2024-05-27-firing-up-the-display
type: posts
draft: true
categories:
  - hardware
  - FPV
tags:
  - hardware
  - FPV
---

Continuing on the [first post](/posts/making-an-fpv-goggle-tick/), we are going to tackle the first part of this project, the displays. 

# The display

As mentioned before, I settled for the VS035ZSM panel by BOE. While there are panels out there that fit better to what I want to achieve, this will do for now. Its specs are:

- resolution:   1440 * 1600
- size:         3.5'
- input:        Dual MIPI DSI
- framerate:    90 fps
- color depth:  8-bit
- driver ic:    NT57860

# Getting the datasheets

Unfortunately, both, display datasheet and MIPI protocol datasheets require an NDA. The MIPI datasheets can be found easily with a few Google searches. I also tried contacting BOE, but got no response. The good news are that at the time of writing this article, the display datasheet can be found by searching `VS035ZSM datasheet` on the images tab. Still, BOE has omitted the initialization code in its datasheet to make our lives even harder. Fortunately, [Project Northstar](https://www.projectnorthstar.org/) uses the same display, and since it is open source, the initialization code can be found. It would be also great to have the datasheet of the display driver ic (NT57860), but it could not be found anywhere, and Novatek ignored my emails. 

# The bridge

The goal is to have an FPGA receive video, process it and send it to the display. I could not find a device that can handle MIPI at the speeds I want, with a low price and great availability. This is where an RGB to MIPI bridge ASIC comes to play. It can convert a 24-bit(8-bits per color) video stream to MIPI DSI, giving me a much bigger selection of FPGAs to work with. Some briges are:
- Toshiba TC358768 or TC358778
- Solomon Systech SSD2828
- Lontium LT8918

The SSD2828 ASIC was chosen due to its availability and the documentation and sample code that can be found on the web.

# A sanity check

Before connecting the SSD2828 to the display, it would be a great idea to evaluate the functionality of the ASIC first. Since the VS035ZSM is quite complex and requires speeds way higher than what I can measure with any oscilloscope, I decided to test a simpler and smaller display first. The AUO H154QN01 display usen on the ipod nano 6th gen. Most importantly, others have worked with this display and documented the process. 

# Making a prototype

