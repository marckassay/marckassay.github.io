---
layout: post
title:  "Philips Sonicare Repair"
date:   2020-05-15 14:01:58 -0400
categories: electronics
---
 
There was a moment in my life of performing a ritual of gathering my thoughts while therapeutically practicing dental hygiene came to an unexpected halt. My Philips Sonicare stopped working after 2 years of usage. When this occurred, for days my thoughts turned into a dialogue of Socratic questioning within myself, followed by inductive reasoning. These thoughts were amplified by having a stance of anti-consumerism where I resisted replacing this device without acquiring further knowledge on the possible causes of failure, and more importantly solutions for this now comatose device. My curiosity became the catalyst of my research.
 
![Philip Sonicare DiamondClean - Black edition](/assets/2020-05-15/media-sm.jpg)
 
## First Attempt
 
Disassembling this device isn't exactly an easy task, as it requires patience, adequate tools, and tremendous force to open this device. There is not a single screw, cover, cap, collet that I initially noticed for a point-of-attack on how to disassemble this device. In my first hour or so of research, I came across online several people having issues with dead batteries that just needed to be replaced. Thankfully, there is a procedure online at [toothbrushbattery.com](https://toothbrushbattery.com/guides/philips-sonicare-diamondclean-hx9340-battery-replacement/) to replace the battery. This procedure involves disassembling it, but it's not exactly designed to be disassembled.
 
Guessing the material of the device's body, perhaps flexible PVC. It’s flexible, which is good because following the disassembly procedure requires deforming the bottom end of the body to form an almost circular profile to enable a probe to hook the inside of the cap. Once removed, the "tremendous force" I mentioned is to dislodge the internal body from its external body. This required me to firmly grip the external body and gradually apply all my body weight over the shaft while being pressed into a sacrificial piece of wood. Once dislodged, the internal body was able to be slipped out.
 
![internal body](/assets/2020-05-15/overview-sm.jpg)
 
The internal body is quite impressive and seems to be well engineered. The left arrow is pointing to the transistor that is mentioned below, while the right arrow points to the battery fuse.
 
Without reiterating what has been mentioned in the analysis by Neil Hao at [uniteng.com](https://uniteng.com/index.php/2014/01/25/philips-sonicare-electric-toothbrush-measurements-without-teardown/), I did notice something obvious and suspicious nearby the transistor next to the 6 gold-colored circles, there was white residue. It wasn't something that seemed to have been from manufacturing, or that seeped out of this transistor but exploded from it. And perhaps it did. This picture, along with the one above, was taken after I cleaned the circuit board with isotopic alcohol.
 
![original MOSFET](/assets/2020-05-15/old-mosfet-sm.jpg)
 
Engraved on this transistor are the letters 'TSC', which indicates the manufacturer is 'Taiwan Semiconductor Manufacturing Company’. Since I suspected that this transistor was the culprit, I ordered replacements from [Digi-Key](https://www.digikey.com/) and where it’s [datasheet](/assets/2020-05-15/TSM6963SD_D15.pdf) is also available. For less than $10 with delivery included, I was willing to attempt to replace it. I purchased 5 of them, with anticipation of destroying some unintentionally during installation.
 
![Digi-Key shipment](/assets/2020-05-15/digikey-order-sm.jpg)
 
This image shows the MOSFET under magnify glass:
 
![original MOSFET under-mircoscope](/assets/2020-05-15/under-microscope-sm.jpg)
 
And this image with it removed:
 
![removed MOSFET under-mircoscope](/assets/2020-05-15/under-microscope-old-removed-sm.jpg)
 
And finally, new MOSFET installed:
 
![new MOSFET under-mircoscope](/assets/2020-05-15/under-microscope-new-added-sm.jpg)
 
Over the course of a few weeks of making attempts to solder this transistor, I tested the IC (integrated circuit) battery fuse and it needed to be replaced too, as it no longer had continuity. So I purchased from Digi-Key again and made the replacement.
 
After difficulties soldering the IC transistor and battery fuse, I cleaned the circuit board once more and doing so had little confidence that it would function when energized. These chips can only endure so much heat before melting and destroying them internally. So I pushed the power button with anticipation of no signs of life, and when I did, after 2 years of being decommissioned it was oscillating once again!
 
Unfortunately, after using the device over the course of a few days, the power was diminishing. And soon after, eventually stopped working. I did have plans to disassemble the device again and replace the battery which is a Li-ion 18650. I did disassemble it, but attempted to re-solder the transistor since it wasn’t installed well. Doing so, I ended up destroying the IC contacts. After several attempts to repair these contacts, the circuit board wasn't looking good. And that's the current state of this device.
 
I'm not exactly sure why I posted this story other than perhaps it's the beginning of more similar stories to come. I guess time will tell.
