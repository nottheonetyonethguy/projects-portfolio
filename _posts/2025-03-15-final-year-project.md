---
title: "Final Year Project: Autonomous Quadcopter"
author: Bibek Yonzan
layout: post
---
## Table of Contents

* TOC
{:toc}

## Introduction

For the final year capstone project, my teammates and I decided to develop an autonomous aerial system by implementing a SLAM and an obstacle avoiding path planning algorithm in a quadcopter. The project timeline was essentially categorized into 3 parts: first select a suitable SLAM algorithm, second decide on a path planning algorithm and finally carry out test flights and log final flight data. The flight stack for this quadcopter was a Pixhawk 4 with a Raspberry Pi 4, either the 8 GB or 4 GB variant. After testing out various SLAM algorithms (ORB3, LSD, RTABMap), we settled on RTABMap as the final SLAM algorithm. The reason for this was primarily based on what little processing power the Raspberry Pi offered. At first, we weren't sure whether we would be using the 8GB or 4GB variant. So the reasonable choice was RTABMap. Initially however, we were advised to try out ORB3 but that turned out to be severely computationally heavy. So RTABMap was the choice algorithm. 

Second, the final choice for the path planning algorithm was A* as the global planner and Regulated Pure Pursuit, or RPP, as the local planner. The selection process for the path planning algorithm was rather tedious, involving countless test flights. The process went through various stages, which is explained in detail in sections below. The test sections were hectic and banal, to no one's surprise. The entire project, end-to-end, took about 10 months and involved serious testing and simulations. The further sections chronicle our entire development and testing phase.

## Beginnings and DJI Tello

Initially when the group came together, none of us had any idea about SLAM or robotics. We were four aerospace students with experience in building fixed wing UAVs and flight models in XPlane but not robotic systems. The first few weeks were spent on learning the basics of robotics and ROS (Robot Operating System). One of my teammates and I had experience using Linux (I used to daily drive a dual-boot Ubuntu-Windows 15 year old dinosaur) but the other two teammates had to get their hands set. Even though we had experience with the Linux system and the shell, ROS was a different beast. At first we weren't sure whether to use the updated ROS2 or the tried and tested ROS1. So, the group was divided in halves; each half learning ROS1 and ROS2. I tried my hands with ROS2. Simple URDFs, xacros and colcon build commands. Quick reminder that the curriculum we were studying focused heavily on aerospace core subjects. Your control systems, flight dynamics and aircraft systems. None of us had any C++ or Python experience except for some handful simple programming. I had experience with Python OOP when I used to make MANIM videos for my presentations but that was it. So the first few weeks was spent trying to get the grasp of C++/Python basics. Now the first hurdle we faced was the lack of compatibility. ROS1's latest version, "Noetic" is compatible with only Ubuntu 20.04, while the ROS2 version we were trying out "Humble" is compatible with Ubuntu 22.04. So my first few weeks was actually spent trying to dual boot as many computers in the design lab of our department with Ubuntu 20 and 22. Nerve wracking, because I have had instances where I'd wiped out the disk with the Windows installation, so needless to say, I was pretty scared. 

Once that was sorted out, our instructor/supervisor advised us to try out ORB3 on a DJI Tello. This was the first real challenge we faced. We dived head first into trying this out on actual Tello hardware, instead of trying it out with simulations first. Bad practice, but it was beneficial because the Tello hardware could not handle ORB3 object detection. The Tello would frequently overheat and the feed would lag. We tried setting the video resolution to 144p 10 fps even, but even that was too much for the small drone. 

{% include image_square.liquid url="/assets/images/tello_0.jpg" description="DJI Tello, source: Amazon" %}

The biggest help to us during this process was this Qiita forum post. The only problem with that post was that it was entirely in [Japanese](https://qiita.com/k-koh/items/1a4fbc175ca37be6206b). So after some tedious translations, we were able to get ORB3 running on the tiny Tello. 

{% include image.liquid url="/assets/images/tello_1.png" description="Screenshot of ORB3 through the monocular Tello camera" %}