---
id: pmllbd94b3isbiyff07y08r
title: ROS 2
desc: ''
updated: 1704969112022
created: 1704965694417
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

## Setup Sources

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