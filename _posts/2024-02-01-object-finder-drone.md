---
layout: post
title: Building an Object Finder Drone, A Computer Vision Project
date: 2024-01-01
description: A lightweight drone project combining computer vision and FreeRTOS for search applications.
tags: [FreeRTOS, Embedded Computing, Software Design, Hardware]
color: danger
categories: projects
---

## Overview

I'm working on an exciting project that combines drone technology with computer vision to create a lightweight, cost-effective search and tracking solution. This drone system is designed to assist in various search operations while adhering to Canadian aviation regulations.


## Key Features

- Lightweight design (under 250g total weight)
- 360-degree vision system using a rotating camera
- Real-time object detection using deep learning
- FreeRTOS-based flight control system
- Wireless communication for ground control
- Target budget of $150 for core components


## Use Cases

The system is designed for several practical applications:

- Search and rescue operations for missing persons/pets
- Wildlife monitoring near campgrounds
- Wildfire progression tracking
- Law enforcement support for vehicle tracking



### Flight Control System

- **Operating System**: FreeRTOS
- **Core Sensors**: 
    - Gyroscope for orientation
    - Accelerometer for motion tracking


### Computer Vision System

- **Hardware**: Single rotating camera for 360° coverage
- **Software Stack**:
    - TensorFlow for object detection
    - OpenCV for image processing
    - Pre-trained models (SSD/Faster R-CNN/EfficientDet)


### Safety Features

- Automatic return-to-home capability
- Safe landing protocols
- Real-time flight monitoring


## Regulatory Compliance

The project is designed to comply with Transport Canada regulations:

- Sub-250g weight classification
- Maximum altitude limit of 122 meters
- Privacy-focused design with no data storage
- Testing in controlled, unpopulated areas


## Current Status

[Project status and next steps will be updated as development progresses]


## Budget Overview

Working within a $150 budget for core components (excluding development tools and external computing resources).


---

*This is an ongoing project. Follow along for updates on the development process and technical deep-dives into specific components.*
