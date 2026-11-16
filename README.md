# TrackMate 🤖

> A tracked robot project built to map its surroundings, recognize what's around it, and respond to natural-language requests.

![ROS 2 Humble in Docker](https://img.shields.io/badge/ROS_2-Humble_in_Docker-22314E?logo=ros)
![Platform](https://img.shields.io/badge/chassis-MentorPi_tracked-blue)
![License](https://img.shields.io/badge/license-GPL--3.0-blue)

TrackMate is built around the **Hiwonder MentorPi (Suspended Tracked Chassis / Depth Camera Edition)** running **ROS 2 Humble** inside the `MentorPi` Docker container on a Raspberry Pi 5. This repository contains the ROS 2 Humble workspace (`/home/ubuntu/ros2_ws`), comprising 17 ROS 2 packages for hardware drivers, sensor peripherals, 2D/3D SLAM, Nav2 navigation, computer vision, and LLM/VLLM task planning.

## Project goals

- 🗺️ **SLAM and navigation:** 2D (`slam_toolbox`) and 3D (`rtabmap_ros`) mapping and Nav2 autonomous navigation
- 👁️ **Vision:** object detection (YOLOv5, MediaPipe, OpenCV) and depth scene understanding
- 💬 **Task planning:** natural-language and voice requests translated into robot actions via LLM/VLLM agents
- 🛞 **Tracked mobility:** closed-loop differential/tracked drive over indoor terrain using the suspended chassis
- 🔌 **Modular ROS 2 nodes:** independently extensible capabilities across 17 workspace packages

The system flow is:

```text
Voice/Text → LLM planner → Task manager → Nav2 / Vision / Motion → MentorPi chassis
```

## Confirmed hardware

**Product:** Hiwonder MentorPi (Suspended Tracked Chassis / Depth Camera Edition).

| Component | Hardware |
| --- | --- |
| Primary host | Raspberry Pi 5 motherboard, arm64; 4 GB or 16 GB configuration |
| Robot controller | RRCLite STM32 controller for real-time PID motor drive, low-level sensing, and power management |
| Chassis | Suspended tracked chassis with spring/damping suspension; all-metal anodized aluminum; 278 × 195 × 182 mm (L × W × H); 1.88 kg net |
| Motors and encoders | Hall-coded DC geared motors with AB-phase incremental Hall-effect encoders for closed-loop wheel odometry |
| Depth camera | Aurora 930 Pro RGB/IR/depth camera (`DEPTH_CAMERA_TYPE=aurora`) for point clouds and VSLAM |
| LiDAR | Oradar MS200 360° ToF LiDAR (`LIDAR_TYPE=MS200`) for 2D SLAM and Nav2 obstacle clearance |
| Battery | 7.4 V 2S, 2200 mAh, 10C battery with built-in protective plate/BMS; approximately 60 minutes rated active runtime |
| Storage | 64 GB TF/microSD card |
| Networking | Dual-band Wi-Fi and Ethernet |
| Control interfaces | WonderROS iOS/Android app, gamepad/handle, and PC remote desktop, VNC, RViz2, or SSH |
| Product software | Raspberry Pi OS or Ubuntu 22.04 LTS host running the `MentorPi` ROS 2 Humble Docker container |
| Programming languages | Python and C/C++ |
| Package weight and dimensions | Approximately 3.2 kg; 397 × 244 × 220 mm (L × W × H) |

The LFD-01 anti-jam servo is for the monocular pan-tilt tier and is **not included** with this fixed-bracket Aurora 930 Pro model.

## Workspace Architecture (`src/`)

This repository is the `/home/ubuntu/ros2_ws` workspace containing **17 ROS 2 packages**:

| Package | Path | Description |
| --- | --- | --- |
| `bringup` | `src/bringup` | System startup launch files, hardware bringup (`bringup.launch.py`), and auto-start scripts |
| `peripherals` | `src/peripherals` | Launch files and nodes for LiDAR (`MS200`), Depth Camera (`aurora`), IMU, joystick, and keyboard teleop |
| `controller` | `src/driver/controller` | Chassis kinematics, odometry publisher (`odom_publisher_node`), EKF localization (`ekf.yaml`), and `rf2o` laser odometry |
| `ros_robot_controller` | `src/driver/ros_robot_controller` | Serial driver interface to the RRCLite STM32 controller board (motors, encoders, IMU, buzzer, LED, battery) |
| `ros_robot_controller_msgs` | `src/driver/ros_robot_controller_msgs` | Custom ROS 2 messages and services for the RRCLite STM32 board |
| `sdk` | `src/driver/sdk` | Shared Python utilities (PID, common math, colour spaces, FPS counters) |
| `mentorpi_description` | `src/simulations/mentorpi_description` | Robot URDF/Xacro models (`mentorpi.xacro`, `tank.xacro`, `depth_cam.xacro`), STL meshes, and RViz configs |
| `slam` | `src/slam` | 2D SLAM (`slam.launch.py`) and 3D RTAB-Map VSLAM (`rtabmap_slam.launch.py`) plus saved occupancy grid maps (`maps/map_01.*`) |
| `navigation` | `src/navigation` | Nav2 2D navigation (`navigation.launch.py`) and 3D RTAB-Map navigation (`rtabmap_navigation.launch.py`) |
| `app` | `src/app` | High-level application service nodes: LiDAR obstacle avoidance/following/guard, line following, object tracking, hand gesture, and AR |
| `example` | `src/example` | Standalone vision & autonomy demos (OpenCV, MediaPipe, QRcode, YOLOv5 demos, self-driving, color/body tracking) |
| `yolov5_ros2` | `src/yolov5_ros2` | ROS 2 YOLOv5 detector node and pretrained weights (`yolov5s.pt`, traffic signs, garbage classification) |
| `large_models` | `src/large_models` | LLM/VLLM agent nodes (movement control, visual patrol, color/object tracking, camera Q&A, navigation, ASR/TTS) |
| `large_models_msgs` | `src/large_models_msgs` | Custom ROS 2 messages and services for LLM/VLLM agent pipelines |
| `interfaces` | `src/interfaces` | Core workspace custom messages and services (`Pose2D`, `ObjectsInfo`, `SetInt64`, `SetColorRGBA`, etc.) |
| `calibration` | `src/calibration` | Linear and angular odometry/IMU calibration nodes and configs |
| `multi` | `src/multi` | Multi-robot formation and synchronized TF/costmap launch configurations |

> **Note on `wheel_link.stl`:** The unused 101 MB raw CAD file `src/simulations/mentorpi_description/meshes/tank/wheel_link.stl` is excluded via `.gitignore` to stay within GitHub's 100 MB file limit. It is not referenced by `tank.xacro`.

## Environment & Build Setup

On the MentorPi Raspberry Pi 5 host, ROS 2 Humble runs inside the **`MentorPi`** Docker container under `/home/ubuntu/ros2_ws`.

1. **Environment configuration (`.typerc` & `.robotrc`):**
   When opening a shell inside the container, `.zshrc` sources `.robotrc` and `.typerc`, which configure:
   - `MACHINE_TYPE=MentorPi_Tank`
   - `LIDAR_TYPE=MS200`
   - `DEPTH_CAMERA_TYPE=aurora`
   - ROS 2 Humble (`/opt/ros/humble/setup.sh`), prebuilt vendor SDKs (`/home/ubuntu/third_party_ros2/third_party_ws/install/local_setup.*`), and this workspace (`/home/ubuntu/ros2_ws/install/local_setup.*`).

2. **Build workspace with `colcon`:**
   Inside the `MentorPi` container (`/home/ubuntu/ros2_ws`):
   ```bash
   source /opt/ros/humble/setup.zsh
   colcon build --symlink-install
   source install/local_setup.zsh
   ```
   To build a single package after editing:
   ```bash
   colcon build --symlink-install --packages-select <package_name>
   ```

## Quick Start & Launch Commands

Before launching individual demos on the robot, stop the background auto-start bringup service inside the container if it is running:
```bash
~/.stop_ros.sh
```

### Peripherals & Teleop
```bash
# Keyboard teleoperation
ros2 launch peripherals teleop_key_control.launch.py

# Aurora 930 Pro depth camera
ros2 launch peripherals depth_camera.launch.py

# Oradar MS200 LiDAR viewer
ros2 launch peripherals lidar_view.launch.py

# IMU viewer
ros2 launch peripherals imu_view.launch.py
```

### Built-in Application Services (`app`)
With `bringup` or `start_app.launch.py` running:
```bash
# LiDAR App (1 = Obstacle Avoidance, 2 = Following, 3 = Guard)
ros2 service call /lidar_app/enter std_srvs/srv/Trigger {}
ros2 service call /lidar_app/set_running interfaces/srv/SetInt64 "{data: 1}"

# Line Following
ros2 service call /line_following/enter std_srvs/srv/Trigger {}
ros2 service call /line_following/set_running std_srvs/srv/SetBool "{data: True}"

# Object Tracking
ros2 service call /object_tracking/enter std_srvs/srv/Trigger {}
ros2 service call /object_tracking/set_running std_srvs/srv/SetBool "{data: True}"
```

### SLAM & Autonomous Navigation
```bash
# 2D SLAM mapping
ros2 launch slam slam.launch.py

# Save 2D map to src/slam/maps/map_01
cd ~/ros2_ws/src/slam/maps && ros2 run nav2_map_server map_saver_cli -f "map_01" --ros-args -p map_subscribe_transient_local:=true

# 3D RTAB-Map VSLAM
ros2 launch slam rtabmap_slam.launch.py

# 2D Nav2 Navigation (using saved map_01)
ros2 launch navigation navigation.launch.py map:=map_01

# 3D RTAB-Map Navigation
ros2 launch navigation rtabmap_navigation.launch.py
```

### LLM / VLLM Demos (`large_models`)
Configure your API keys in `src/large_models/large_models/config.py`, then run:
```bash
ros2 launch large_models llm_control_move.launch.py
ros2 launch large_models llm_color_track.launch.py
ros2 launch large_models llm_visual_patrol.launch.py
ros2 launch large_models vllm_with_camera.launch.py
ros2 launch large_models vllm_track.launch.py
ros2 launch large_models vllm_navigation.launch.py map:=map_01
```

## Roadmap

- [x] Establish repository packages for the confirmed ROS 2 Humble platform
- [x] Chassis bringup and teleop (`bringup`, `controller`, `peripherals`)
- [x] SLAM mapping (`slam` — 2D & 3D RTAB-Map)
- [x] Nav2 autonomous navigation (`navigation`)
- [x] Vision nodes (`yolov5_ros2`, `app`, `example`)
- [x] LLM / VLLM task planner baseline (`large_models`)
- [ ] Custom TrackMate end-to-end autonomy & voice task manager integration
