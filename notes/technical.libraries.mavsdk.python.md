---
id: 48t3p34wpa8beuh0iaxnfic
title: MAVSDK Python
desc: ''
updated: 1705153677126
created: 1705150902976
tags:
    - software
    - drone
    - python
---

# Installation

The `mavsdk-python` library can be installed through the `pip` Python module.

```bash
python3 -m pip install mavsdk
```

_The GitHub repository to this library can be found [here](https://github.com/mavlink/MAVSDK-Python)._


# Tutorials and Examples

Sample codes for usage of this library can be found in the repository under the [examples](https://github.com/mavlink/MAVSDK-Python/tree/main/examples) directory.

## Boilerplate

```python
import asyncio

from mavsdk import System

async def run():
    drone = System()
    await drone.connect(system_address="udp://:14540")

    print("Waiting for drone to connect...")
    async for state in drone.core.connection_state():
        if state.is_connected:
            print(f"-- Connected to drone!")
            break

    print("Waiting for drone to have a global position estimate...")
    async for health in drone.telemetry.health():
        if health.is_global_position_ok and health.is_home_position_ok:
            print("-- Global position estimate OK")
            break

    ## The rest of the code runs here
    ## ...
    ## ______________________________
    
    return

if __name__ == "__main__":
    # Run the asyncio loop
    asyncio.run(run())
```

# Asynchronicity

As seen in the section [[above|technical.libraries.mavsdk.python#boilerplate]], the MAVSDK Python library uses asynchronous functions. This is primarily due to the nature of communication and control in drone operations.

1. **Non-blocking Operations** - Drone control involves a lot of real-time operations, like sending commands to the drone, receiving telemetry data, and processing sensor inputs. Async functions allow these operations to occur without blocking the execution of other parts of the program.
2. **Concurrency** - Drones need to handle multiple tasks simultaneously, such as flying, capturing images, and communicating with the control station.
3. **Network Communication** - MAVSDK is used for interacting with drones over a network (often wireless). Network communication is inherently asynchronous, as it involves waiting for responses without knowing the exact timing.
4. **Improved Performance** -  By using async functions, MAVSDK can manage multiple drones or multiple tasks for a single drone without the overhead of traditional multi-threading or multi-processing.
5. **Scalability** - Asynchronous programming provides better scalability, enabling MAVSDK to handle more complex operations and larger numbers of drones without a significant increase in resource usage.