# 🤖 TurtleBot3 Autonomous Navigation in Gazebo

<p align="center">

**ROS 2 Jazzy • Gazebo • Nav2 • AMCL • LiDAR • RViz**

</p>

<p align="center">

Autonomous navigation and obstacle avoidance of a TurtleBot3 Burger
in a simulated Gazebo environment using ROS 2.

</p>

---

## 📌 Overview

This project demonstrates autonomous navigation of a **TurtleBot3 Burger** in a Gazebo simulation using the ROS 2 navigation stack.

The system integrates:

- Gazebo simulation
- TurtleBot3 Burger
- 2D LiDAR
- Odometry
- SLAM-generated occupancy-grid map
- AMCL localization
- Nav2 autonomous navigation
- RViz2 visualization
- Obstacle avoidance

The robot can be given navigation goals through RViz and autonomously plan and execute a path while avoiding obstacles.

---

## 🏗️ System Architecture

```text
                    ┌─────────────────┐
                    │     RViz2       │
                    │  2D Goal Pose   │
                    └────────┬────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │      Nav2       │
                    │                 │
                    │ Planner +       │
                    │ Controller      │
                    └────────┬────────┘
                             │
                         /cmd_vel
                             │
                             ▼
                    ┌─────────────────┐
                    │     Gazebo      │
                    │   TurtleBot3    │
                    └───────┬─────────┘
                            │
                 ┌──────────┴──────────┐
                 ▼                     ▼
               /scan                 /odom
                 │                     │
                 ▼                     ▼
              LiDAR                Odometry
                 │                     │
                 └──────────┬──────────┘
                            ▼
                           AMCL
                            │
                           /map
TECHNOLOGIES:

| Component           | Technology         |
| ------------------- | ------------------ |
| Operating System    | Ubuntu             |
| Robotics Middleware | ROS 2 Jazzy        |
| Robot               | TurtleBot3 Burger  |
| Simulation          | Gazebo             |
| Visualization       | RViz2              |
| Localization        | AMCL               |
| Navigation          | Nav2               |
| Range Sensor        | 2D LiDAR           |
| Mapping             | SLAM               |
| Robot Model         | Differential Drive |

📡 Important ROS 2 Interfaces
LiDAR
/scan
sensor_msgs/msg/LaserScan
Odometry
/odom
nav_msgs/msg/Odometry
Velocity Command
/cmd_vel
geometry_msgs/msg/TwistStamped
Localization
/amcl_pose
geometry_msgs/msg/PoseWithCovarianceStamped
Navigation
/navigate_to_pose
🗺️ Mapping & Localization

A 2D occupancy-grid map was generated using SLAM and saved for later navigation.

The saved map consists of:

maps/
├── turtlebot3_slam_map.pgm
└── turtlebot3_slam_map.yaml

AMCL is then used to estimate the robot's pose within the saved map.

The primary TF chain is:

map
 └── odom
      └── base_footprint
           └── base_link
🧭 Autonomous Navigation

Nav2 provides the autonomous navigation pipeline, including:

Global path planning
Local trajectory control
Goal handling
Obstacle avoidance
Recovery behaviors

Navigation goals are provided through RViz using the 2D Goal Pose tool.

The robot then plans a path toward the selected goal and navigates through the simulated environment.

📁 Repository Structure
turtlebot3-nav2-gazebo/
│
├── config/
│   └── burger.yaml
│
├── maps/
│   ├── turtlebot3_slam_map.pgm
│   └── turtlebot3_slam_map.yaml
│
├── rviz/
│   └── navigation_baseline.rviz
│
├── docs/
│
└── README.md
🚀 Running the Simulation
1. Set the TurtleBot3 model
export TURTLEBOT3_MODEL=burger
2. Launch Gazebo
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
3. Launch Nav2

Use the saved map and TurtleBot3 navigation parameters:

ros2 launch nav2_bringup bringup_launch.py \
  map:=/path/to/turtlebot3_slam_map.yaml \
  use_sim_time:=True \
  params_file:=/path/to/burger.yaml
4. Launch RViz
rviz2 -d /path/to/navigation_baseline.rviz
5. Send a navigation goal

In RViz:

2D Goal Pose → Click and drag on the map

Nav2 will calculate and execute a path toward the selected goal.

✅ Verified Functionality

The following components were successfully tested:

 TurtleBot3 Burger simulation
 Gazebo world
 LiDAR sensor
 Odometry
 TF tree
 SLAM map
 Saved occupancy-grid map
 AMCL localization
 Nav2
 RViz visualization
 Autonomous navigation
 Obstacle avoidance
 Multiple navigation goals
