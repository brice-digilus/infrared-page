---
layout: page
title: "Hemispherical Infrared - Sensor"
description: "Sensor calibration and compensation"
---


To Be completed

TODO - aka to be put in this page:
 - Sensor calibration set-up (follow PDF) ?
 - Homogeneity by hand
 - Outlook on calibration using overlap
 - Striping defects
 - Super resolution ?


# Sensor calibration

As we have a good control of where the sensor looks at, we can create a thermal point source, observe it under different angles and extract a good geometric sensor model.

Which is what I've done, allowing to extract spherical aberration, horizontal Field of view, vertical field of view. Sensor Center was not a relevant issue here.

[Sensor calibration notebook](/assets/pdf/Overlap and geometric calibration.pdf)


# Sensor homogeneity

A clear improvement was visible by using a cos^4 law for the temperature relative to the angle from the center.
It's thermal vignetting so it should be more complex than optical vignetting as the "occulting" black piece is an emitter itself, so I think it should depends on the sensor package temperature.
Hopefully between the internal temperature probe and the average IR temperature we should obtain a good proxy.

More work need to be done here, but please enjoy a few results.


# Auto calibration using overlapping images

The great aspect of having a servo mounted sensor is that we can control the overlap of the different images. With this overlap we can adjust a model on the sensor behavior. Either a "by hand" model or a "learned" model.



[HOME](/index.html)

