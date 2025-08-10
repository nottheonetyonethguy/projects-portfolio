---
title: Autonomous Drone with Visual SLAM and Path Planning
author: Bibek Yonzan
layout: post
---
## Simulation and Real Time Implementation of Visual SLAM in an Indoor Environment using a Quadcopter

<table>
  <tr>
    <td colspan="2" align="center"><b>Team: Anil Kumar Shah, Bibek Yonzan, Lucky Babu Jayswal, Samim Khadka</b></td>
  </tr>
  <tr>
    <td><b>Timeline</b>: 1 year</td>
    <td><b>Technologies</b>: ROS Noetic, Gazebo, PX4, C++, Docker, RTABMap</td>
  </tr>
</table>

{% include image.liquid url="/assets/images/quad.png" description="Final Quadcopter" %}

The goal was to implement a visual SLAM algorithm, in a drone, that enabled autonomous flight in an indoor environment with degraded GPS signals. Indoor autonomous navigation provides unique challenges: GPS denied environments, dynamic obstacles, and a need for real time mapping and localization. Our solution was to use a RGB-D camera with a Pixhawk 4 as the flight controller and a Raspberry Pi 4 as the flight computer to perform localization and mapping the environment, for obstacle free autonomous navigation.

We developed a  comprehensive  autonomous system with RTAB-Map integrated with a PX4-Raspberry Pi flight stack, achieving reliable indoor navigation with 7.7 cm accuracy.

 {% include image.liquid url="/assets/images/setup.png" description="Final Quadcopter" %}

| **Technical Implementation** |

* **RTAB-Map implementation**: Configured and optimized for quadcopter platforms with an RGB-D sensor
* **Height sensing**: Configured a LiDAR to detect the height of the quadcopter from the ground while in flight
* **Loop Closure Detection**: Fine tuned parameters for place and marker recognition in indoor environments
* **Path Planning Implementation**: Implemented a global and local planner for autonomous navigation
	* **Global Planner**: Configured the A* algorithm for global obstacle avoidance and path planning
	* **Local Planner**: Configured Regulated Pure Pursuit (RPP) for local path planning and adjustments
* **PX4 Parameter Tuning**: Adjusted the PX4 parameters for visual inertial odometry and GPS-denied environments
* **Sensor Fusion**: Integrated visual inertial odometry using an RGB-D sensor (Intel D435) and the PX4 IMU for state estimation

{% include image.liquid url="/assets/images/gazebo-env.png" description="Gazebo simulation world" %}

| **Results and Performance** |

The quadcopter was tested in a variation of 3 different environments: a netted cluttered environment, two different live rooms with objects, and the actual outside world. The indoor environment created was a 35 x 25 feet with nets on all four sides. It also contained boxes acting as obstacles, in various combinations of positions. 

{% include image.liquid url="/assets/images/environment.JPG" description="Netted Indoor Environment" %}

The indoor environment had three different configurations: no obstacles (i.e., boxes), single obstacle and two obstacles. Initial height at which the drone was flown is 0.5 m off the ground. The results for each of the configuration are:

<table>
<tr>
	<td><b>Configuration</b></td>
	<td><b>Mean Altitude Deviation</b></td>
</tr>
<tr>
	<td>Zero obstacles</td>
	<td>0.024 m </td>
</tr>
<tr>
	<td>Single Obstacle</td>
	<td>0.035 m</td>
</tr>
<tr>
 <td>Two Obstacles</td>
 <td>0.029 m</td>
</tr>
</table>

{% include image.liquid url="/assets/images/env-result.png" description="Altitude Deviation Plots: Zero (left), Single (right), and Two obstacles (right)" %}

From the test flights, a 3D point cloud map was also obtained. Following is a point cloud map for the 2 obstacles configuration.

{% include video.liquid url="/assets/videos/2_obs.mp4" description="3D Point Cloud Map: 2 obstacle configuration" %}

Next, two different "real" world environments were selected to mimic operations in the real world. A garden before the CES building in our campus, the senior year classroom, and the manufacturing laboratory.

{% include image.liquid url="/assets/images/other-env.png" description="Garden (left), Senior classroom (middle) and Manufacturing Lab (right)" %}

The results and the subsequent plots and point cloud maps are:

<table>
<tr>
	<td><b>Environment</b></td>
	<td><b>Mean Altitude Deviation</b></td>
</tr>
<tr>
	<td>Garden</td>
	<td>0.044 m</td>
</tr>
<tr>
	<td>Senior Classroom</td>
	<td>0.021</td>
</tr>
<tr>
 <td>Manufacturing Lab</td>
 <td>0.026 m</td>
</tr>
</table>

{% include image.liquid url="/assets/images/real-world.png" description="Altitude Deviation Plots: Garden (left), Senior classroom (middle) and Manufacturing Lab (right)" %}

{% include video.liquid url="/assets/videos/manufacturing.mp4" description="3D Point Cloud Map: Manufacturing Lab" %}

