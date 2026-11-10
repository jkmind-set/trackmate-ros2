# TrackMate 🤖

> A tracked robot that maps your home, sees what's around it, and understands what you ask.

![ROS 2 Jazzy](https://img.shields.io/badge/ROS_2-Jazzy-22314E?logo=ros)
![Ubuntu 24.04](https://img.shields.io/badge/Ubuntu-24.04-E95420?logo=ubuntu&logoColor=white)
![Platform](https://img.shields.io/badge/chassis-MentorPi_tracked-blue)
![License](https://img.shields.io/badge/license-MIT-green)

TrackMate is a personal assistant robot built on the **MentorPi suspension
tracked chassis**. It combines **SLAM** for navigation, **vision AI** for
perception, and a **large language model** for natural-language commands, all
running on **ROS 2 Jazzy**.

## Features

- 🗺️ **SLAM and navigation:** builds a map and navigates autonomously with
  slam_toolbox and Nav2
- 👁️ **Vision AI:** object detection and scene understanding
- 💬 **LLM brain:** turns requests like “bring me the cup” into robot actions
- 🛞 **Tracked suspension chassis:** handles rugs, thresholds, and rough floors
- 🔌 **Modular ROS 2 nodes:** each capability can be swapped or extended

## Architecture

```text
Voice/Text → LLM planner → Task manager → Nav2 / Vision / Motion → MentorPi chassis
```

## Hardware

| Component | Hardware |
| --- | --- |
| Chassis | Hiwonder MentorPi T1 anodized aluminum tracked tank (270 × 195 × 182 mm, approximately 1.88 kg) |
| Compute | Raspberry Pi 5 (arm64; 4 GB kit upgraded with a 16 GB board) |
| Sub-controller | RRC Lite (STM32 for closed-loop PID motor control, IMU, and power monitoring) |
| Sensors | Oradar MS200 360° ToF LiDAR, Aurora930 Pro 3D Depth Camera, onboard IMU |
| Audio AI | WonderEcho Pro AI Voice Interaction Box (CI1302 speech processor, microphone array, and speaker) |
| Actuators | DC geared motors with AB-phase Hall-effect quadrature encoders |
| Power | 7.4 V (2S) 2200 mAh 10C LiPo battery with 8.4 V 2 A wall charger |
| Storage | 64 GB pre-flashed microSD card |

## Software

- ROS 2 Jazzy
- Ubuntu 24.04 (Noble Numbat)
- Cyclone DDS

## Quick start

```bash
git clone https://github.com/jkmind-set/trackmate-ros2.git
cd trackmate-ros2
colcon build --symlink-install
source install/setup.bash
ros2 launch trackmate_bringup trackmate.launch.py
```

## Roadmap

- [x] ROS 2 Jazzy environment (Cyclone DDS)
- [ ] Chassis bringup and teleop
- [ ] SLAM mapping
- [ ] Nav2 autonomous navigation
- [ ] Vision node (detection)
- [ ] LLM task planner
- [ ] Voice interface
