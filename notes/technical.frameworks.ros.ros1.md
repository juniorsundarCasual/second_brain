---
id: 5asykjrlp4z3pzxtg0xtc0z
title: ROS 1
desc: ''
updated: 1704970777261
created: 1704965684638
tags:
    - software
    - robotics
---

# Installation (Noetic Ninjemys)

NOTE: This is being attempted in the Ubuntu 20.04 operating system.

## Set up `sources.list`

Prepare system to accept software from packages.ros.org.

```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

## Set up keys

```bash
sudo apt install curl # if you haven't already installed curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

## Install ROS 1 Packages

Update repository and existing packages first.

```bash
sudo apt update
sudo apt upgrade
```

For the Desktop Install (Recommended) which includes everything in Desktop plus 2D/3D simulators and 2D/3D perception packages .

```bash
sudo apt install ros-noetic-desktop-full
```
> For on the ROS-Base Install (Bare Bones) which contains Communication libraries, message packages, command line tools. No GUI tools.
> 
> ```bash
> sudo apt install ros-noetic-ros-base
> ```

## Sourcing the Environment

Referring to the previous section on ROS packages.

![[technical.frameworks.ros#packages]]

We can permanently source the ROS installation by running:

```bash
echo "source /opt/ros/noetic/setup.bash" >> .bashrc
```

## Dependencies

To create and manage new packages and workspaces, certain tools are required that are distributed separately. These are now going to be installed.

```bash
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
```

To handle package dependencies and install them as needed, the `rosdep` tool also needs to be initialised.

```bash
sudo apt install python3-rosdep
sudo rosdep init
rosdep update
```

# Centralised System

the communication between nodes (the basic units of software in ROS that process data) is primarily facilitated through a combination of TCPROS and UDPROS protocols, along with the ROS Master.

- **ROS Master** - Provides name registration and lookup to the rest of the ROS network. When a node starts, it registers with the ROS Master, providing information about the publishers and subscribers it has. This enables nodes to find each other and establish direct connections for communication.
- **TCPROS** - This is the main transport protocol used in ROS 1 for node-to-node communication. TCPROS is used for sending and receiving messages, service calls, and actions over TCP/IP networks. It's reliable and ensures that data is received as sent, making it suitable for most of ROS's communication needs.
- **UDPROS** - This protocol is used for transmitting ROS messages over UDP. It's an optional protocol that can be used in situations where lower latency is required and loss of messages is acceptable. UDPROS is less commonly used compared to TCPROS due to its unreliability in message delivery.

These protocols operate in a peer-to-peer fashion, meaning that once nodes are aware of each other (through the ROS Master), they communicate directly without routing messages through the Master. This setup allows for flexible and scalable communication between nodes in a ROS network.