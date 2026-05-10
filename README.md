# Autonomous Navigation Robot (ROS 2 jazzy & Nav2)

This repository contains a full implementation of the **Nav2 (Navigation 2) Stack** for a differential drive robot simulated in **Gazebo Sim**. The project demonstrates a complete autonomous workflow, from URDF modeling and sensor integration to global/local path planning and lifecycle management.

---

## 📂 Project Structure

```text
Autonomous_Mobile_Robot/
├── bot_bringup/                 # Core navigation & launch logic
│   ├── config/                  # nav2_params.yaml, gazebo_bridge.yaml
│   ├── launch/                  # Modular xml launch files
│   ├── behavior_trees/          # Custom Behavior Tree XML files
│   └── maps/                    # Environment maps (.yaml & .pgm)
├── follower_robot_description/  # Robot model and visualization
│   ├── urdf/                    # Xacro files with fixed TF origins                
├── .gitignore                   # Workspace cleaning rules
└── README.md                    # Project documentation
```

🛠 Engineering Challenges & Debugging

This project involved significant troubleshooting to achieve a stable navigation state. Below are the key engineering hurdles resolved during development:
1. The Clock Synchronization Mismatch

    Problem: Laser scan messages were being dropped because the sensor data used simulation time while the system used wall time, resulting in a TF extrapolation error (timestamp at 0.0).

    Solution: Implemented a /clock bridge between Gazebo and ROS 2 and enforced use_sim_time: True globally across all lifecycle nodes.

2. Lifecycle State Management

    Problem: The controller_server and bt_navigator were getting stuck in the Inactive state, preventing action servers from responding to 2D Nav Goals.

    Solution: Integrated the nav2_lifecycle_manager to orchestrate node transitions. Verified the health of the stack using ros2 lifecycle list to ensure all servers reached the Active state.

3. YAML Namespacing & Plugin Loading

    Problem: The Controller Server failed to load the FollowPath plugin due to incorrect YAML nesting and missing "critics" definitions for the DWB planner.

    Solution: Flattened the YAML hierarchy to match exact node names and defined a comprehensive list of critics (GoalAlign, PathAlign, ObstacleFootprint) to allow the planner to initialize.

```
Prerequisites

    OS: Ubuntu 24.04

    ROS 2 jazzy

    Nav2 Binaries: sudo apt install ros-jazzy-navigation2 ros-jazzy-nav2-bringup

    Gazebo Sim
```
