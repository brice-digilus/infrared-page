---
layout: page
title: "Hemispherical Infrared - Software"
description: "Software documentation for hemispherical infrared camera. From angle picking to combined image"
---


Note: This section is WIP


# Overview of the software

In this section will be covered the software aspects of this project. There will be a lot of stitching, camera parameters estimation as well as communication with the sensor and the servos.
I hope this software can be useful to you, as such it is released under an Apache 2.0 License. Feel free to reach out if any part here is confusing or just to share ideas.

The softare is divided in a few components: 
 - A [angle calculation "library" (in python)](#soft_angles_lib) that is meant to go from the camera viewpoints to cubemaps and vice versa.
 - A set of [preparation tools (in python)](#soft_angles_opt) that allow to select the capturing angles for the hemispherical camera
 - A program to [drive the servos and the sensors (in C++)](#soft_motion_and_capt). This program stores the data in a [TIFF file with custom metadata](#soft_tiff)
 - A set of programs to [analyze the sensor data and project the thermal information onto cubemaps](#soft_image_analysis)
 - A program to [estimate the sensor geometrical information from calibration measurements](#soft_sensor_calibration)
 - A program to [display the cubemap in a browser using Three.JS](#soft_cubemap_threejs)
 - And some extra notes on [quaternions](#soft_quaternions) and [raspberry pi set-up](#soft_pi_set_up)


Here some results examples: TODO


# Preparation/angle optimization tools {#soft_angles_opt}


[Back to top](./)
# Angles calculation "library" {#soft_angles_lib}


[Back to top](./)
# Motion and capture software {#soft_motion_and_capt}


[Back to top](./)
# Sensor calibration software {#soft_sensor_calibration}


[Back to top](./)
# Image analysis software {#soft_image_analysis}


[Back to top](./)
# Display the cubemap using three JS {#soft_cubemap_threejs}




[Back to top](./)
# A few notes on quaternions {#soft_quaternions}

In the software I am using quaternions to describe rotations. Once the set-up is in place, these are a very practical tool to handle arbitrary rotations.
The image stitching is the rotation of the two servos and the rotation associated to the sensor pixels versus the rotation of the point of the cubemap.
So a robust rotation parametrization is key. In this project I have decided to use quaternions, as they do not suffer from gimbal lock, rotation combinations are easy (it's a kind of product), and they are simple to invert.
They are not easy to parametrize directly but the conversion to other rotation parametrizations is well documented.

Quaternions are described in various places, here you can find a link on the key equations used and it should help for notation 

[Some notes about quaternions](/assets/pdf/Angles and rotations.pdf)

[Back to top](./)
# Our TIFF storage and metadata {#soft_tiff}

In order to store the angles and the imaging conditions, we add metadata to the TIFF file containing the raw measurements. The two following sections describe the metadata syntax as well as where is the metadata stored in the TIFF file.

The metadata is stored in the tag 270: ImageDescription.

This is one example of metadata
```
THDS_V0.04//NIM:37//ANH:201.00;241.00;281.00;321.00;361.00;41.00;81.00;121.00;161.00;141.00;101.00;61.00;21.00;341.00;301.00;261.00;221.00;181.00;201.00;241.00;281.00;321.00;361.00;41.00;81.00;121.00;161.00;129.57;78.14;26.71;335.29;283.86;232.43;181.00;181.00;301.00;61.00//ANV:-32.00;-32.00;-32.00;-32.00;-32.00;-32.00;-32.00;-32.00;-32.00;-4.00;-4.00;-4.00;-4.00;-4.00;-4.00;-4.00;-4.00;-4.00;23.00;23.00;23.00;23.00;23.00;23.00;23.00;23.00;23.00;51.00;51.00;51.00;51.00;51.00;51.00;51.00;76.00;76.00;76.00//TA_S:27.47;27.48;27.47;27.47;27.47;27.45;27.45;27.45;27.44;27.44;27.43;27.44;27.44;27.44;27.43;27.43;27.43;27.43;27.44;27.43;27.44;27.42;27.42;27.41;27.31;27.31;27.30;27.31;27.31;27.31;27.31;27.30;27.29;27.39;27.26;27.28;27.26//SFPS:1//TI_SD:20201211//TI_ST:091058//TI_DU:150.15
```
## Metadata definition

The tags start with an header, here ```THDS_V0.04``` followed by pairs ```Name:Value```. Each field is separated by ```\\```

This is the descriptions of the field

| Field Name  | Value Description |
| ----------- | ----------- |
| NIM         |  (always first) the number of images in the file. Example ```NIM:3``` |
| ANH         |  The angles in the "Horizontal plane", corresponding to Y in the angle convention. Each angle is separated by a semicolon ```;``` and in a preffered format ```%.2f```. Example ```ANH:201.00;241.00;281.00```  |
| ANV         |  The angles in the "Vertical plane", corresponding to Xi in the angle convention. Each angle is separated by a semicolon ```;``` and in a preffered format ```%.2f```. Example ```ANV:-32.00;-32.00;-32.00```  |
| SFPS        |  The sensor integration time in seconds. Example ```SFPS:1``` |
| TA_S        |  The sensor internal temperature for each image in Degree Celcius. Example ```TA_S:27.47;27.48;27.47```|
| TI_SD       |  The measurement start date, format ```%Y%m%d```. Example ```TI_SD:20201211``` |
| TI_ST       |  The measurement start time, format ```%H%M%S```. Example ```TI_ST:091058```|
| TI_DU       |  The measurement duration. Example ```TI_DU:150.15```|
| SEN         |  (Future use) The sensor model. Example ```SEN:MLX90640_110```|
| TI_IO       |  (Future use) The time from start for each image. Example ```TI_IO:0;2.3;5.8```|
| TEMP        |  (Future use) The overall temperature. Example ```TEMP:21.5```|


## Metadata storage

This is the structure of the generated TIFF file. The code is inspired from (Paul Bourke http://paulbourke.net/ work).
These are the sections of the generated TIFF file in order. Tiff files can be complex, below is the simplest TIFF file that suited the needs of the project.

| Field Name  | Value | Description |
| ----------- | ----------- | ----------- |
| HEADER | 0x4d4d002a | Big endian & TIFF identifier. Size: 4bytes |
| FIRST IFD OFFSET | "Nx * Ny * 2 + 8" | Where to find the "Image File Directory" which is the description of the file information. Size: 4bytes |
| IMAGE_DATA | Image data | Rraw grayscale, 2Bytes per pixel. Size: Nx * Ny * 2 Bytes |
| This is the start of the IFD  |
| TAG N | 12 | The number of tags in the IFD. Size 2Bytes |
| TAG 0x00fe: Image type |  0 | |
| TAG 0x0100: Image Width | Nx | Number of horizontal pixels |
| TAG 0x0101: Image Height | Ny | Number of vertical pixels |
| TAG 0x0102: Bits/sample | 16 | Grayscale 2bytes/pixel |
| TAG 0x0103: Compression | 1 | uncompressed |
| TAG 0x0106: Photometric Interpretation | 1 | Minimum is black |
| TAG 0x0111: Strip Offsets | 8 |  This is the pointer in bytes on where to find the data. As the data is at the beginning after the two headers its value is 4 + 4 = 8bytes|
| TAG 0x0112: Orientation | 1 | First pixel is top left |
| TAG 0x0115: SamplesPerPixel | 1 |  Grayscale image = 1 sample per pixel |
| TAG 0x0117: StripByteCounts | Nx * Ny * 2 | Number of bytes in the strip. We are doing a single strip image so that's the size of the image data |
| TAG 0x011C: PlanarConfiguration | 1 | How the components of each pixel are stored. 1 = The component values for each pixel are stored contiguously |
| TAG 0x010E: ImageDescription| Pointer to data | TAG_N (tag data len): Even String length. tag data Offset: 8 (Image header) + Nx * Ny * 2 ( Image data ) + 2 (Tag number field size) + 12*12 (Tag information, each tag is 12 Bytes total) + 4 (Next IFD offset field)  |
| NEXT IFD OFFSET | 0x00000000 |  The image is over so we do not have another IFD | 
| IMAGE_DESCRIPTION | our metadata | See appropriate section |
| EOF | 'neo' | Normal tiff parsers must not reach here because the image is over, but for the fun I added a footer. | 



## Reading metadata in python

As simple as this

```
from PIL import Image
img = Image.open('xxx.tiff')
metadata = img.tag_v2.get(270,"").replace("\x00","")
```


[Back to top](./)
# Raspberry pi set-up {#soft_pi_set_up}

run ```raspi_config``` and set-up
 - ssh (if you use it)
 - enable i2c port
 - serial port (if using lidar): no console and hardware enabled

edit ```/boot/config.txt``` and adjust the i2c_frequency (200kHz worked fine for me) and if using lidar, disable bluetooth to free the serial port by adding ```dtoverlay=pi3-disable-bt-overlay```

if using lidar on serial, I run also this command to disable the bluetooth ```sudo systemctl disable hciuart```

Note: for remote display with rdp ```sudo apt purge realvnc-vnc-server && sudo apt install xrdp```. Client wise it works on linux/windows and iPad but not all apps (iteleport works fine)


[Back to top](./)
# Angles convention {#soft_angle_convention}


Here are the conventions used for the angles in this project
![Angles_conventions](/assets/images/201212_Mech_Angles_invert.png)

[Back to top](./)


