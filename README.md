# loam-livox-converter

## Example Dataset: 

Download the dataset from [Bunker DVI Dataset](https://charleshamesse.github.io/bunker-dvi-dataset/)  

## Intended use 

This small toolset allows to integrate SLAM solution provided by [loam-livox](https://github.com/hku-mars/loam_livox/) with [HDMapping](https://github.com/MapsHD/HDMapping).
This repository contains ROS 1 workspace that :
  - submodule to tested revision of Loam livox
  - a converter that listens to topics advertised from odometry node and save data in format compatible with HDMapping.

## Dependencies

```shell
sudo apt install -y nlohmann-json3-dev
```

## Modified for build


**Changes made:**

 
   ```
  File:

  test_ws/src/loam-livox-to-hdmapping/src/loam_livox/source/read_camera.cpp

    cap.set(CV_CAP_PROP_SETTINGS, 1); //opens camera properties dialog
    cap.set( CV_CAP_PROP_FRAME_WIDTH, 320 );
    cap.set( CV_CAP_PROP_FRAME_HEIGHT, 240 );

    CHANGED TO:

    cap.set(cv::CV_CAP_PROP_SETTINGS, 1); //opens camera properties dialog
    cap.set( cv::CV_CAP_PROP_FRAME_WIDTH, 320 );
    cap.set( cv::CV_CAP_PROP_FRAME_HEIGHT, 240 );

    test_ws/src/loam-livox-to-hdmapping/src/loam_livox/source/laser_feature_extractor.hpp

    #include <opencv/cv.h>

    CHANGED TO:

    #include <opencv2/opencv.hpp>

## Building

Clone the repo
```shell
mkdir -p /test_ws/src
cd /test_ws/src
git clone https://github.com/marcinmatecki/loam-livox-to-hdmapping --recursive
cd ..
catkin_make
```

## Usage - data SLAM:

Prepare recorded bag with estimated odometry:

In first terminal record bag:
```shell
rosbag record /velodyne_cloud_registered /aft_mapped_to_init
```

and start odometry:
```shell 
cd /test_ws/
source ./install/setup.sh # adjust to used shell
For launch,check https://github.com/hku-mars/loam_livox/
```

## Usage - conversion:

```shell
cd /test_ws/
source ./install/setup.sh # adjust to used shell
rosrun loam-to-hdmapping listener <recorded_bag> <output_dir>
```

## Record the bag file:

```shell
rosbag record /velodyne_cloud_registered /aft_mapped_to_init -O {your_directory_for_the_recorded_bag}
```

## LOAM Launch:

```shell
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
roslaunch loam_livox livox.launch
rosbag play sequence.bag --clock /pp_points/synced2rgb:=/livox/lidar
```

## During the record (if you want to stop recording earlier) / after finishing the bag:

```shell
In the terminal where the ros record is, interrupt the recording by CTRL+C
Do it also in ros launch terminal by CTRL+C.
```

## Usage - Conversion (ROS bag to HDMapping, after recording stops):

```shell
cd /test_ws/
source ./devel/setup.sh # adjust to used shell
rosrun loam-to-hdmapping listener <recorded_bag> <output_dir>
```
