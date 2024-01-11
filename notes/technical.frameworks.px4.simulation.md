---
id: 8slw67n6b4mks3k0egpozjp
title: Simulation
desc: ''
updated: 1704954945266
created: 1704872877142
---

# Introduction

Simulators allow PX4 flight code to control a computer modeled vehicle in a simulated environment, with the same level of software interactability as the real drone. PX4 supports both [[technical.terminologies.software-in-the-loop]] (SITL) and [[technical.terminologies.hardware-in-the-loop]] (HITL).

There are few supported simulators by the PX4 flight controller, these are listed below with their advantages and disadvantages.

# Simulators

Since the PX4 is an important component in the system's design, it is used as the starting point for this search. While simulation can take place with a wide variety of softwares and physics environments, it is best to start from a point where PX4 is supported out-of-the-box. PX4's dev [documentation](https://dev.px4.io/v1.11_noredirect/en/simulation/) lists the following simulation options:

1. Gazebo
2. jMAVSim
3. FlightGear
4. Microsoft AirSim

## [Gazebo](https://dev.px4.io/v1.11_noredirect/en/simulation/gazebo.html)

| Advantages | Disadvantages |
|------------|---------------|
| Wide variety of sensor support through plugins. Useful when use cases extend into sensor data streams. | Preferred if ROS is being used. Otherwise, there are cheaper (resource use wise) options. |
| Able to simulate multiple vehicles at the same time. More scalable than the other options. | Less accurate (only marginally) in replicating flight behaviour. |
| Comes with ROS/ROS2 support out-of-the-box |  |

## [jMAVSim](https://dev.px4.io/v1.11_noredirect/en/simulation/jmavsim.html)

| Advantages | Disadvantages |
|------------|---------------|
| Able to simulate multiple vehicles at the same time. Most scalable as it is lightweight. | No ROS 2 support. Limited out-of-the-box ROS 1 support. It is stated here that it is possible to set up ROS interface onboard the same way as with a real vehicle. No examples available, so will have to test this out separately. |
| Simulation responds appropriately to various fail conditions (GPS failure, [adversarial flight conditions](https://www.researchgate.net/publication/353541566_Experimental_Implementation_of_an_Adaptive_Digital_Autopilot), etc.) | Least accurate option to replicate drone behaviour. |
| Comes with ROS/ROS2 support out-of-the-box. | Limited to only multi-rotor/quadrotor setups. |
| Extremely lightweight (resource use wise). | More difficult to set-up multi-vehicle simulation than Gazebo. |
| Easy to set-up. |  |

## [FlightGear](https://dev.px4.io/v1.11_noredirect/en/simulation/flightgear.html)

| Advantages | Disadvantages |
|------------|---------------|
| Most accurate simulator. Able to replicate weather conditions very close to reality. | Limited support available with ROS. No indication if there is ROS 2 support. |
| Able to simulate multiple vehicles at the same time. | Very heavy in resource usage. Excessive for most application cases. |
| Visually realistic. | Due to resource usage, there is a limit on how many drones can be simulated at the same time. |

## [Microsoft AirSim](https://dev.px4.io/v1.11_noredirect/en/simulation/airsim.html)

| Advantages | Disadvantages |
|------------|---------------|
| Built on Unreal Engine. Realistic physics simulation. | Limited support available with ROS. No indication if there is ROS 2 support. |
|  | No indication of support with latest Linux kernels. |
|  | No support for multiple vehicle simulations at the same time. |
|  | Very high resource usage. |
|  | No support for multiple vehicle simulations at the same time. |

# [[technical.terminologies.software-in-the-loop]] (SITL)

To run the simulations, you need to first enter the PX4-Autopilot toolchain directory that was cloned.
![[technical.frameworks.px4#autopilot-installation]]

```bash
cd ./PX4-Autopilot
```

From there, we can build the code for which ever simulation environment that we want as long as it exists within the build tags.

```bash
# For Gazebo Simulation
make px4_sitl gz_x500

# For jMAVSim simulation
make px4_sitl jmavsim
```
From here, in a separate terminal, if you run [[QGroundControl|technical.frameworks.px4#qgroundcontrol]].