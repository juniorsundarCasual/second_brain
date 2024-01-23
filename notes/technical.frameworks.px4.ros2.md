---
id: fzk8zi1ywesxgonead65obf
title: PX4-ROS 2 Integration
desc: ''
updated: 1706006247501
created: 1704872897172
tags:
    - software
    - robotics
    - drone
---

# Setting up a ROS 2 Workspace

## Download

ROS 2 comes installed with some basic packages. A ROS package is a directory or folder that contains code, data, and configuration files that provide a specific functionality or module. For instance, the “`sensor_msgs`” package contains common ROS 2 message types used to transfer sensor data such as PointCloud2, Image, DepthCloud, and so on.

For local installations, we need to set up a ROS Workspace. A ROS workspace is needed in this particular case because we need to install the `px4_msgs` and `px4_ros_coms` packages.

```bash
# Create a workspace folder
mkdir -p ~/px4_ws/src
cd ~/px4_ws/src
# Clone the two packages into it
git clone https://github.com/PX4/px4_msgs.git
git clone https://github.com/PX4/px4_ros_com.git
```

The `px4_msgs` is important for the ROS interface between controller and processor. All the uORB topics sent from the controller and repackaged into ROS 2 topics. In order for this to happen, there needs to be the appropriate message types in the companion processor which are inside the px4_msgs package.

Finally, we need to build the packages. ROS 2 uses the colcon build tool.

## Build

```bash
# Navigate to workspace root
cd ~/px4_ws
# Build packages
colcon build
```

This should take some time at first run as the tool generates all the message files. Once finished, the workspace should have the following structure:

```
L px4_ws/
    L build/
       L ...
    L install/
       L ...
    L log/
       L ...
    L src/
       L px4_msgs
       L px4_ros_coms
       L <add more here>
```

## Source

Finally, since this is a local installation, we need to source this workspace so that we can access the locally installed packages. Refer to this for more explanation:

![[technical.frameworks.ros.ros2#sourcing-the-environment]]

In every new terminal that requires you to access packages from this local installation, run:

```bash
source ~/px4_ws/install/local_setup.bash
```

# Listening to Data

## Launch Simulation

The first step of this process requires us to launch the simulation through either Gazebo or JMavSim (i.e. simulator of choice). This is described in the following excerpt:

![[technical.frameworks.px4.simulation#launch-simulation]]

For this case, we will go with Gazebo. Upon building for SITL, the following window should open up:

![Gazebo simulation screen with a x500 drone](./assets/technical.frameworks.px4.ros21.png)


## Initialise μXRCE-DDS Agent 

After than, ensure that the DDS agent is running so that the uORB topics are exposed to the ROS 2 framework. In a separate terminal run:

```bash
MicroXRCEAgent udp4 -p 8888 
```

## Subscribing to ROS 2 Topics

As explained before, data from the PX4 is obtained from the ULog. The information is packaged into uORB topics inside the controller and transmitted to the companion processor through the μXRCE-DDS channel. At the agent side, the computer, this information is shared with ROS 2 as topics. The uORB topics are directly translated into ROS 2 topics once you activate the MicroXRCEAgent.

To see the topics available, open a new terminal and [[source |technical.frameworks.px4.ros2#source]] the local installation of `px4_msgs`. After that, run:

```bash
ros2 topic list
```

In the terminal, you should now see something like this:

```bash
# Publishable
/fmu/in/obstacle_distance
/fmu/in/offboard_control_mode
/fmu/in/onboard_computer_status
/fmu/in/sensor_optical_flow
/fmu/in/telemetry_status
/fmu/in/trajectory_setpoint
/fmu/in/vehicle_attitude_setpoint
/fmu/in/vehicle_command
/fmu/in/vehicle_mocap_odometry
/fmu/in/vehicle_rates_setpoint
/fmu/in/vehicle_trajectory_bezier
/fmu/in/vehicle_trajectory_waypoint
/fmu/in/vehicle_visual_odometry
# Subscribable
/fmu/out/failsafe_flags
/fmu/out/position_setpoint_triplet
/fmu/out/sensor_combined
/fmu/out/timesync_status
/fmu/out/vehicle_attitude
/fmu/out/vehicle_control_mode
/fmu/out/vehicle_global_position
/fmu/out/vehicle_gps_position
/fmu/out/vehicle_local_position
/fmu/out/vehicle_odometry
/fmu/out/vehicle_status
```

All subscribable topics can be observed in the terminal itself with the following command:

```bash
ros2 topic echo /fmu/out/<topic_name>
```

**Note** that if you don’t have the local installation sourced, the following error might pop up:

```bash
The message type '<message_name>' is invalid
```

In that case, simply [[source | technical.frameworks.px4.ros2#source]] the local installation for the terminal and try again.

To get information about the message type being sent through these topics, run:

```bash
ros2 topic info /fmu/out/<topic_name>
```

The following subscribable topics are available out-of-the-box. A brief explanation of the data that is structured inside them is provided alongside.

| Topic Name                        | Description                                                                 |
|-----------------------------------|-----------------------------------------------------------------------------|
| /.../out/failsafe_flags           | Flags for the failsafe state machine set by the arming & health checks.     |
| /.../out/position_setpoint_triplet| Global position setpoint triplet in WGS84 coordinates.                      |
| /.../out/sensor_combined          | Sensor readings in SI-unit form (gyro, accelerometer).                      |
| /.../out/timesync_status          |                                                                             |
| /.../out/vehicle_attitude         | This is similar to the mavlink message ATTITUDE_QUATERNION, but for onboard use. |
| /.../out/vehicle_control_mode     | Flagging control modes.                                                     |
| /.../out/vehicle_global_position  | Fused global position in WGS84. It is global position estimation not raw GPS. |
| /.../out/vehicle_gps_position     | GPS position in WGS84 coordinates.                                          |
| /.../out/vehicle_local_position   | Fused local position in NED frame. Origin set to vehicle position at time of EKF module start. |
| /.../out/vehicle_odometry         | Vehicle odometry data. Fits ROS REP 147 for aerial vehicles.                |
| /.../out/vehicle_status           | Encodes system state of vehicle published by commander.                     |


## Exposing More Topics

[[These |technical.frameworks.px4.uorb#messages]] are the messages being actively exchanged inside the PX4 through uORB topics. Nearly all of them can also be accessed in ROS 2 as long as the Micro XRCE-DDS agent is active. You can find a list of message types available in ROS 2 [here](https://github.com/PX4/px4_msgs/tree/main/msg). You can cross-reference the uORB messages with the ROS 2 messages.

However, you can see that only a few out of this list are actually available when you call up the topic list. This is because it is possible to only passthrough selective topics through the Micro XRCE-DDS bridge. If you want to access more topics that aren’t in ROS 2 but available in the PX4 controller, add them to the [`dds_topics.yaml`](https://github.com/PX4/PX4-Autopilot/blob/main/src/modules/uxrce_dds_client/dds_topics.yaml) file in `~/PX4-Autopilot/blob/main/src/modules/uxrce_dds_client/`. You may need to build again after making this change.

Instructions on exposing additional topics through the DDS connection can be found [here](https://docs.px4.io/main/en/middleware/uxrce_dds.html#dds-topics-yaml).

# Controling Drone

## Offboard Mode

Control of PX4 through ROS 2 requires the drone to be set to offboard mode. The vehicle obeys position, velocity, acceleration, attitude, attitude rates or thrust/torque setpoints provided by some source that is external to the flight stack, such as a companion computer.

IMPORTANT: PX4 requires a steady (minimum) 2Hz “proof-of-life“ signal. This is either any of the MAVLink supported commands or ROS 2 messages. If this is not satisfied, the controller will exit offboard control.

## Create Package

Control scripts for PX4 can be written in C++ or Python and launch as ROS 2 nodes. Here, we will only focus on the Python implementations.

The nodes that will control the drone will be added to a new ROS 2 package inside the pre-existing workspace made [[here |technical.frameworks.px4.ros2#download]]. First, navigate to the workspace root:

```bash
# If the following workspace does not exist, refer to the hyperlinked page to create the workspace.
# This is important since packages relevant to controlling the PX4 are installed in this workspace.
cd ~/px4_ws
```

The source files for packages inside this workspace are in the `/src` directory. In here, we will create a new package called `px4_control_py`. This is where all the Python nodes will be added. This package will have a set of dependencies:  `rclpy`, `px4_msgs`, `std_msgs`,  `sensor_msgs`, `geometry_msgs`. This package will use the `ament_python` build system since the nodes are primarily in Python.

```bash
cd ./src
# Create package
ros2 pkg create --build-type ament_python px4_control_py --dependencies rclpy px4_msgs std_msgs sensor_msgs geometry_msgs
```

This should create a new folder inside the directory. Observe the directory tree:

```
L px4_ws/
    L build/
       L ...
    L install/
       L ...
    L log/
       L ...
    L src/
       L px4_msgs
       L px4_ros_coms
       L px4_control_py
          L px4_control_py
             L __init__.py
          L resource
          L test
          L package.xml
          L setup.cfg
          L setup.py
```

## Writing a Control Node

The node will be a Python script, and will be located inside `px4_control_py` package.

```bash
touch ~/px4_ws/src/px4_control_py/px4_control_py/px4_control_node.py
```

Copy and paste the following code inside the script file:

<details>

  <summary>Show code</summary>

```python
# px4_control_node.py
import rclpy
from rclpy.node import Node
from rclpy.qos import QoSProfile, ReliabilityPolicy, HistoryPolicy, DurabilityPolicy
from px4_msgs.msg import OffboardControlMode, TrajectorySetpoint, VehicleCommand, VehicleLocalPosition, VehicleStatus


class OffboardControl(Node):
    """Node for controlling a vehicle in offboard mode."""

    def __init__(self) -> None:
        super().__init__('offboard_control_node')

        # Configure QoS profile for publishing and subscribing
        qos_profile = QoSProfile(
            reliability=ReliabilityPolicy.BEST_EFFORT,
            durability=DurabilityPolicy.TRANSIENT_LOCAL,
            history=HistoryPolicy.KEEP_LAST,
            depth=1
        )

        # Create publishers
        self.offboard_control_mode_publisher = self.create_publisher(
            OffboardControlMode, '/fmu/in/offboard_control_mode', qos_profile)
        self.trajectory_setpoint_publisher = self.create_publisher(
            TrajectorySetpoint, '/fmu/in/trajectory_setpoint', qos_profile)
        self.vehicle_command_publisher = self.create_publisher(
            VehicleCommand, '/fmu/in/vehicle_command', qos_profile)

        # Create subscribers
        self.vehicle_local_position_subscriber = self.create_subscription(
            VehicleLocalPosition, '/fmu/out/vehicle_local_position', self.vehicle_local_position_callback, qos_profile)
        self.vehicle_status_subscriber = self.create_subscription(
            VehicleStatus, '/fmu/out/vehicle_status', self.vehicle_status_callback, qos_profile)

        # Initialize variables
        self.offboard_setpoint_counter = 0
        self.vehicle_local_position = VehicleLocalPosition()
        self.vehicle_status = VehicleStatus()
        self.takeoff_height = -5.0

        # Create a timer to publish control commands
        self.timer = self.create_timer(0.1, self.timer_callback) # 10Hz frequency

    def vehicle_local_position_callback(self, vehicle_local_position):
        """Callback function for vehicle_local_position topic subscriber."""
        self.vehicle_local_position = vehicle_local_position

    def vehicle_status_callback(self, vehicle_status):
        """Callback function for vehicle_status topic subscriber."""
        self.vehicle_status = vehicle_status

    def arm(self):
        """Send an arm command to the vehicle."""
        self.publish_vehicle_command(
            VehicleCommand.VEHICLE_CMD_COMPONENT_ARM_DISARM, param1=1.0)
        self.get_logger().info('Arm command sent')

    def disarm(self):
        """Send a disarm command to the vehicle."""
        self.publish_vehicle_command(
            VehicleCommand.VEHICLE_CMD_COMPONENT_ARM_DISARM, param1=0.0)
        self.get_logger().info('Disarm command sent')

    def engage_offboard_mode(self):
        """Switch to offboard mode."""
        self.publish_vehicle_command(
            VehicleCommand.VEHICLE_CMD_DO_SET_MODE, param1=1.0, param2=6.0)
        self.get_logger().info("Switching to offboard mode")

    def land(self):
        """Switch to land mode."""
        self.publish_vehicle_command(VehicleCommand.VEHICLE_CMD_NAV_LAND)
        self.get_logger().info("Switching to land mode")

    def publish_offboard_control_heartbeat_signal(self):
        """Publish the offboard control mode."""
        msg = OffboardControlMode()
        msg.position = True
        msg.velocity = False
        msg.acceleration = False
        msg.attitude = False
        msg.body_rate = False
        msg.timestamp = int(self.get_clock().now().nanoseconds / 1000)
        self.offboard_control_mode_publisher.publish(msg)

    def publish_position_setpoint(self, x: float, y: float, z: float):
        """Publish the trajectory setpoint."""
        msg = TrajectorySetpoint()
        msg.position = [x, y, z]
        msg.yaw = 1.57079  # (90 degree)
        msg.timestamp = int(self.get_clock().now().nanoseconds / 1000)
        self.trajectory_setpoint_publisher.publish(msg)
        self.get_logger().info(f"Publishing position setpoints {[x, y, z]}")

    def publish_vehicle_command(self, command, **params) -> None:
        """Publish a vehicle command."""
        msg = VehicleCommand()
        msg.command = command
        msg.param1 = params.get("param1", 0.0)
        msg.param2 = params.get("param2", 0.0)
        msg.param3 = params.get("param3", 0.0)
        msg.param4 = params.get("param4", 0.0)
        msg.param5 = params.get("param5", 0.0)
        msg.param6 = params.get("param6", 0.0)
        msg.param7 = params.get("param7", 0.0)
        msg.target_system = 1
        msg.target_component = 1
        msg.source_system = 1
        msg.source_component = 1
        msg.from_external = True
        msg.timestamp = int(self.get_clock().now().nanoseconds / 1000)
        self.vehicle_command_publisher.publish(msg)

    def timer_callback(self) -> None:
        """Callback function for the timer."""
        self.publish_offboard_control_heartbeat_signal()

        if self.offboard_setpoint_counter == 10:
            self.engage_offboard_mode()
            self.arm()

        if self.vehicle_local_position.z > self.takeoff_height and self.vehicle_status.nav_state == VehicleStatus.NAVIGATION_STATE_OFFBOARD:
            self.publish_position_setpoint(0.0, 0.0, self.takeoff_height)

        elif self.vehicle_local_position.z <= self.takeoff_height:
            self.land()
            exit(0)

        if self.offboard_setpoint_counter < 11:
            self.offboard_setpoint_counter += 1

def main(args=None) -> None:
    print('Starting offboard control node...')
    rclpy.init(args=args)
    offboard_control = OffboardControl()
    rclpy.spin(offboard_control)
    offboard_control.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    try:
        main()
    except Exception as e:
        print(e)
```
</details>

This  node will activate offboard control mode and publish a offboard control signal (`self.publish_offboard_control_heartbeat_signal()`) to the UAV at a 10Hz frequency to ensure that it doesn’t leave the offboard control mode.

It will then arm the drone, cause it to takeoff to a height of 5m, and then land safely. Edit the following segment of the code to match your control needs:


```python
def timer_callback(self) -> None:
        """Callback function for the timer."""
        self.publish_offboard_control_heartbeat_signal()

        if self.offboard_setpoint_counter == 10:
            self.engage_offboard_mode()
            self.arm()

        if self.vehicle_local_position.z > self.takeoff_height and self.vehicle_status.nav_state == VehicleStatus.NAVIGATION_STATE_OFFBOARD:
            self.publish_position_setpoint(0.0, 0.0, self.takeoff_height)

        elif self.vehicle_local_position.z <= self.takeoff_height:
            self.land()
            exit(0)

        if self.offboard_setpoint_counter < 11:
            self.offboard_setpoint_counter += 1
```

Now, all that is left is to build this node so that it can be launched.

Open in a text editor: 
`~/px4_ws/src/px4_control_py/setup.py`

Edit it so that it looks like this:

```python
# setup.py
from setuptools import find_packages, setup

package_name = 'px4_control_py'

setup(
    name=package_name,
    version='0.0.0',
    packages=find_packages(exclude=['test']),
    data_files=[
        ('share/ament_index/resource_index/packages',
            ['resource/' + package_name]),
        ('share/' + package_name, ['package.xml']),
    ],
    install_requires=['setuptools'],
    zip_safe=True,
    maintainer='name',
    maintainer_email='name@todo.todo',
    description='TODO: Package description',
    license='TODO: License declaration',
    tests_require=['pytest'],
    entry_points={
        'console_scripts': [
            'offboard_control_node = px4_control_py.px4_control_node:main',
        ],
    },
)
```

Finally, in a new terminal, run the following command:

```bash
# Build the new node and source the workspace
cd ~/px4_ws
colcon build
source install/local_setup.bash
```

Now, we can launch this node alongside our simulation. Run the following sequence of terminal command.

```bash
# in Terminal 1
cd ~/PX4-Autopilot
make px4_sitl gz_x500
# in Terminal 2
MicroXRCEAgent udp4 -p 8888
# in Terminal 3
source ~/px4_ws/install/local_setup.bash
ros2 run px4_control_py px4_control_node
```