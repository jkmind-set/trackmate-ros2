# TrackMate 🤖

> A tracked robot project aiming to map its surroundings, recognize what's around it, and respond to natural-language requests.

![ROS 2 Humble in Docker](https://img.shields.io/badge/ROS_2-Humble_in_Docker-22314E?logo=ros)
![Platform](https://img.shields.io/badge/chassis-MentorPi_tracked-blue)
![License](https://img.shields.io/badge/license-GPL--3.0-blue)

TrackMate is a project built around the **Hiwonder MentorPi (Suspended Tracked
Chassis / Depth Camera Edition)**. Autonomous mapping, vision-based perception,
and natural-language task planning are project goals; they are not yet
implemented in this repository.

## Project goals

- 🗺️ **SLAM and navigation:** mapping and autonomous navigation
- 👁️ **Vision:** object detection and scene understanding
- 💬 **Task planning:** natural-language requests translated into robot actions
- 🛞 **Tracked mobility:** use the suspended chassis for indoor traversal
- 🔌 **Modular ROS 2 nodes:** independently extensible capabilities

The intended system flow is:

```text
Voice/Text → LLM planner → Task manager → Nav2 / Vision / Motion → MentorPi chassis
```

## Confirmed hardware

**Product:** Hiwonder MentorPi (Suspended Tracked Chassis / Depth Camera
Edition).

| Component | Hardware |
| --- | --- |
| Primary host | Raspberry Pi 5 motherboard, arm64; 4 GB or 16 GB configuration |
| Robot controller | RRCLite STM32 controller for real-time PID motor drive, low-level sensing, and power management |
| Chassis | Suspended tracked chassis with spring/damping suspension; all-metal anodized aluminum; 278 × 195 × 182 mm (L × W × H); 1.88 kg net |
| Motors and encoders | Hall-coded DC geared motors with AB-phase incremental Hall-effect encoders for closed-loop wheel odometry |
| Depth camera | Aurora 930 Pro RGB/IR/depth camera for point clouds and VSLAM |
| LiDAR | Oradar MS200 360° ToF LiDAR for 2D SLAM and Nav2 obstacle clearance |
| Battery | 7.4 V 2S, 2200 mAh, 10C battery with built-in protective plate/BMS; approximately 60 minutes rated active runtime |
| Storage | 64 GB TF/microSD card |
| Networking | Dual-band Wi-Fi and Ethernet |
| Control interfaces | WonderROS iOS/Android app, gamepad/handle, and PC remote desktop, VNC, RViz2, or SSH |
| Product software | Raspberry Pi OS or Ubuntu 22.04 LTS; ROS 2 Humble running in Docker |
| Programming languages | Python, C/C++, and JavaScript |
| Package weight and dimensions | Approximately 3.2 kg; 397 × 244 × 220 mm (L × W × H) |

The LFD-01 anti-jam servo is for the monocular pan-tilt tier and is **not
included** with this fixed-bracket Aurora 930 Pro model.

## Software platform

The confirmed supplied platform is Raspberry Pi OS or Ubuntu 22.04 LTS with
ROS 2 Humble in Docker. This describes the product configuration; this
repository does not currently define a separate development environment.

## Quick start

This repository currently contains only this README and the project license;
it has no ROS 2 packages, build files, or launch files. A functional
clone/build/launch quick start is not available yet.

## Roadmap

- [ ] Establish repository packages for the confirmed ROS 2 Humble platform
- [ ] Chassis bringup and teleop
- [ ] SLAM mapping
- [ ] Nav2 autonomous navigation
- [ ] Vision node (detection)
- [ ] LLM task planner
- [ ] Voice interface
