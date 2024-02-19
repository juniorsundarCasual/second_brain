# Software-in-the-Loop (SITL)
	- > Access [reference]([[Terminology/Software-in-the-Loop]])
	- ## Set-Up
		- To run the simulations, you need to first enter the PX4-Autopilot toolchain directory that was cloned. This is explained in this {:$/notes/technical/frameworks/px4:*** Autopilot Installation}[section].
		- ```bash
		  cd ./PX4-Autopilot
		  ```
	- ## Launch Simulation
		- From there, we can build the code for which ever simulation environment that we want as long as it exists within the build tags.
		- ```bash
		  # For Gazebo Simulation
		  make px4_sitl gz_x500
		  # For jMAVSim simulation
		  make px4_sitl jmavsim
		   ```
		- From here, in a separate terminal, if you run [QGroundControl], you should be able to control the drone through that.
	- ## Low-Level Control
		- The out-of-the-box set-up for SITL does not provide access to PWM inputs into the actuators. This is because the simulation does not have an ESC model. As a result direct control of the simulated motors is not possible.
		- There is a repository that offers a way to achieve this low-level control [SaxionMechatronics/px4_offboard_lowlevel](https://github.com/SaxionMechatronics/px4_offboard_lowlevel)
		- In this code, they are using a commonly used "quadratic approximation" that maps RPM to PWM:
		- $\text{PWM} = c_2 \text{RPM}^2 + c_1 \text{RPM} + c_0$
		- In this application:
		- ```yaml
		  # For the holybro x500
		  ros__parameters:
		       uav_parameters:
		           mass: 2.0                  # Kg
		           arm_length: 0.25             # m
		           num_of_arms: 4
		           inertia:                     # Kg.m^2
		               x: 0.08612
		               y: 0.08962
		               z: 0.16088
		           moment_constant: 0.016     # m
		           thrust_constant: 8.54858e-06   # N.s^2/rad^2
		           max_rotor_speed: 1000         # rad/s
		           gravity: 9.81                # m/s^2
		           omega_to_pwm_coefficient:
		               x_2: 0.001142
		               x_1: 0.2273
		               x_0: 914.2
		           PWM_MIN: 1075
		           PWM_MAX: 1950
		           input_scaling: 1000
		           zero_position_armed: 10
		  ```
- # Hardware-in-the-Loop (HITL)
  id:: 65d37aec-1660-44bf-8b7d-291d5b966eae
	- > Access [reference]([[Terminology/Hardware-in-the-Loop]])
	- ## Set-Up with Saluki V2
		- In order to connect via ethernet, need to provide the following details for `eth0` connection:
		- > *IPv4 Method*: Manual
		  > *Addresses*:
		  > *Address*: 192.168.200.100
		  > *Netmask*: 255.255.255.0
		  > *Gateway*: 192.168.200.1
		- The drone should now connect to `/dev/ttyACM1` after rebooting drone. Can confirm in QGroundControl by connecting to that channel.
	- ## Launch Simulation
		- Then, to launch HiTL, run:
		- ```bash
		  # From px4-firmware root
		  # Confirmed to work with github.com/tiiuae/px4-firmware -b faulty-controller-hitl
		   ./Tools/simulation/jmavsim/jmavsim_run.sh -q -s -d /dev/ttyACM1 -b 921600 -r 250
		  ```
	- ## Access ROS 2 Topics
		- To launch the DDS-Agent for accessing ROS 2 Topics:
		- ```bash
		  docker pull ghcr.io/tiiuae/tii-microxrce-agent:sha-9e67fa5
		  docker run -it --rm --network=host ghcr.io/tiiuae/tii-microxrce-agent:sha-9e67fa5
		  ```
		- Also possible to run from local installation with open-source MicroXRCE-DDS Agent:
		  
		   ```bash
		   MicroXRCEAgent udp4 --port 2020 --send_port 2019 --refs ./agent.refs
		  ```
		- Where `agents.refs` file is defined:
		  
		  ```xml
		   <profiles>
		       <participant profile_name="default_xrce_participant">
		           <domainId>0</domainId>
		           <rtps>
		               <name>default_xrce_participant</name>
		           </rtps>
		       </participant>
		   </profiles>
		  ```