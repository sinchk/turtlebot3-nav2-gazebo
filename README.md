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
