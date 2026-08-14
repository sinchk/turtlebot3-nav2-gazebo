# 🤖 TurtleBot3 Autonomous Navigation in Gazebo

<p align="center">
  <strong>ROS 2 Jazzy · Gazebo · Nav2 · AMCL · LiDAR · RViz2</strong>
</p>

<p align="center">
  Autonomous navigation and obstacle avoidance of a TurtleBot3 Burger
  in a simulated Gazebo environment using ROS 2.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/ROS%202-Jazzy-blue" alt="ROS 2 Jazzy">
  <img src="https://img.shields.io/badge/Gazebo-Simulation-orange" alt="Gazebo">
  <img src="https://img.shields.io/badge/Nav2-Navigation-green" alt="Nav2">
  <img src="https://img.shields.io/badge/Robot-TurtleBot3%20Burger-red" alt="TurtleBot3">
  <img src="https://img.shields.io/badge/Status-Working-success" alt="Project Status">
</p>

---

## 📌 Overview

This project demonstrates a complete autonomous navigation pipeline for a **TurtleBot3 Burger** in a simulated Gazebo environment using **ROS 2 Jazzy** and the **Nav2 navigation stack**.

The robot uses a 2D LiDAR for environment perception, odometry for motion estimation, AMCL for localization, and Nav2 for path planning, trajectory control, and obstacle avoidance.

The project was developed and tested entirely in simulation.

### Core Capabilities

- 🏠 Gazebo-based robot simulation
- 🤖 TurtleBot3 Burger differential-drive robot
- 📡 2D LiDAR perception
- 📍 Odometry
- 🗺️ SLAM-generated occupancy-grid map
- 🎯 AMCL localization
- 🧭 Nav2 autonomous navigation
- 🛡️ Obstacle avoidance
- 🖥️ RViz2 visualization
- 🎯 Goal-based navigation using `2D Goal Pose`

---

## 🏗️ System Architecture

<p align="center">

```text
                         ┌─────────────────────────┐
                         │         RViz2           │
                         │                         │
                         │      2D Goal Pose       │
                         └────────────┬────────────┘
                                      │
                                      │ Navigation Goal
                                      ▼
                         ┌─────────────────────────┐
                         │          Nav2           │
                         │                         │
                         │    BT Navigator         │
                         │          │              │
                         │    ┌─────┴─────┐        │
                         │    ▼           ▼        │
                         │ Planner     Controller  │
                         │ Server       Server     │
                         └────────────┬────────────┘
                                      │
                                      │ /cmd_vel
                                      ▼
                         ┌─────────────────────────┐
                         │         Gazebo          │
                         │                         │
                         │     TurtleBot3 Burger   │
                         │                         │
                         │    Differential Drive   │
                         └────────────┬────────────┘
                                      │
                         ┌────────────┴────────────┐
                         │                         │
                         ▼                         ▼
                    ┌─────────┐               ┌─────────┐
                    │  LiDAR  │               │ Odometry│
                    │  /scan  │               │  /odom  │
                    └────┬────┘               └────┬────┘
                         │                         │
                         └────────────┬────────────┘
                                      │
                                      ▼
                              ┌───────────────┐
                              │     AMCL      │
                              │               │
                              │ Localization  │
                              └───────┬───────┘
                                      │
                                      ▼
                                   /map
```

</p>

### Navigation Data Flow

```text
Gazebo
  │
  ├── /scan ───────────────► LiDAR perception
  │
  ├── /odom ───────────────► Odometry
  │
  ▼
AMCL
  │
  └── Robot pose in map
          │
          ▼
        Nav2
          │
          ├── Global Planner
          │
          ├── Local Controller
          │
          └── Obstacle Avoidance
                    │
                    ▼
                 /cmd_vel
                    │
                    ▼
                 Gazebo
```

---

## 🧰 Technologies

| Component | Technology |
|:--|:--|
| Operating System | `Ubuntu` |
| Robotics Middleware | `ROS 2 Jazzy` |
| Simulation | `Gazebo` |
| Robot | `TurtleBot3 Burger` |
| Navigation | `Nav2` |
| Localization | `AMCL` |
| Visualization | `RViz2` |
| Range Sensor | `2D LiDAR` |
| Mapping | `SLAM` |
| Drive System | `Differential Drive` |

---

## 📡 ROS 2 Interfaces

### 🔴 LiDAR

**Topic**

`/scan`

**Message**

`sensor_msgs/msg/LaserScan`

The LiDAR provides range measurements used for environment perception and obstacle detection.

---

### 🔵 Odometry

**Topic**

`/odom`

**Message**

`nav_msgs/msg/Odometry`

Odometry provides the robot's estimated motion and velocity information.

---

### 🟢 Velocity Command

**Topic**

`/cmd_vel`

**Message**

`geometry_msgs/msg/TwistStamped`

Nav2 publishes velocity commands used to control the TurtleBot3.

---

### 🟡 AMCL Localization

**Topic**

`/amcl_pose`

**Message**

`geometry_msgs/msg/PoseWithCovarianceStamped`

AMCL publishes the estimated robot pose within the saved map.

---

### 🟣 Navigation

**Action**

`/navigate_to_pose`

This action interface is used by Nav2 to execute navigation goals.

---

## 🗺️ Mapping & Localization

A 2D occupancy-grid map was generated using SLAM and saved for later autonomous navigation.

### Saved Map

```text
maps/
├── turtlebot3_slam_map.pgm
└── turtlebot3_slam_map.yaml
```

### Localization Pipeline

```text
                    SLAM-Generated Map
                            │
                            ▼
                           /map
                            │
                            ▼
                          AMCL
                            │
                            ▼
                    Estimated Robot Pose
                            │
                            ▼
                       /amcl_pose
```

### TF Structure

```text
map
└── odom
    └── base_footprint
        └── base_link
            ├── base_scan
            ├── imu_link
            ├── wheel_left_link
            ├── wheel_right_link
            └── caster_back_link
```

---

## 🧭 Autonomous Navigation

The Nav2 stack handles the complete navigation process from receiving a goal to controlling the robot.

### Navigation Pipeline

```text
                    User
                     │
                     │ 2D Goal Pose
                     ▼
                   RViz2
                     │
                     ▼
              BT Navigator
                     │
             ┌───────┴────────┐
             ▼                ▼
        Planner Server   Controller Server
             │                │
             ▼                ▼
        Global Path      Local Control
             │                │
             └───────┬────────┘
                     │
                     ▼
                  /cmd_vel
                     │
                     ▼
                   Gazebo
                     │
                     ▼
               TurtleBot3
```

The robot was tested using multiple navigation goals within the simulated environment.

---

## 🛡️ Obstacle Avoidance

Obstacle avoidance is achieved using LiDAR observations together with the Nav2 planning and control pipeline.

```text
                 ┌─────────────────┐
                 │     Gazebo      │
                 │  Environment    │
                 └────────┬────────┘
                          │
                          ▼
                       /scan
                          │
                          ▼
                    ┌───────────┐
                    │   LiDAR   │
                    └─────┬─────┘
                          │
                          ▼
                    ┌───────────┐
                    │    Nav2   │
                    │           │
                    │ Planner   │
                    │    +      │
                    │Controller │
                    └─────┬─────┘
                          │
                       /cmd_vel
                          │
                          ▼
                    ┌───────────┐
                    │ TurtleBot3│
                    └───────────┘
```

The navigation stack continuously uses the available sensor information to plan and control the robot while accounting for obstacles in the environment.

---

## ⚙️ Nav2 Configuration

The TurtleBot3 Burger navigation configuration used during testing is stored in:

```text
config/
└── burger.yaml
```

The configuration contains parameters for the major Nav2 components used in the navigation pipeline.

### Main Components

```text
AMCL
Controller Server
Planner Server
Route Server
Behavior Server
Velocity Smoother
Collision Monitor
BT Navigator
Waypoint Follower
```

### Planner

```text
nav2_navfn_planner::NavfnPlanner
```

### AMCL Motion Model

```text
nav2_amcl::DifferentialMotionModel
```

---

## 🖥️ RViz2 Configuration

The verified RViz2 configuration is stored in:

```text
rviz/
└── navigation_baseline.rviz
```

RViz2 is used to visualize:

- Occupancy-grid map
- Robot model
- TF frames
- LiDAR data
- AMCL localization
- Navigation paths
- Navigation goals

### Fixed Frame

```text
map
```

### Navigation Tools

```text
2D Pose Estimate
2D Goal Pose
```

---

## 🚀 Running the Simulation

### 1. Set the TurtleBot3 Model

```bash
export TURTLEBOT3_MODEL=burger
```

### 2. Launch Gazebo

```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

### 3. Launch Nav2

Use the saved map and Burger configuration:

```bash
ros2 launch nav2_bringup bringup_launch.py \
  map:=/path/to/turtlebot3_slam_map.yaml \
  use_sim_time:=True \
  params_file:=/path/to/burger.yaml
```

Replace `/path/to/` with the actual location of the files on your system.

### 4. Launch RViz2

```bash
rviz2 -d /path/to/navigation_baseline.rviz
```

### 5. Set the Initial Pose

In RViz2:

```text
2D Pose Estimate
```

Click and drag on the map to provide the robot's approximate initial position and orientation.

### 6. Send a Navigation Goal

In RViz2:

```text
2D Goal Pose
```

Click and drag on the map to specify the desired goal position and orientation.

Nav2 will then calculate and execute a navigation path.

---

## 🔍 Verified ROS 2 Interfaces

### Topics

```text
/clock
/scan
/odom
/cmd_vel
/map
/amcl_pose
```

### Navigation Action

```text
/navigate_to_pose
```

### Message Types

```text
/scan
└── sensor_msgs/msg/LaserScan

/odom
└── nav_msgs/msg/Odometry

/cmd_vel
└── geometry_msgs/msg/TwistStamped

/amcl_pose
└── geometry_msgs/msg/PoseWithCovarianceStamped
```

---

## 🔄 Complete Navigation Workflow

```text
┌──────────────────────────────┐
│           Gazebo             │
│                              │
│    TurtleBot3 + Obstacles    │
└──────────────┬───────────────┘
               │
        ┌──────┴──────┐
        │             │
        ▼             ▼
      /scan         /odom
        │             │
        ▼             ▼
      LiDAR        Odometry
        │             │
        └──────┬──────┘
               │
               ▼
             AMCL
               │
               ▼
         Robot Pose
               │
               ▼
             Nav2
               │
       ┌───────┴────────┐
       │                │
       ▼                ▼
    Planner         Controller
       │                │
       └───────┬────────┘
               │
               ▼
            /cmd_vel
               │
               ▼
             Gazebo
               │
               ▼
        Robot Movement
```

---

## 🧪 Verification & Testing

The following components were successfully tested during development:

| Test | Status |
|:--|:--:|
| TurtleBot3 Burger simulation | ✅ Passed |
| Gazebo simulation world | ✅ Passed |
| LiDAR `/scan` | ✅ Passed |
| Odometry `/odom` | ✅ Passed |
| TF tree | ✅ Passed |
| SLAM map generation | ✅ Passed |
| Saved occupancy-grid map | ✅ Passed |
| AMCL localization | ✅ Passed |
| Nav2 configuration | ✅ Passed |
| RViz2 visualization | ✅ Passed |
| Initial pose estimation | ✅ Passed |
| Navigation goals | ✅ Passed |
| Autonomous navigation | ✅ Passed |
| Obstacle avoidance | ✅ Passed |
| Multiple navigation goals | ✅ Passed |

---

## 📁 Repository Structure

```text
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
```

---

## 🔮 Future Work

Potential extensions include:

```text
Classical Nav2 Navigation
            │
            ▼
      Baseline System
            │
            ▼
   Reinforcement Learning
            │
      ┌─────┴─────┐
      ▼           ▼
     PPO          SAC
      │           │
      └─────┬─────┘
            ▼
     Learned Policy
            │
            ▼
    Obstacle Avoidance
            │
            ▼
     Sim-to-Real Testing
```

Possible future extensions:

- Reinforcement-learning-based obstacle avoidance
- Custom Gymnasium environment
- PPO / SAC navigation policies
- Domain randomization
- Sim-to-real transfer
- Comparison between classical Nav2 and learned navigation policies

The current repository represents the **verified classical Nav2 navigation baseline**.

---

## 📊 Project Status

| Component | Status |
|:--|:--:|
| Gazebo Simulation | ✅ Working |
| TurtleBot3 Burger | ✅ Working |
| LiDAR | ✅ Working |
| Odometry | ✅ Working |
| SLAM Map | ✅ Working |
| AMCL Localization | ✅ Working |
| Nav2 | ✅ Working |
| RViz2 | ✅ Working |
| Autonomous Navigation | ✅ Working |
| Obstacle Avoidance | ✅ Working |

### Current Status

> 🟢 **Working simulation with autonomous navigation and obstacle avoidance using ROS 2 Nav2.**

---

## 👤 Author

### Ra

**Robotics and AI Engineering**

---

## ⭐ Project Summary

```text
Gazebo
   │
   ▼
TurtleBot3 Burger
   │
   ├── LiDAR
   └── Odometry
   │
   ▼
SLAM-Generated Map
   │
   ▼
AMCL Localization
   │
   ▼
Nav2
   │
   ├── Global Planning
   ├── Local Control
   └── Obstacle Avoidance
   │
   ▼
Autonomous Navigation
   │
   ▼
RViz2 Visualization
```

**ROS 2 Jazzy + Gazebo + Nav2 + AMCL + LiDAR + RViz2**

A complete simulation pipeline for autonomous mobile robot navigation.
