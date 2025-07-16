Dual-Robotic System for Autonomous Search and Rescue

Author: Aravind Sairam Saravanan (2264341)
Supervisor: Dr. Hamidreza Nemati
Submitted: September 5, 2023
Degree: MSc in Robotics, University of Bristol & University of the West of England

⸻

📌 Table of Contents
	•	Project Overview
	•	System Architecture
	•	Technologies Used
	•	Implementation Details
	•	Phase 1: Autonomous Mapping with the Husky Robot
	•	Phase 2: Human Detection & Rescue with a TurtleBot Swarm
	•	Performance Evaluation
	•	Husky Robot Metrics
	•	TurtleBot Swarm Metrics
	•	Project Setup Instructions
	•	Running the Simulation
	•	Conclusion & Future Recommendations
	•	License
	•	Acknowledgments

⸻

🚨 Project Overview

This project addresses the challenge of improving search and rescue operations in complex indoor environments (e.g., office buildings during emergencies) using a dual-robotic autonomous system.

The system is split into two phases:
	•	Husky Robot: Performs autonomous mapping of the unknown indoor space.
	•	TurtleBot Swarm: A group of 20 TurtleBots performs search and rescue operations using the generated map.

The system was simulated in a Gazebo environment and evaluated for localization, obstacle avoidance, and rescue efficiency.

⸻

🧠 System Architecture

✅ Husky Robot (Primary Mapping Unit)
	•	Mobility: 4-wheel drive, rugged design
	•	Sensors: 3D LIDAR, Thermal Camera
	•	Navigation: Mapless (LIDAR-based SLAM)
	•	Software: move_base
	•	Payload: 75kg

✅ TurtleBot Swarm (Search and Rescue Unit)
	•	Mobility: 2-wheel drive, agile
	•	Sensors: 2D LIDAR, RGB-D Camera
	•	Navigation: Map-based (AMCL + A*)
	•	Software: move_base, OpenCV DNN, YOLOv4-tiny
	•	Payload: 5kg

⸻

🧰 Technologies Used
	•	ROS Noetic – Middleware and communication
	•	Gazebo – Simulation environment
	•	RViz – Visualization and debugging
	•	move_base – Navigation stack
	•	Gmapping – 2D SLAM for map generation
	•	AMCL – Monte Carlo localization for TurtleBots
	•	A* – Path planning algorithm
	•	OpenCV DNN – Deep learning integration
	•	YOLOv4-tiny – Pretrained object detection model

⸻

🔧 Implementation Details

Phase 1: Autonomous Mapping with the Husky Robot
	•	Configured Husky with no pre-loaded map.
	•	Used Gmapping to build 2D occupancy grid in real-time.
	•	Saved the generated map using:

rosrun map_server map_saver -f my_office_map



Phase 2: Human Detection & Rescue with a TurtleBot Swarm
	•	Deployed 20 TurtleBots into the mapped Gazebo world.
	•	Each TurtleBot:
	•	Used AMCL for localization.
	•	Navigated using A* algorithm.
	•	Processed RGB-D camera feed using YOLOv4-tiny with OpenCV DNN.
	•	Upon detecting a human:
	•	Cancelled current goal.
	•	Re-planned to nearest exit.
	•	Returned to home location.

⸻

📊 Performance Evaluation

Husky Robot Metrics

Metric	Result	Notes
Avg. Localization Error	1.324 m	Slightly above goal of <1m
Collision Avoidance	72.7%	16/22 obstacles successfully avoided
Avg. Path Length	12.095 m	Measured across 3 runs
Navigation Efficiency	0.156 min/m²	Time per square meter for 243m² area

TurtleBot Swarm Metrics

Metric	Result	Notes
Detection Accuracy	95%	Excellent object detection via YOLOv4-tiny
Guidance Accuracy	68.42%	Dynamic navigation still challenging
Return-to-Home Accuracy	61.54%	Final return stage was inconsistent
Stuck Rate	50%	Half the bots got stuck in tight corridors
Recovery Rate	90%	Custom recovery logic worked effectively


⸻

🛠 Project Setup Instructions

# 1. Install ROS Noetic (Ubuntu 20.04)
# http://wiki.ros.org/noetic/Installation/Ubuntu

# 2. Create a Catkin workspace
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin_make
source devel/setup.bash

# 3. Clone required repositories
cd ~/catkin_ws/src
git clone https://github.com/ros-planning/navigation.git
git clone https://github.com/leggedrobotics/darknet_ros.git
git clone https://github.com/clearpathrobotics/cpr_gazebo.git
# git clone <your project repo>

# 4. Install dependencies and build
cd ~/catkin_ws
rosdep install --from-paths src --ignore-src -r -y
catkin_make


⸻

🚀 Running the Simulation

# Step 1: Launch the office environment
roslaunch your_package_name office_environment.launch

# Step 2: Run Husky for mapping
roslaunch husky_navigation mapless_navigation.launch

# Step 3: Save the map
rosrun map_server map_saver -f office_map

# Step 4: Launch TurtleBot swarm for rescue
roslaunch turtlebot_swarm search_and_rescue.launch map_file:="/path/to/office_map.yaml"


⸻

🧾 Conclusion & Future Recommendations

✅ Accomplishments
	•	Built a full search and rescue simulation pipeline.
	•	Achieved high human detection success with YOLOv4-tiny.
	•	Demonstrated coordination between mapping and rescue robots.

🔧 Recommendations
	•	Better Localization: Use sensor fusion (e.g., EKF) for Husky.
	•	Advanced Navigation: Integrate D* Lite or ML-based planners to reduce stuck rate.
	•	Hardware Testing: Extend project to real-world deployment using physical robots.

⸻

📝 License

This project is licensed under the MIT License – see the LICENSE.md file for details.

⸻

🙏 Acknowledgments
	•	Dr. Hamidreza Nemati (Supervisor)
	•	MSc Robotics Department, University of Bristol & UWE
	•	Friends and peers for motivation and testing support
