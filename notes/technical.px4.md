---
id: u5wyzvvhy0gyli06ndqwlld
title: PX4 - Autopilot
desc: ''
updated: 1704873411634
created: 1704869894839
tags:
    - software
    - drone
---

# Introduction

It is a professional autopilot that can be used to power all kinds of vehicles, ranging from drones to ground vehicle and submarines.

_Access the documentation [here](https://docs.px4.io/main/en/)._

# Flight Controller

Brains of the unmanned vehicle. PX4 can run on many flight controller boards. Selection of board must suit physical and sensor constraints of the vehicle as well as the type of function the vehicle has.

## Pixhawk Series

These are open-hardware flight controllers that run PX4 on the NuttX OS.

_Access their website [here](https://pixhawk.org/)._

# Middleware

## [[technical.px4.uorb]]

It is an asynchronous publish() / subscribe() messaging API used for inter-thread/inter-process communication. It is automatically started early on bootup.

In the controller, the desired data is being logged by ULog (PX4's flight logging system). This data is packaged into messages that are transmitted through uORB topics.

_Read advanced notes on this [here](https://docs.px4.io/main/en/middleware/uorb.html)._

## μXRCE-DDS (PX4-ROS 2/DDS Bridge)

This is the middleware that allows the uORB messages to be published and subscribed on the companion computer as though they were ROS 2 topics.

The architecture is defined in the below image:
![](https://docs.px4.io/main/assets/img/architecture_xrce-dds_ros2.fed61809.svg)

The client runs on the flight controller, while the agent is on the companion processor. The client reveals the uORB topics active the flight controller to the agent, and the agent decides which of these are to exposed as ROS 2 topics that can then be accessed through nodes running on the local system.

_Read advanced notes on this [here](https://docs.px4.io/main/en/middleware/uxrce_dds.html)._

## MAVLink

It is a lightweight messaging protocol that is designed to be used in the drone ecosystem.

PX4 uses MAVLink to communicate with ground stations and MAVLink SDKs, such as [[technical.px4#qgroundcontrol]] and [[technical.libraries.mavsdk]]]

# Development

## Toolchain Installation (Ubuntu)

### Autopilot Installation

To install the PX4-Autopilot toolchain for the Ubuntu system, run the following.

```bash
# For Ubuntu
# Pull the source code
# This will automatically retrieve the most recent version of the autopilot
git clone https://github.com/PX4/PX4-Autopilot.git --recursive

# Running this will install everything
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh --no-nuttx

# Restart after installation
sudo reboot now
```

### μXRCE-DDS Agent Installation

The agent needs to be built on the companion processor, with the following steps.

```bash
# For Ubuntu
# This is the installation for the agent.
git clone https://github.com/eProsima/Micro-XRCE-DDS-Agent.git
cd Micro-XRCE-DDS-Agent
mkdir build
cd build
cmake ..
make
sudo make install
sudo ldconfig /usr/local/lib/
```

### QGroundControl

This software provides full flight control and mission planning for any MAVLink enabled drone.

To install this, do the following:

```bash
# For Ubuntu
sudo usermod -a -G dialout $USER
sudo apt-get remove modemmanager -y
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
sudo apt install libqt5gui5 -y
sudo apt install libfuse2 -y
```
Log out and log in to enable user changes.

Download the application from [here](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/getting_started/download_and_install.html).

Install using teminal commands.

```bash
# For Ubuntu
chmod +x ./QGroundControl.AppImage
./QGroundControl.AppImage  # or double click
```

### [[technical.px4.simulation]] (SITL)

### ROS