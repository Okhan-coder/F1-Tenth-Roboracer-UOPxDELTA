#SETUP OF ZED
https://developer.nvidia.com/embedded/learn/jetson-orin-nano-devkit-user-guide/software_setup.html

Make an Nvidia Developer Account 

First Download/SETUP sdk MANAGER ON HOST computer

Plug in SSD to Jetson Orin Nano and Connect to host computer through a USB-C Wire

Restart Jetson Orin in Force Recovery mode jump pins 9&10

Once Plugged SDK should automatically Sense connection 


INSTALL 6.1 with all Packages



Go here for Isaac ROS SETUP:
https://nvidia-isaac-ros.github.io/

Do Hardware Setup in Isaac-ROS setup

Follow along also test if Docker is installed on device 


RUN This terminal command to check:
sudo docker run hello-world

Do the Hardware Final Step by downloading Docker Key





Set up your Developer Enviroment so all Global Variables and Nvidia Tools  can be used inside of Terminal

First go to developer enviroment setup: https://nvidia-isaac-ros.github.io/getting_started/dev_env_setup.html

Follow Steps 2 to 4
However! STEP 4 Follow This command not Guide this to run the following Command for global ISAAC ROS WORKSPACE:

mkdir -p ~/workspaces/isaac-ros-dev/
echo "export ISAAC_ROS_WS=~/workspaces/isaac-ros-dev/" >> ~/.bashrc
source ~/.bashrc

change to this directory

mkdir src

go to src

run:

git clone -b release-3.2 https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common.git isaac_ros_common
Before moving further lets do some ZED SETUP:https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/zed_setup.html

Follow Steps 1 to 3 
Once you Connect your ZED Device to USB of Jetson Orin Nano

run: Warning: The first run of this will take a long time due to it building dependencies from scratch 

./scripts/run_dev.sh

This command should let you run ROS-2 

*ERROR: If you get this message Unable to run docker commands. If you have recently added |orinnano| to 'docker' group, you may need to log out and log back in for it to take effect.
Otherwise, please check your Docker installation. just Log out and Log back in 

The ./scripts/run_dev.sh script runs ROS 2 inside a Docker container This might take a few minutes to download

If STEP 10 of the docker download does not work follow this solution:
open a new terminal and go to 
cd ${ISAAC_ROS_WS}/src/isaac_ros_common/docker
Open with your choice of editor this file Dockerfile.aarch64
Navigate to Line 183 and Comment out "yourdfpy>=0.0.53" \

(REFERENCE) go to the last post:
https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/issues/183 

Run the Script again it should start where you left off last time

*ERROR:docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: could not apply required modification to OCI specification: error modifying OCI spec: failed to inject CDI devices: unresolvable CDI devices nvidia.com/gpu=all: unknown

Run 'docker run --help' for more information
~/workspaces/isaac-ros-dev/src/isaac_ros_common

To fix this go to: https://github.com/NVIDIA-ISAAC-ROS/isaac_ros_common/issues/163 at the bottom

Run this command from this issue: sudo nvidia-ctk cdi generate --output=/etc/cdi/nvidia.yaml --mode=csv


continued zed setup Setup: https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/sensors/zed_setup.html
continue steps 5 to 7

Step 6 needs to be researched further

ERROR IS : /zed_camera_component.cpp: In member function ‘void stereolabs::ZedCamera::publishHealthStatus()’:
/workspaces/isaac_ros-dev/src/zed-ros2-wrapper/zed_components/src/zed_camera/src/zed_camera_component.cpp:11696:35: error: ‘struct sl::HealthStatus’ has no member named ‘low_image_quality’
11696 |   msg->low_image_quality = status.low_image_quality;
      |                                   ^~~~~~~~~~~~~~~~~
/workspaces/isaac_ros-dev/src/zed-ros2-wrapper/zed_components/src/zed_camera/src/zed_camera_component.cpp:11697:30: error: ‘struct sl::HealthStatus’ has no member named ‘low_lighting’
11697 |   msg->low_lighting = status.low_lighting;
      |                              ^~~~~~~~~~~~
/workspaces/isaac_ros-dev/src/zed-ros2-wrapper/zed_components/src/zed_camera/src/zed_camera_component.cpp:11698:39: error: ‘struct sl::HealthStatus’ has no member named ‘low_depth_reliability’
11698 |   msg->low_depth_reliability = status.low_depth_reliability;
      |                                       ^~~~~~~~~~~~~~~~~~~~~
/workspaces/isaac_ros-dev/src/zed-ros2-wrapper/zed_components/src/zed_camera/src/zed_camera_component.cpp:11700:12: error: ‘struct sl::HealthStatus’ has no member named ‘low_motion_sensors_reliability’
11700 |     status.low_motion_sensors_reliability;
      |            ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
gmake[2]: *** [CMakeFiles/zed_camera_component.dir/build.make:118: CMakeFiles/zed_camera_component.dir/src/zed_camera/src/zed_camera_component.cpp.o] Error 1
gmake[1]: *** [CMakeFiles/Makefile2:166: CMakeFiles/zed_camera_component.dir/all] Error 2
gmake[1]: *** Waiting for unfinished jobs....
gmake: *** [Makefile:146: all] Error 2

Still works afterwards so should be fine

