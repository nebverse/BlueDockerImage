# BlueDockerImage

This repository hosts the Docker image for the **Blue simulator**, configured with the `humble-desktop` tag for underwater robotics simulation. The image includes all the necessary dependencies for working with the BlueROV2 and related tasks like teleoperation, simulation, and control.

## Contents

- **Blue Docker Image**: Preconfigured container for BlueROV2 simulation and control.
- **Instructions**: Steps to set up and run the container, teleoperate the BlueROV2, and use simulation tools.
- **Documentation**: Includes how to work with the provided Docker image, keyboard teleoperation, and additional configuration details.

---

## Getting Started

### Prerequisites

- Docker and Docker Compose installed on your system.
- NVIDIA graphics card (optional, for GPU acceleration).

### Pull the Docker Image

To pull the Docker image from the **GitHub Container Registry (GHCR)**:

```bash
docker pull ghcr.io/nebverse/blue:humble-desktop
```

## Running the Blue Image

### 1. Running the Container Using Compose

Depending on your system configuration, use the appropriate command:

- **No NVIDIA Graphics Card:**
  ```bash
  docker compose -f nouveau-desktop.yaml up
  ```

- **With NVIDIA Graphics Card:**
  ```bash
  docker compose -f nvidia-desktop.yaml up
  ```


### 2. Accessing the Container

To run a shell inside the container, use the following command in a new terminal:

```bash
docker compose -f nouveau-desktop.yaml up
```

Note: The container name (usuario-blue-1) may vary depending on the folder from where it is executed. Use docker ps to check active containers.


### 3. Launching Applications

Once inside the container, you can launch applications like the BlueROV2 simulation. For example:

```bash
ros2 launch blue_bringup bluerov2.launch.yaml use_sim:=true
```

## Keyboard Teleoperation

### Steps to Teleoperate the BlueROV2 Using Your Keyboard

1. **Launch the Demo in Simulation:**
   ```bash
   ros2 launch blue_demos bluerov2_demo.launch.yaml use_sim:=true
   ```

2. **Launch the Demo Control Framework in a new terminal:**
   ```bash
   ros2 launch blue_demos bluerov2_controllers.launch.py use_sim:=true
   ```

3. **Open another terminal and launch the teleop_twist_keyboard node:**
   ```bash
   ros2 run teleop_twist_keyboard teleop_twist_keyboard
   ```

4. **Run the message_transforms node to convert velocity commands to the correct maritime conventions:**
   ```bash
   ros2 launch message_transforms message_transforms.launch.py \
   parameters_file:=/home/blue/ws_blue/src/blue/blue_demos/teleoperation/config/transforms.yaml
   ```

5. **You should now be able to teleoperate the BlueROV2 using your keyboard.**

## Project Overview

### What’s Included in This Package

- **Simulation Setup**: Pre-configured BlueROV2 simulation environment.
- **Teleoperation Framework**: Tools for controlling the BlueROV2 using keyboard inputs.
- **ROS2 Integration**: Launch files and configuration for BlueROV2 control, including ArUco marker detection and manipulator integration.
- **Gripper Arm Control**: Integration of the gripper arm endpoint with revolute joints for object manipulation.

## Pool, Box, and Gripper Integration

This section highlights the work done to integrate the virtual UJI pool, black box, and gripper arm into the simulation environment, creating a realistic setup for underwater robotics tasks.

### 1. Virtual UJI Pool
The **UJI pool** was integrated into the Blue simulator to replicate a controlled underwater environment. This pool serves as the testing ground for navigation, detection, and manipulation tasks, providing a realistic yet safe space to validate the robot's capabilities.

### 2. Black Box with ArUco Marker
A **black box** model was added to the environment, featuring an **ArUco marker** on one side. The marker is crucial for localization, as it allows the robot to detect and align itself with the box using visual feedback. This setup mimics the real-world scenario where markers assist in underwater object detection and retrieval.

### 3. Gripper Arm and Endpoint
A **gripper arm** was attached to the BlueROV2 in the simulation. This arm features:
- **Revolute Joints**: For precise and flexible movements.
- **Endpoint Manipulation**: Designed to securely grasp and manipulate objects, such as the black box.
  
This integration ensured that the robot could perform realistic retrieval tasks, bridging the gap between simulation and real-world application.

### Key Takeaways
The integration of the pool, box, and gripper arm significantly enhanced the realism of the simulation environment, enabling more accurate testing and validation of the BlueROV2's functionality.
