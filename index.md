---
layout: home
---


[//]: # (TODO:)
[//]: # ( - Results images)


# Hemispherical Infrared sensor

## Goal

The overall goal of this project is to build an infrared sensor that have an overall view (hemispherical) and can get time sequences of infrared images. All of this using inexpensive off the shelve components.

For the lastest updates please see the [Updates](/Updates) section

## Ok but in real life ?

This is what one of the prototypes look like (more images in the [Photos](/Photos) section)

![HIrv01](/assets/photos/20201212/DSC8047_400px.jpg)

And a self portrait in my living room, one can see the lightbulbs at the top, the water heaters, the TV and the window frames.
![Living_room](/assets/images/20201217_Living_room.png)

This page is also visible reprojected on a cube [Click here and have fun](/Thermweb)


**Why hemispherical, time-sequences ?**

Most infrared sensors on the market aim for "single-shot" diagnostics. I wanted here to explore more of the idea of an infrared "surveillance"/"overall monitoring" camera. Where information is not in pure resolution but spacial and time dynamics. Also it is interesting to combine it with a Lidar sensor (WIP) so the infrared image can be segmented according to the spacial shape.

**Why inexpensive off the shelve ?**

Because it's a side project :-). I find interesting to take cheap devices and try to extract the information that they provide. I also want other people to potentially build upon this, so the cheapest and easiest the better while keeping in mind a minimum performance (see the servo choice in the [Hardware](/Hardware) page). Of course there is no fundamental limitation to swap the 32px sensor with a higher resolution besides cost.

## Status

Today, the current prototype can 
 - capture a hemispherical infrared image
 - store this information in a TIFF with custom metadata for transfer
 - stitch the images onto a cubemap
 - compensate for the sensor spherical aberration, non square pixel and vignetting
 - It also includes some helper tools to choose the angles to be performed and calibrate the sensor.

The image capture is done on a raspberry pi, but the code is in C++ and very lightweight so with a very minor rewrite for the I2C and servo communication it can be ported to other arm processors like Teensy, Cortex-Mx, STM32.

The sensor spherical aberration, field of view and pixel aspect ratio have been measured (cf [Hardware](/Hardware) section) and taken into account. For the sensor used these parameters are far from ideal, thus a proper calibration is necessary but only a thermal point source is necessary for this.

Sensor vigneting is also compensated and can be measured through the image overlap (WIP).

Beyond this, I am looking into: 
 - Using image overlap for super resolution
 - Using image overlap to train a sensor model to compensate for inhomogeneities
 - Combine infrared data with lidar measurements to map on a pseudo 3D model the infrared images as well as its visualization with threeJS
 - More streamlined processing, and video generation
 - Rework the holder, using only laser cut wood


## Looking a bit closer

This prokect site is divided into a few sections to ease reading.

 - For an overall architecture view and hardware consideration please refer to the [Hardware](/Hardware) page.
 - If you just want to see what it looks like please have a look to the [Photos](/Photos) section.
 - If you want more information on the sensor calibration and consideration, refer to the [Sensor](/Sensor) section.
 - Finally if you want to know more about the underlying calculations and the software in general, please have a look to the [Software](/Software) section.



## A note about licenses

The content of this website is licenced under a Creative commons BY-NC-NA v4.0. The source code is licensed under an Apache 2.0 License.
Please contact me if you have specific needs that are not consistent with these licenses.

## Contact

Feel free to send comments, ideas, support, to infrared __ at __ bricedv __ . __ net


