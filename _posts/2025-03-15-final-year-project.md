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

At the start of the year, the plan was to divide the task into three sections: implement SLAM, add path planning and refine with test flights. The start of the project was quite slow. It was spent mostly on being up to date with robotics, autonomous flight and the ROS ecosystem. The group was divided into two halves; each sub-group learning ROS Noetic and ROS 2 Humble. Once we were comfortable working with ROS and the Gazebo ecosystem, our advisor/supervisor advised to try out ORB3 in a DJI Tello drone. The team dove headfirst into the task, which admittedly is bad practice. However, this was necessary for us to learn the importance of simulations and test environments in robotics.

{% include image_square.liquid url="/assets/images/tello_0.jpg" description="DJI Tello, source: Amazon" %}

The biggest help to us during this process was this Qiita forum post. The only problem with that post was that it was entirely in [Japanese](https://qiita.com/k-koh/items/1a4fbc175ca37be6206b). So after some tedious translations, we were able to get ORB3 running on the tiny Tello. Even though, we'd managed to get ORB3 running on the Tello, it's hardware could not handle ORB3 object detection. The Tello would frequently overheat and the feed would lag. We tried setting the video resolution to 144p 10 fps even, but even that was too much throughput for the small drone. 

{% include image.liquid url="/assets/images/tello_1.png" description="Screenshot of ORB3 through the monocular Tello camera" %}

The original plan for this project was to fabricate a quadcopter, add a depth camera to it and fly it in a cluttered environment, but being able to make it work first in a test subject like the Tello was important. However, at that point in time, we weren't confident in the Tello's processing power to run ORB3 smoothly, or any SLAM algorithm for that matter, so it had to be dropped.    

{% include image.liquid url="https://lh7-rt.googleusercontent.com/slidesz/AGV_vUej5zZrBpifI3rszeQdaBM1WaZs8iArgh1mk0JynU2NO4uBo29CA8RQTnTKmkTIskltHm34bfzKGPCjJezo4wS6MV1nyyHu5lqMPWursuAuQkVXq-EsHig-wVEaJM-OMEqUT2r97CZsaQwcCefWtNo=s2048?key=JBnHMngRatBQUCvjWtIccA" description="RViz with ORB3 running on the Tello" %}

## The Pixhawk 4 and Gazebo Simulations

Once it was sure that the Tello was not suitable for our testing purposes, we build the quadcopter for our project. If we wanted to test out various algorithms, we were going to need a strong frame capable with some processing power to handle said algorithms. So, for this we made a flight stack of a Pixhawk 4 as the flight controller, a Raspberry Pi 4 as the onboard flight computer and an Intel D435 depth camera. Initially, we'd ordered a D435i for this purpose but the order got mixed up and we ended up with a D435 camera. The plan was to ssh into the onboard Raspberry Pi from a laptop, acting as ground control and launch the necessary launch files. The Pixhawk would communicate with the onboard computer via a serial port, while the Pi, running Ubuntu MATE 20.04, would be connected in the same network as the ground control computers. At first, a [Docker](2024-08-09-docker-setup.md) based setup was thought, it would make running multiple container with different versions of ROS easier, but was quickly dropped in favor for better computing efficiency. 

{% include image.liquid url = "/assets/images/setup.png"  description = "Working Setup" %}

The next step was to be familiar with the Pixhawk,specifically offboard controlled Pixhawk. For this Alireza Ghaderi's [MAVSDK-Python](https://github.com/alireza787b/MAVSDK-Python) repository came in handy. It came with set examples and was a good introduction to MAVSDK and mavlink. We were not able to replicate Alireza's examples exactly as in his repositories but we did manage control the quadcopter using an offboard control and fly it on set points from a CSV file. There were some technical difficulties at first, though. One thing we quickly found out was that, the Pixhwak, and subsequently QGroundControl, does not arm the drone unless there are sufficient GPS satellites. At first, the plan was to not include the GPS module, but upon discovering this, it had to be included. 

{% include image.liquid url = "/assets/images/quad-trajectory.png"  description = "Drone trajectory (left) and the data obtained from the Pixhawk, showing the trajectory (right)" %}

 {% include video.liquid url = "/assets/videos/offboard-1.mp4"  description = "test video" %} 