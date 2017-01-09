---
layout: post
title: Introducing Polystack
tags: [polystack]
---
![Chickadee Tech Laptimer](https://i.imgur.com/9dUisvx.jpg)
Hi, I'm Scott and I quit my job at Google in June of last year to work for myself. Although the first few projects didn't gain much traction, that didn't dampen the fun I was having and my desire to make something awesome. In September, I caught the hardware bug and created a timing system prototype that kinda worked but not as well as I had liked.

While visiting my Grandma's house in October I was reading a post by Andy Shen of [Shendrones](http://www.shendrones.com/), not sure which, in which he mentioned something about the difficulty of designing for a flight controller's (or FC for short) normal footprint of 36mm by 36mm and the desire to stack vertically. "Thats it!", I thought. Create a smaller form factor FC that stacks modules on top rather than requiring wires running everywhere. While I gave up on the smaller form factor for the time being because frames are designed for the 36mm x 36mm form factor, the modular stacking idea stuck. Modular stacking would make builds cleaner and upgradeable. Now when someone wants additional functionality they can reuse their existing FC instead of rebuying it all. All thats need is a spine for this new, modular stackable system that would carry signals and power between modules. It would be the secret sauce.

## Secret Sauce

I spent hours browsing all types of connectors on Digi-Key and Mouser. Finding a connector that is both durable, high density and has a small footprint is not easy. The connector would carry power and signals to connect the base microcontroller board to everything else. I studied Arduino Shields, Raspberry Pi Hats and Beaglebone Capes to see how they stacked so well. They all use normal pin headers, which are great for providing strong connections between modules but they take a ton of room. A standard FC is 36mm by 36mm or 1.4 inches by 1.4 inches for us Americans. With standard spacing of headers, called pitch, you can fit ~12 pins from one edge to the other. I was hoping for many more signals so that you could expand motor outputs for an octocopter, run GPS, on screen display (OSD), telemetry, remote controls and log flight information to a microSD card all at the same time if you wanted to. I needed something with a finer pin pitch to give more pins in less area.

![](https://i.imgur.com/OHVo1di.jpg)

I settled on the 80-pin Hirose DF40 connector because it [packs 80 pins into a 18.6mm by 3.4mm footprint](https://www.hirose.com/product/en/download_file/key_name/DF40/category/Catalog/doc_file_id/31649/?file_category_id=4&item_id=22&is_series=1) and had stacking heights ranging from 1.5mm to 4mm. Although I hadn't thought of it at first, optimizing the use of vertical space was just as important as horizontal board space in order to fit all potential functionality into a miniquad.

The next challenge was that the DF40 connector was surface mount rather than through hole like the pin headers on the other stackable systems. Unlike on those, these pins wouldn't be automatically connected to each other. Instead, I'd have to use vias, holes drilled through a circuit board and lined with copper, to pass signal from one side of the board to the other. This created one awesome opportunity and one challenge.

The opportunity we have with using traces and vias to pass the signal from one side to the other is to rewire the connector between the incoming signals on the bottom and the outgoing signals on the top. While stacking systems like Arduino Shields and Adafruit Feathers are limited to essentially one expansion above, two if you are careful about pin conflicts, Polystack could rewire the connector on each mod so that pin conflicts never happen.

In order to allow for mods to consume pins, the connector is [divided up into sets of pins](https://github.com/chickadee-tech/polystack/wiki/Pin-Definitions) that can all perform a certain protocol such as GPIO, SPI and UART. The pins that a mod must exclusively use are consumed by the mod and not passed upwards. This could cause trouble if the mod above uses the same pin positions, however, the mod below also shifts the remaining pins of the same type into the spot where the pins it used are. This way, both mods can have exclusive access to pins which do what it needs.

![How pins get shifted in the Polystack connector](https://i.imgur.com/VcGEK50.png)

The challenge with using vias is minimizing the board area required for all 80 of the vias and the traces that run to them. It took a lot of trial and error to come up with a decent layout but I came up with the one below, dubbed the butterfly. Each via is as close to the next as it can be and the traces are too. The pin functionality is designated so that pins are shifted outwards which gives them more room to shift into. This layout is autogenerated by [a Python script](https://github.com/chickadee-tech/polystack/blob/master/tools/start_pcb/start_pcb.py) when you start the mod so don't worry too much about having to lay this all out.

Once the connection system was established and tested, Polystack was born. Its unique connection system allows for up to seven mods without worrying about pin conflicts!

## Polystack
Fast forward many months of testing, crashing, tweaking, retesting and a couple regulator fires and we have Polystack. Polystack is a new platform for modular microcontrol systems. Originally designed as a modular flight controller for multirotors dubbed FlightStack (in case you hear it in old videos I made), Polystack features an innovative connectivity system that enables unrivaled expansion by stacking modules (or mods) on a base control board. This connectivity allows for flight controllers which can be adapted from a lean mean racing machine to a tricked out autonomous flying machine. Soon this will expand to include all varieties of projects that can be done with an Arduino-compatible control board. Follow its development on our Twitch stream and watch past streams on YouTube.

![](http://i.imgur.com/VSnFupc.jpg)

Check here over the next few weeks for introductions to my company Chickadee Tech, our philosophy and new ways of doing things. Or you can subscribe to our RSS feed. Thanks!