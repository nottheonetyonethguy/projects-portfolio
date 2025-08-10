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

{% include image.liquid url="/assets/images/quad.png" %}

The goal was to implement a visual SLAM algorithm, in a drone, that enabled autonomous flight in an indoor environment with degraded GPS signals. Indoor autonomous navigation provides unique challenges: GPS denied environments, dynamic obstacles, and a need for real time mapping and localization. Our solution was to use a RGB-D camera with a Pixhawk 4 as the flight controller and a Raspberry Pi 4 as the flight computer to perform localization and mapping the environment, for obstacle free autonomous navigation.

We developed a  comprehensive  autonomous system with RTAB-Map integrated with a PX4-Raspberry Pi flight stack, achieving reliable indoor navigation with 7.7 cm accuracy.

 {% include image.liquid url="/assets/images/setup.png" description="Working Setup" %}

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

{% include video.liquid url="https://i.imgur.com/W0bd120.mp4" description="3D Point Cloud Map: 2 obstacle configuration" %}

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

{% include video.liquid url="https://i.imgur.com/SznT68X.mp4" description="3D Point Cloud Map: Manufacturing Lab" %}

| **Further Work and Issues** |

1. The processing power could be improved. Instead of a Raspberry Pi, a Jetson Nano or an Intel NUC could be used. The processing power was one of the major bottlenecks in this project, and a GPU powered, or a simply more powerful CPU could boost this project tremendously.
2. Another major issue faced was during sensor fusion. Partially due to the processing limitations of the onboard computer as well as the mismatch between the polling rates of the image sensor and the IMU. A camera with an IMU unit could be used to replace the main sensing unit, thus reducing the dependance on external sensor fusion. 
3. Due to economical limitations, ground truth could not be measured. As such proper quantitative analysis could not be done. Installing motion capture capture cameras could help gather ground truth data, thus making the results and analysis further meaningful.

| **Flight Test Videos** |

This is one of the first test flights, here we are trying to test out offboard controls on the drone for the first time. 

{% include embed.liquid url="https://www.youtube.com/embed/ISGFztw0mzk?&amp;mute=1" description="Initial Test Flight" %}

This is one of the tests at the indoor environment at IIEC.

{% include embed.liquid url="https://www.youtube.com/embed/cv64lAjkGY8?&amp;mute=1" description="Test Flight: Zero Obstacles at IIEC" %}

This is the one of the final test flights, this one is from the senior classroom environment. The whole final test flights can be found [here](https://youtube.com/playlist?list=PLkHhU0xN0fkKgWfZG8JTeAtxZkMeFaX9Y&feature=shared). 

{% include embed.liquid url="https://www.youtube.com/embed/IfEmNWopbNU?list=PLkHhU0xN0fkKgWfZG8JTeAtxZkMeFaX9Y&amp;mute=1" description="Final Test Flight: Senior Classroom" %}

### References

1. [https://github.com/77bas-SLAMdrone/indoor-slam-drone](https://github.com/77bas-SLAMdrone/indoor-slam-drone)
2. [https://github.com/matlabbe/rtabmap_drone_example](https://github.com/matlabbe/rtabmap_drone_example)
3. [RTAB-Map as an Open-Source Lidar and Visual SLAM Library for Large-Scale and Long-Term Online Operation](https://arxiv.org/abs/2403.06341)
4. [Project Report](https://www.dropbox.com/scl/fi/we7jity7zmpx6d9xaidcv/077BAS_Group2_SLAM_Drone.pdf?rlkey=ax4inm95uitoaz93x0q0ss7nv&st=nv07c6ek&dl=0)
