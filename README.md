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

**Product:** Raspberry Pi 5 smart car, MentorPi suspension tracked chassis,
SLAM vision AI large model, ROS robot

| Component | Hardware |
| --- | --- |
| Product model | MentorPi (with depth camera) |
| Chassis | Suspended tracked chassis; all-metal aluminum alloy with anodized finish (278 × 195 × 182 mm, L × W × H); approximately 1.88 kg |
| Compute / ROS master | RRCLite robot controller with Raspberry Pi 5 motherboard |
| Motors and encoders | Hall-coded DC geared motors with AB-phase incremental Hall encoders |
| Sensors | Orida MS200 LiDAR, Aurora 930 Pro depth camera, onboard IMU |
| Audio AI | WonderEcho Pro AI Voice Interaction Box (CI1302 speech processor, microphone array, and speaker) |
| Servo | LFD-01 anti-jam servo (monocular camera version) |
| Control | Mobile app, handheld controller, or PC |
| Power | 7.4 V 2200 mAh 10C lithium battery with protection board; approximately 60 minutes of runtime (varies by use) |
| Communication | Wi-Fi / Ethernet |
| Storage | 64 GB TF card |
| Programming | Python, C/C++, and JavaScript |
| Product software | Raspberry Pi OS / Ubuntu 22.04 LTS with ROS 2 Humble in Docker; iOS / Android mobile app |
| Package contents | AI Pi5 4G or 16G Depth: robot car with Raspberry Pi 5 (4 GB or 16 GB), SD card, and AI |
| Package size and weight | Approximately 3.2 kg; 397 × 244 × 220 mm (L × W × H) |
| Supporting materials | Video tutorials, ROS source code, system images, and supporting software |

## Software

The product configuration is listed above. This repository's development environment
uses:

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
