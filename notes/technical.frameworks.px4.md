---
id: u5wyzvvhy0gyli06ndqwlld
title: PX4 - Autopilot
desc: ''
updated: 1705997411882
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

## [[technical.frameworks.px4.uorb]]

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

It is a lightweight messaging protocol that is designed to be used in the drone ecosystem. It is designed for efficiently sending messages over unreliable low-bandwidth radio links.

PX4 uses MAVLink to communicate with ground stations and MAVLink SDKs, such as [[technical.frameworks.px4#qgroundcontrol]] and [[technical.libraries.mavsdk]]

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

### [[technical.frameworks.px4.simulation]] (SITL)

Simulation is a quick, easy, and most importantly, safe way to test changes to PX4 code before attempting to fly in the real world. It is also a good way to start flying with PX4 when you haven't yet got a vehicle to experiment with.

PX4 supports both Software In the Loop (SITL) simulation, where the flight stack runs on computer (either the same computer or another computer on the same network) and Hardware In the Loop (HITL) simulation using a simulation firmware on a real flight controller board. HITL is only community supported.

### [[ROS | technical.frameworks.px4.ros2]]

Limited only to ROS 2 since ROS 1 is slowly reaching deprecation. Through the [[μXRCE-DDS | technical.frameworks.px4#μxrce-dds-px4-ros-2dds-bridge]] client-agent, it is possible to expose the uORB messages in the flight controller to the ROS 2 framework. This facilitates forward and backwards communication.

## Fault Injection

It is possible to inject faults into the flight controller. This methodology is observed in [[literature review | ideas.srta-exploration.literature-review#fault-injector-for-autonomous-quadrotors]]. The strategy can be applied by introducing faults into the source code itself.

### Example: IMU

To introduce a dynamic way to inject and remove faults from this module, we can take advantage of the parameter server in the FC (by launching custom parameters).


```cpp
/**
* Navigate to ./PX4-Autopilot/src/modules/sensors/vehicle_imu/VehicleIMU.cpp
* Add the following snippet of code before the _vehicle_imu_pub.publish(imu) call
*
**/
// Adding faults to the accelerometer
param_t accel_fault = param_find("SENS_ACCEL_FAULT");
int32_t accel_fault_flag;
param_get(accel_fault, &accel_fault_flag);

if (accel_fault_flag == 1)
{
    param_t accel_noise = param_find("SENS_ACCEL_NOISE");
    float_t accel_noise_flag;
    param_get(accel_noise, &accel_noise_flag);

    if (abs(accel_noise_flag) > 0)
    {
        unsigned seed = std::chrono::system_clock::now().time_since_epoch().count();
        std::default_random_engine generator (seed);

        std::normal_distribution<float> distribution (0.0,accel_noise_flag);
        float dev = distribution(generator);
        imu.delta_velocity[0] += imu.delta_velocity[0]*dev;
        imu.delta_velocity[1] += imu.delta_velocity[1]*dev;
        imu.delta_velocity[2] += imu.delta_velocity[2]*dev;
    }

    param_t accel_bias_shift = param_find("SENS_ACCEL_SHIF");
    float_t accel_bias_shift_flag;
    param_get(accel_bias_shift, &accel_bias_shift_flag);

    if (abs(accel_bias_shift_flag) > 0)
    {
        imu.delta_velocity[0] += imu.delta_velocity[0]*accel_bias_shift_flag;
        imu.delta_velocity[1] += imu.delta_velocity[1]*accel_bias_shift_flag;
        imu.delta_velocity[2] += imu.delta_velocity[2]*accel_bias_shift_flag;
    }

    param_t accel_bias_scale = param_find("SENS_ACCEL_SCAL");
    float_t accel_bias_scale_flag;
    param_get(accel_bias_scale, &accel_bias_scale_flag);

    if (abs(accel_bias_scale_flag) > 0)
    {
        imu.delta_velocity[0] *= accel_bias_scale_flag;
        imu.delta_velocity[1] *= accel_bias_scale_flag;
        imu.delta_velocity[2] *= accel_bias_scale_flag;
    }

    param_t accel_drift = param_find("SENS_ACCEL_DRIFT");
    float_t accel_drift_flag;
    param_get(accel_drift, &accel_drift_flag);

    if (abs(accel_drift_flag) > 0)
    {
        //! Will have to initialise and declare 'accel_drift_timestep'
        imu.delta_velocity[0] += 0.01f*accel_drift_flag*accel_drift_timestep/1000000;
        imu.delta_velocity[1] += 0.01f*accel_drift_flag*accel_drift_timestep/1000000;
        imu.delta_velocity[2] += 0.01f*accel_drift_flag*accel_drift_timestep/1000000;

        accel_drift_timestep += 1;
    }

    param_t accel_zero = param_find("SENS_ACCEL_ZERO");
    int32_t accel_zero_flag;
    param_get(accel_zero, &accel_zero_flag);

    if (accel_zero_flag == 1)
    {
        imu.delta_velocity[0] = 0;
        imu.delta_velocity[1] = 0;
        imu.delta_velocity[2] = 0;
    }
}

// Adding faults to the gyroscope
param_t gyro_fault = param_find("SENS_GYRO_FAULT");
int32_t gyro_fault_flag;
param_get(gyro_fault, &gyro_fault_flag);

if (gyro_fault_flag == 1)
{
    param_t gyro_noise = param_find("SENS_GYRO_NOISE");
    float_t gyro_noise_flag;
    param_get(gyro_noise, &gyro_noise_flag);

    if (abs(gyro_noise_flag) > 0)
    {
        unsigned seed = std::chrono::system_clock::now().time_since_epoch().count();
        std::default_random_engine generator (seed);

        std::normal_distribution<float> distribution (0.0,gyro_noise_flag);
        float dev = distribution(generator);
        imu.delta_angle[0] += imu.delta_angle[0]*dev;
        imu.delta_angle[1] += imu.delta_angle[0]*dev;
        imu.delta_angle[2] += imu.delta_angle[0]*dev;
    }

    param_t gyro_bias_shift = param_find("SENS_GYRO_SHIF");
    float_t gyro_bias_shift_flag;
    param_get(gyro_bias_shift, &gyro_bias_shift_flag);

    if (abs(gyro_bias_shift_flag) > 0)
    {
        imu.delta_angle[0] += imu.delta_angle[0]*gyro_bias_shift_flag;
        imu.delta_angle[1] += imu.delta_angle[1]*gyro_bias_shift_flag;
        imu.delta_angle[2] += imu.delta_angle[2]*gyro_bias_shift_flag;
    }

    param_t gyro_bias_scale = param_find("SENS_GYRO_SCAL");
    float_t gyro_bias_scale_flag;
    param_get(gyro_bias_scale, &gyro_bias_scale_flag);

    if (abs(gyro_bias_scale_flag) > 0)
    {
        imu.delta_angle[0] *= gyro_bias_scale_flag;
        imu.delta_angle[1] *= gyro_bias_scale_flag;
        imu.delta_angle[2] *= gyro_bias_scale_flag;
    }

    param_t gyro_drift = param_find("SENS_GYRO_DRIFT");
    float_t gyro_drift_flag;
    param_get(gyro_drift, &gyro_drift_flag);

    if (abs(gyro_drift_flag) > 0)
    {
        //! Will have to initialise and declare 'gyro_drift_timestep'
        imu.delta_angle[0] += 0.01f*gyro_drift_flag*gyro_drift_timestep/1000000;
        imu.delta_angle[1] += 0.01f*gyro_drift_flag*gyro_drift_timestep/1000000;
        imu.delta_angle[2] += 0.01f*gyro_drift_flag*gyro_drift_timestep/1000000;

        gyro_drift_timestep += 1;
    }

    param_t gyro_zero = param_find("SENS_GYRO_ZERO");
    int32_t gyro_zero_flag;
    param_get(gyro_zero, &gyro_zero_flag);

    if (gyro_zero_flag == 1)
    {
        imu.delta_angle[0] = 10000;
        imu.delta_angle[1] = 10000;
        imu.delta_angle[2] = 10000;
    }
}
```