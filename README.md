# 🤖 TurtleBot3 Autonomous Navigation in Gazebo

<p align="center">

`ROS 2 Jazzy` • `Gazebo` • `Nav2` • `AMCL` • `LiDAR` • `RViz2`

</p>

<p align="center">

Autonomous navigation and obstacle avoidance of a TurtleBot3 Burger
in a simulated Gazebo environment using ROS 2.

</p>

---

## 📌 Overview

This project demonstrates autonomous navigation of a **TurtleBot3 Burger** in a Gazebo simulation environment using the ROS 2 navigation stack.

The system integrates:

- `Gazebo` for robot simulation
- `TurtleBot3 Burger` differential-drive robot
- `2D LiDAR` for environment sensing
- `Odometry` for motion estimation
- `SLAM` for map generation
- `AMCL` for robot localization
- `Nav2` for autonomous navigation
- `RViz2` for visualization and navigation goals

The robot can be given navigation goals through RViz2 and autonomously plan and execute paths while avoiding obstacles in the simulated environment.

---

## 🏗️ System Architecture

```text
                         RViz2
                           │
                    2D Goal Pose
                           │
                           ▼
                    ┌──────────────┐
                    │     Nav2     │
                    │              │
                    │   Planner    │
                    │      +       │
                    │  Controller  │
                    └──────┬───────┘
                           │
                       /cmd_vel
                           │
                           ▼
                    ┌──────────────┐
                    │    Gazebo    │
                    │              │
                    │  TurtleBot3  │
                    │    Burger    │
                    └──────┬───────┘
                           │
                 ┌─────────┴─────────┐
                 │                   │
               /scan               /odom
                 │                   │
                 ▼                   ▼
              2D LiDAR           Odometry
                 │                   │
                 └─────────┬─────────┘
                           │
                           ▼
                          AMCL
                           │
                           ▼
                          /map


| Component           | Technology           |
| ------------------- | -------------------- |
| Operating System    | `Ubuntu`             |
| Robotics Middleware | `ROS 2 Jazzy`        |
| Simulation          | `Gazebo`             |
| Robot               | `TurtleBot3 Burger`  |
| Navigation          | `Nav2`               |
| Localization        | `AMCL`               |
| Visualization       | `RViz2`              |
| Range Sensor        | `2D LiDAR`           |
| Mapping             | `SLAM`               |
| Drive System        | `Differential Drive` |
