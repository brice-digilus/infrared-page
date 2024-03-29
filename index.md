---
layout: home
---



# Hemispherical Infrared sensor

## Goal/summary

The overall goal of this project is to build an infrared sensor that have an overall view (hemispherical) and can get time sequences of infrared images. All of this using inexpensive off the shelve components.

This project is a proof of concept to explore with thermography. I believe that by measuring over extended time periods and with a large field of view, one can get an understanding of a building behavior that is complementary from spot measurements. I think by correlating with other parameters like outside temperature and heating/cooling load one can infer general characteristics and especially weak points that impede the energy performance of the enveloppe.

In general with these super cheap infrared sensors I was quite happy about the results. I believe with some improvements in the processing and potentially slightly more expensive sensors one could expect very good results.

The main lesson is the stitching, good characterization of the optical response of the sensor was key. This can be done without complicated targets. A single point source of heat can do it, thanks to the motorization.

More details can be found on the various sections of the website.

This project is on hold and given for curiosity or for anyone that want to take it further. Feel free to contact me if any questions/comments/want to discuss need arise.

For the lastest updates please see the [Updates](/Updates) section. 

## Ok but in real life ?

This is what the prototypes look like (more images in the [Photos](/Photos) section)

![HIrv01](/assets/photos/20201212/DSC8047_400px.jpg){:height="35%" width="35%"}
![HIrv02](/assets/photos/202102xx/DSC9074.jpg){:height="35%" width="52.5%"}

And a self portrait in my living room, one can see the lightbulbs at the top, the water heaters, the TV and the window frames.
![Living_room](/assets/images/20201217_Living_room.png)

To know more about cubemap projection please read this [wikipedia article](https://en.wikipedia.org/wiki/Cube_mapping), the example below comes from [Arieee](https://en.wikipedia.org/wiki/Cube_mapping#/media/File:Skybox_example.png) CC BY-SA 3.0

![Cubemap example](https://upload.wikimedia.org/wikipedia/commons/b/b4/Skybox_example.png){:height="40%" width="40%"}

This page is also visible reprojected on a cube [Click here and have fun](/Thermweb)


**Why hemispherical, time-sequences ?**

Most infrared sensors on the market aim for "single-shot" diagnostics. I wanted here to explore more of the idea of an infrared "surveillance"/"overall monitoring" camera. Where information is not in pure resolution but spacial and time dynamics. Also it is interesting to combine it with a Lidar sensor (WIP) so the infrared image can be segmented according to the spacial shape. It's a curiosity project around infrared imaging and it is also a curiosity project around pushing low cost sensors. I think it can have applications in building science or any big place where one is curious about heat flow...

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


