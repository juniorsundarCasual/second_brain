---
id: pmllbd94b3isbiyff07y08r
title: ROS 2
desc: ''
updated: 1704970476960
created: 1704965694417
tags:
    - software
    - robotics
---

# Installation (Humble Hawksbill)

NOTE: This is being attempted in the Ubuntu 22.04 operating system.

## Set Locale

Make sure there is a locale which supports UTF-8. If in a minimal environment (such as a docker container), the locale may be something minimal like POSIX. Test with the following settings.

```bash
locale  # check for UTF-8

sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

locale  # verify settings
```

## Set up Sources

Add ROS 2 `apt` repository to the system. First ensure that Ubuntu Universe repository is enabled.

```bash
sudo apt install software-properties-common
sudo add-apt-repository universe
```

Add ROS 2 GPG keys.

```bash
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg

```

Add the repository to sources list.

```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```

## Install ROS 2 Packages

Update repository and existing packages first.

```bash
sudo apt update
sudo apt upgrade
```

For the Desktop Install (Recommended) which includes ROS, RViz, demos, tutorials.

```bash
sudo apt install ros-humble-desktop
sudo apt install ros-dev-tools
```
> For on the ROS-Base Install (Bare Bones) which contains Communication libraries, message packages, command line tools. No GUI tools.
> 
> ```bash
> sudo apt install ros-humble-ros-base
> sudo apt install ros-dev-tools
> ```

## Sourcing the Environment

Referring to the previous section on ROS packages.

![[technical.frameworks.ros#packages]]

We can permanently source the ROS installation by running:

```bash
echo "source /opt/ros/humble/setup.bash" >> .bashrc
```

# Distributed System

DDS (Data Distribution Service) plays a crucial role in ROS (Robot Operating System), especially in ROS 2. It acts as the underlying connectivity framework that facilitates communication between different parts of a ROS-based system.

1. **Middleware Layer** - DDS is used as the default middleware. This means that DDS is responsible for the efficient and reliable exchange of information between different nodes (components) of a ROS 2 system. It's a layer that sits between the ROS application layer and the network, managing data exchange and communication.
2. **Vendor Agnosticism** - Is designed to be DDS vendor-agnostic. This means it can work with different DDS implementations, such as eProsima's Fast DDS, RTI’s Connext DDS, Eclipse Cyclone DDS, and GurumNetworks GurumDDS. This flexibility allows users to choose a DDS implementation that best fits their specific requirements.
3. **Key Functions Provided by DDS** - DDS provides several essential services in ROS 2, including discovery (finding and connecting to other nodes in the network), message serialization (converting data to a format suitable for transmission), and publish-subscribe transport (managing how messages are sent and received).
4. **Enhanced Features** - Brings enhanced features compared to ROS 1, such as distributed discovery (eliminating the need for a central master for node discovery), more granular quality of service settings, and potentially more efficient intra-process communication through shared memory.