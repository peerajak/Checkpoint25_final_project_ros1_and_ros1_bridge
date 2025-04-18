# Checkpoint25_final_project_ros1_and_ros1_bridge

ROS2 <-> Ros1 bridge <-> web bridge <-> website Architecture run on the construct machine


This is Ros1 part of the Checkpoint 25. The repo is build on Ros1 Noetic source insallation on Ubuntu 22.04 (Jammy) along with ROS2 Humble.

Since my CP25 final project of the construct is designed this way

ROS2_Humble simulator, Moveit, rviz, aruco_detector <---> Ros2's ros1_bridge package <---> Ros1_Noetic webbridge <---> Webserver,

therefore, this repository provides ROS2's ros1_bridge tar file, and Ros1 packages for CP25 project.

Ros1 Noetic source installation on Ubuntu 22.04 Jammy is quite difficult. Luckily, and thankfully someone has provide a installation procedure, below 

https://medium.com/@jean.guillaume.durand/installing-ros-noetic-on-ubuntu-22-04-1678e9dab1f5#id_token=eyJhbGciOiJSUzI1NiIsImtpZCI6IjI1ZjgyMTE3MTM3ODhiNjE0NTQ3NGI1MDI5YjAxNDFiZDViM2RlOWMiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jb20iLCJhenAiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQiOiIyMTYyOTYwMzU4MzQtazFrNnFlMDYwczJ0cDJhMmphbTRsamRjbXMwMHN0dGcuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMDcwMTY2NTE3Nzc2MjQ3NTI1NDgiLCJlbWFpbCI6InBlZXJhamFrQGdtYWlsLmNvbSIsImVtYWlsX3ZlcmlmaWVkIjp0cnVlLCJuYmYiOjE3NDE1MDY4OTIsIm5hbWUiOiJQZWVyYWphayBXaXRvb25jaGFydCIsInBpY3R1cmUiOiJodHRwczovL2xoMy5nb29nbGV1c2VyY29udGVudC5jb20vYS9BQ2c4b2NMZ3JUN1lGN2owQkNMcFBWWTZoOHduMXh0RExINDJERTRSaUgyVFpEVTlKQzM3UklDNT1zOTYtYyIsImdpdmVuX25hbWUiOiJQZWVyYWphayIsImZhbWlseV9uYW1lIjoiV2l0b29uY2hhcnQiLCJpYXQiOjE3NDE1MDcxOTIsImV4cCI6MTc0MTUxMDc5MiwianRpIjoiZTJjZDIwNjRiYTQ1OGU1ODU5NGM1NGNlMGQ1MzEyNDI5YzcwNTA5MyJ9.BnNYOgosxFmJXOs0EPrYlTohobS5Ij0Q2y5ji9gU7I2BojZhXWpjkW_iU1JSkB6OinUGXd0nd8bppsTNP4BuzkOj3p_ZUi2KXgiRzdKmkhJVg_I1nckjRHOdqLXpIjG82ILDxi2UsZTIDYllSJV71av2NpaWC6W7RDxMUnQFKk7fTOhCNKcFlcEPJsBA5lX9GPR5fbyb1a6AChYdSAVxpqex39Q7OfqvIGrdFVKZ3Z2CLBvO516MhdW98jBdBxhuJyTx9CXaAyggBacAA03b8SFikz5L-RgCmbog6Et65zPXdiD2YLPXfHL5ClEt9SrXAm69UHJ5IGBJL79sGLrtbQ

.

Ros2's ros1_bridge package is here https://github.com/ros2/ros1_bridge. Choose the master branch to install to ~/ros2_ws_ros1_bridge. Do not forget that to compile ros1_bridge package, the noetic source installation must be completed because the said package use both environment sourceing, i.e 

```
source $ROS1_INSTALL_PATH/setup.bash
source $ROS2_INSTALL_PATH/setup.bash
colcon build --packages-select ros1_bridge
```

```
git clone -b master https://github.com/ros2/ros1_bridge
```

## Real Robot

### Terminal 1: all ros2 launches

Make sure that there is no ROS environment

```
echo $ROS_DISTRO
```
expect 
humble


```
cd ~/ros2_ws
source ~/ros2_ws/install/setup.bash ; ros2 launch launch_cp25 ros2_realrobot.launch.py
```

### Terminal 2: all ros1 launches
Install dependencies

```
sudo apt update
sudo apt install libpoco-dev liblog4cxx-dev libturbojpeg* -y
pip install autobahn
pip install defusedxml
pip install pycryptodomex gnupg
```

```
unset ROS_VERSION ROS_PYTHON_VERSION  ROS_IP  ROS_DISTRO
export ROS_MASTER_URI=http://$ROS_HOSTNAME:11311
cd ~/ros1_ws
source ~/ros1_source_ws/devel_isolated/setup.bash 
source ~/ros1_ws/devel/setup.bash
roslaunch launch_cp25_ros1 ros1_realrobot_tf2_webbridge.launch.xml
```

### Terminal 3 : ros1_bridge


```
unset ROS_VERSION ROS_PYTHON_VERSION  ROS_IP  ROS_DISTRO
export ROS_MASTER_URI=http://$ROS_HOSTNAME:11311
cd ~/ros2_ws_ros1_bridge/
source ~/ros1_source_ws/devel_isolated/setup.bash ; source ~/ros2_ws/install/setup.bash
source install/setup.bash 
ros2 run ros1_bridge parameter_bridge
```


### Terminal 4: Web App

```
cd ~/ros2_ws/src/Checkpoint25_final_project/cp25_realrobot_webapp
python3 -m http.server 7000
```


### Terminal 5: addresses

```
rosbridge_address
webpage_address
```

## simulation Robot


### Terminal 1: simulator

```
cd ~/ros2_ws
source ~/ros2_ws/install/setup.bash
ros2 launch the_construct_office_gazebo starbots_ur3e.launch.xml
```



### Terminal 2: all ros2 launches

```
cd ~/ros2_ws
source ~/ros2_ws/install/setup.bash ; ros2 launch launch_cp25 ros2_sim.launch.py
```



### Terminal 3: all ros1 launch

Install dependencies

```
sudo apt update
sudo apt install libpoco-dev liblog4cxx-dev libturbojpeg* -y
pip install autobahn
pip install defusedxml
pip install pycryptodomex gnupg
```

Make sure that there is no ROS environment



```
unset ROS_VERSION ROS_PYTHON_VERSION  ROS_IP  ROS_DISTRO
export ROS_MASTER_URI=http://$ROS_HOSTNAME:11311
cd ~/ros1_ws
source ~/ros1_source_ws/devel_isolated/setup.bash 
source ~/ros1_ws/devel/setup.bash
roslaunch launch_cp25_ros1 ros1_tf2_webbridge.launch.xml
```

### Terminal 4: ros2's ros1_bridge


```
unset ROS_VERSION ROS_PYTHON_VERSION  ROS_IP  ROS_DISTRO
export ROS_MASTER_URI=http://$ROS_HOSTNAME:11311
cd ~/ros2_ws_ros1_bridge/
source ~/ros1_source_ws/devel_isolated/setup.bash ; source ~/ros2_ws/install/setup.bash
source install/setup.bash 
ros2 run ros1_bridge parameter_bridge
```



### Terminal 5: Web App

```
cd ~/ros2_ws/src/Checkpoint25_final_project/cp25_webapp
python3 -m http.server 7000
```


### Terminal 6: addresses

```
rosbridge_address
webpage_address
```

## building 

Ros1

```
cd  ~/ros1_ws
catkin_make
```

Ros2

```
cd ~/ros2_ws
colcon build
```

ROS 2's ros1_bridge

```
cd ~/ros2_ws_ros1_bridge
source ~/ros1_source_ws/devel_isolated/setup.bash 
source ~/ros1_ws/source devel/setup.bash
colcon build --packages-select ros1_bridge
```



# ROS1_bridge on Ubuntu Jammy with ROS2 humble, and ROS1 noetic

This prove that ros1_bridge can be installed on Ubuntu Jammy with ROS2 humble, and ROS1 noetic

Prerequisite

Install ROS2 humble with apt install
install ROS1 noetic from source

then place this two lines in .bashrc

```
export ROS1_INSTALL_PATH=/home/peerajak/ros1_source_ws/install_isolated/
export ROS2_INSTALL_PATH=/opt/ros/humble/
```


and do

```
cd ~
. .bashrc
```
make sure that .bashrc do not source any ROS environment.


### how to clone 

```
mkdir -p ros2_ws_ros1_bridge/src
cd ros2_ws_ros1_bridge/src/
git clone -b master https://github.com/ros2/ros1_bridge.git
```

### how to build

```
cd ros2_ws_ros1_bridge
source $ROS1_INSTALL_PATH/setup.bash; source $ROS2_INSTALL_PATH/setup.bash
colcon build --packages-select ros1_bridge
```
 
### how to run

```
cd ros2_ws_ros1_bridge
source install/setup.bash 
ros2 run ros1_bridge dynamic_bridge
```


## Example 1a: ROS 1 talker and ROS 2 listener

Terminal 1

```
source $ROS1_INSTALL_PATH/setup.bash
roscore
```

Terminal 2

```
cd ~/ros2_ws_ros1_bridge
source ${ROS1_INSTALL_PATH}/setup.bash; source ${ROS2_INSTALL_PATH}/setup.bash
source ~/ros2_ws_ros1_bridge/install/setup.bash
export ROS_MASTER_URI=http://localhost:11311
ros2 run ros1_bridge dynamic_bridge
```

Terminal 3

```
# Shell C:
source ${ROS1_INSTALL_PATH}/setup.bash
rosrun rospy_tutorials talker
```

Terminal 4

```
source ${ROS2_INSTALL_PATH}/setup.bash
ros2 run demo_nodes_cpp listener
```


# Example Ros2 USBcam - Ros1 Bridge - Ros1 ShowImage


Terminal 1 ros1_core:

```
source $ROS1_INSTALL_PATH/setup.bash
roscore
```

Terminal 2 ros2_usbcam:

```
source ${ROS2_INSTALL_PATH}/setup.bash
ros2 run image_tools cam2image --ros-args -r /image:=/camera/image_raw
```


Terminal 3 ros1_bridge:

```
cd ~/ros2_ws_ros1_bridge
source ${ROS1_INSTALL_PATH}/setup.bash; source ${ROS2_INSTALL_PATH}/setup.bash
source ~/ros2_ws_ros1_bridge/install/setup.bash
export ROS_MASTER_URI=http://localhost:11311
ros2 run ros1_bridge dynamic_bridge
```

Terminal 4 ros1_image_show:

```
source $ROS1_INSTALL_PATH/setup.bash
rosrun rqt_image_view rqt_image_view /camera/image_raw
```




## ROS1, 2, 1-2 dir

peerajak@ryzen9:~/ros1_ws/src/Checkpoint25_final_project_ros1_and_ros1_bridge$ ls -1
async_web_server_cpp
course_web_dev_ros
launch_cp25_ros1
README.md
rg2_gripper_description
robotnik_sensors
ros1_bridge.tar.gz
rosauth
rosbridge_suite
tf2_web_republisher
Universal_Robots_ROS2_Description
web_video_server

------

peerajak@ryzen9:~/ros2_ws_ros1_bridge/src$ ls -l
ros1_bridge

------
peerajak@ryzen9:~/ros2_ws/src$ ls -1
build
Checkpoint25_final_project
install
launch_param_builder
log
move_group.launch.py
moveit2
moveit2_tutorials
moveit_resources
moveit_task_constructor
moveit_visual_tools
rosparam_shortcuts
ros_tutorials
srdfdom
universal_robot_ros2
vision_opencv



## To change the sim robot's camera position 

change the followin to files 
~/ros2_ws/src/universal_robot_ros2/Universal_Robots_ROS2_Description/urdf/ur.urdf.xacro
~/ros2_ws/src/Checkpoint25_final_project/cp25_webapp/Universal_Robots_ROS2_Description/urdf/ur.urdf.xacro

at line 109

   <xacro:sensor_r430 prefix="wrist_rgbd" parent="base_link" >
       <origin xyz="-0.3 -0.4 0.3" rpy="0 ${7*pi/18} ${pi/2}"/>
   </xacro:sensor_r430>

your proper xyz rpy

and

~/ros1_ws/src/Checkpoint25_final_project_ros1_and_ros1_bridge/Universal_Robots_ROS2_Description/urdf/ur.urdf.xacro

at line 116

   <xacro:if value="${is_sim_robot}">
       <xacro:sensor_r430 prefix="wrist_rgbd" parent="base_link">
        <origin xyz="-0.3 -0.4 0.3" rpy="0 ${7*pi/18} ${pi/2}"/>
       </xacro:sensor_r430>
   </xacro:if> <!-- peerajak this is testing value-->

your proper xyz rpy


