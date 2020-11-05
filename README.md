# eus_vive
[![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/knorth55/eus_vive/CI/master)](https://github.com/knorth55/eus_vive/actions)

Robot remote control program with Vive/Oculus

## Note

This package depends on these branches below:
- [knorth55/baxter_interface@baxter-vive-control](https://github.com/knorth55/baxter_interface/tree/baxter-vive-control)
- [knorth55/jsk_robot@vive-control](https://github.com/knorth55/jsk_robot/tree/vive-control)
  - [jsk-ros-pkg/jsk_robot#1150](https://github.com/jsk-ros-pkg/jsk_robot/pull/1150)
- [knorth55/jsk_pr2eus@vive-control](https://github.com/knorth55/jsk_pr2eus/tree/vive-control)
  - [jsk-ros-pkg/jsk_pr2eus#443](https://github.com/jsk-ros-pkg/jsk_pr2eus/pull/443)
- [knorth55/rviz_camera_stream@master](https://github.com/knorth55/rviz_camera_stream)
- [knorth55/vive_ros@use-camera](https://github.com/knorth55/vive_ros/tree/use-camera)
- [tohirose/dynamixel_motor@softhand-v2-devel](https://github.com/tohirose/dynamixel_motor/tree/softhand-v2-devel)

## Tested Environment

### Build environment

- Ubuntu 16.04 + ROS Kinetic
  - NVidia driver: `396.37`
  - OpenVR: `1.3.22`
  - Steam VR: `1.6.10`
- Ubuntu 18.04 + ROS Melodic
  - NVidia driver: `390.116`
  - OpenVR: `1.3.22`
  - Steam VR: `1.6.10`

### VR Devices

- Vive
  - [x] Arm motion tracking
  - [x] Controller button interface
  - [x] HMD visual interface 
  - [x] Vibration interface
  - [x] Sound interface
- Oculus
  - [x] Arm motion tracking
  - [ ] Controller button interface
  - [ ] HMD visual interface 
  - [ ] Vibration interface
  - [ ] Sound interface

## Installation

### Dependency installation

#### Install dependencies (for Vive)

```bash
sudo apt-get install --reinstall xserver-xorg-video-intel libgl1-mesa-glx libgl1-mesa-dri xserver-xorg-core
sudo dpkg-reconfigure xserver-xorg
```

#### Install nvidia-driver (for Vive)

```bash
# for melodic, run command below
sudo apt install nvidia-driver-390
# for kinetic, install cuda-9.2 deb (local) manually and run command below.
sudo apt install nvidia-396
```

#### Install `OpenVR`, `steam` and `steamVR` (for Vive)

Follow [here](https://github.com/knorth55/vive_ros)

#### Install kodak 4k pro camera udev (for Baxter)

```bash
sudo cp udev/99-kodak.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules && sudo udevadm trigger
```

### ROS Workspace build

```bash
mkdir ~/vive_ws/src -p
cd ~/vive_ws/src
wget https://raw.githubusercontent.com/knorth55/eus_vive/master/fc.rosinstall -O .rosinstall
wstool up
rosdep install --ignore-src --from-path . -y -r -i
cd ~/vive_ws
catkin config
catkin build
```

## How to start 

### PR2 + Vive

#### PR1040 in JSK 73B2

```bash
rossetip
rossetmaster pr1040
roslaunch eus_vive pr2_73b2_vive.launch
```

#### PR1012 in JSK 610 

```bash
rossetip
rossetmaster pr1012
roslaunch eus_vive pr2_610_vive.launch
```

#### Gazebo

```bash
roslaunch eus_vive pr2_gazebo_vive.launch
```

### Baxter + Vive

#### Real Robot in JSK 73B2

```bash
rossetip
rossetmaster baxter
roslaunch eus_vive baxter_73b2_vive.launch
```

#### Gazebo

```bash
roslaunch baxter_gazebo baxter_world.launch
roslaunch eus_vive vive.launch
roslaunch eus_vive baxter_gazebo_vive.launch
```

### Demo & Experiments

#### Miraikan Demo 2019/08/23-24

```bash
rossetip
rossetmaster baxter
roslaunch eus_vive baxter_miraikan_mirror_vive.launch
```

#### Miraikan Demo 2020/09/11-13

##### Vive control PC (Pilot side)

```bash
rossetip
rossetbaster
roslaunch eus_vive baxter_miraikan_remote_vive.launch
```
##### Visualization, display and feedback PC (Pilot side)

```bash
rossetip
rossetbaster
roslaunch eus_vive baxter_miraikan_remove_display.launch
```

##### Robot control PC (Robot side)

```bash
rossetip
rossetbaster
roslaunch eus_vive baxter.launch
```


## How to use Vive controller

![Vive controller](https://www.vive.com/media/filer_public/e3/da/e3daf208-4d4e-4adf-b911-22f9458ab883/guid-2d5454b7-1225-449c-b5e5-50a5ea4184d6-web.png)


### PR2

| Button | Usage |
|:-:|:-:|
| 1 / Menu | Control toggle: base / arm (Default: base) |
| 3 / Stream Menu | Steam Menu |
| 8 / Grip | Not used |

#### Arm mode

You can enable arm mode of right and left arm separately.

| Command | Usage |
|:-:|:-:|
| 7 / Trigger | Gripper toggle: open/close (Default: open) | 
| Controller pose | robot end effector's pose |

#### Base mode

Base mode is enabled when both arms are disabled in Arm mode.

| Command | Usage |
|:-:|:-:|
| 2 / Trackpad | Torso control: right: down / left: up |
| 2 / Trackpad + 7 / Trigger (right) | Safe base control: right: x, y / left: w |
| 2 / Trackpad + 7 / Trigger (right + left) | Unsafe base control: right: x, y / left: w |

### Baxter

| Button | Usage |
|:-:|:-:|
| 2 / Trackpad | Control toggle: stop / arm (Default: stop) |
| 3 / Stream Menu | Steam Menu |
| 8 / Grip | Not used |

#### Arm mode

You can enable arm mode of right and left arm separately.

| Command | Usage |
|:-:|:-:|
| 7 / Trigger | Gripper toggle: open/close (Default: open) |
| Controller pose | robot end effector's pose |

## Demo

### PR2 Fridge demo
- Trial 1: https://drive.google.com/open?id=1NsNnplM9bZQi78BY5IJC7CGwROcHd_Nc
- Trial 2: https://drive.google.com/open?id=1BSpfWCgKMXQykrTQrrrhYMP5UryFPHr7
- Trial 3: https://drive.google.com/open?id=13E_h0JvgZn_RpvnEMDYGR6lNazLvFG0w

### Baxter APC demo
- Trial 1: https://drive.google.com/open?id=156y7_M6OwD9z6B4RKwZ0UPLnyHKaNKFm
- Trial 2: https://drive.google.com/open?id=1Qb0k0Uzj0pTZhNvM7VIgD6g0RC1ohqg1
- Trial 3: https://drive.google.com/open?id=10XZ_5bBKgEk_QqtONCfnXYRLKTE6rpgE
- Trial 4: https://drive.google.com/open?id=1IyVME3OggckIfDYCDwIjQmdNneWYdXWd
- Trial 5: https://drive.google.com/open?id=1jwq_UdDzgDf-UfBpAS0G0KeykvZ-gLhW

## Tips

### Baxter network configuration

Open [Field Service Menu](https://sdk.rethinkrobotics.com/wiki/Field_Service_Menu_(FSM)) and change network configuration
