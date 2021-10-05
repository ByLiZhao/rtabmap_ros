rtabmap_ros [![Build Status](https://github.com/introlab/rtabmap_ros/actions/workflows/ros2.yml/badge.svg)](https://github.com/introlab/rtabmap_ros/actions/workflows/ros2.yml)
===========

RTAB-Map's ROS2 package (branch `ros2`). **UNDER CONSTRUCTION**: currently most nodes are ported to ROS2, however they are not all tested yet. The interface is the same than on ROS1 (parameters and topic names should still match ROS1 documentation on [rtabmap_ros](http://wiki.ros.org/rtabmap_ros)). 

`rtabmap.launch` is also ported to ROS2 with same arguments. If you see [ROS1 examples](http://wiki.ros.org/rtabmap_ros/Tutorials/HandHeldMapping) like this:

```bash
$ roslaunch zed_wrapper zed_no_tf.launch

$ roslaunch rtabmap_ros rtabmap.launch \
    rtabmap_args:="--delete_db_on_start" \
    rgb_topic:=/zed/zed_node/rgb/image_rect_color \
    depth_topic:=/zed/zed_node/depth/depth_registered \
    camera_info_topic:=/zed/zed_node/rgb/camera_info \
    frame_id:=base_link \
    approx_sync:=false
```

The ROS2 equivalent is (with those [lines](https://github.com/stereolabs/zed-ros2-wrapper/blob/b512dce6ad4565f4770273995b147122e735ca0f/zed_wrapper/config/common.yaml#L58-L60) set to false to avoid TF conflicts):

```bash
$ ros2 launch zed_wrapper zed.launch.py

$ ros2 launch rtabmap_ros rtabmap.launch.py \
    rtabmap_args:="--delete_db_on_start" \
    rgb_topic:=/zed/zed_node/rgb/image_rect_color \
    depth_topic:=/zed/zed_node/depth/depth_registered \
    camera_info_topic:=/zed/zed_node/rgb/camera_info \
    frame_id:=base_link \
    approx_sync:=false
```


# Installation 

* RTAB-Map ROS2 package:
    ```bash
    $ cd ~/ros2_ws
    $ git clone https://github.com/introlab/rtabmap.git src/rtabmap
    $ git clone --branch ros2 https://github.com/introlab/rtabmap_ros.git src/rtabmap_ros
    $ export MAKEFLAGS="-j6" # Can be ignored if you have a lot of RAM
    $ colcon build 
    ```

# Example with Turtlebot3

1. Launch Turtlebot3 simulator:
    ```bash
    $ export TURTLEBOT3_MODEL=waffle
    $ ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
    
    $ export TURTLEBOT3_MODEL=waffle
    $ ros2 run turtlebot3_teleop teleop_keyboard
    ```

2. Launch RTAB-Map:
    ```
    $ ros2 launch rtabmap_ros turtlebot3_scan.launch.py
    
    # OR with rtabmap.launch.py
    $ ros2 launch rtabmap_ros rtabmap.launch.py \
       visual_odometry:=false \
       frame_id:=base_footprint \
       subscribe_scan:=true depth:=false \
       approx_sync:=true \
       odom_topic:=/odom \
       scan_topic:=/scan \
       args:="-d --RGBD/NeighborLinkRefining true --Reg/Strategy 1" \
       use_sim_time:=true
    ```
See [launch/ros2](https://github.com/introlab/rtabmap_ros/tree/ros2/launch/ros2) subfolder for some other ROS2 examples with turtlebot3 in simulation and a RGB-D camera.
