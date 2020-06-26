
# CORB-SLAM

**Authors:** Fu Li(lifu11@nudt.edu.cn),Shaowu Yang(shaowu.yang@nudt.edu.cn)

**Current version:** 1.0.0

## introduction

**CORB-SLAM(collaborative ORB-SLAM)** is a centralized multi-robot visual SLAM system based on  **[ORB_SLAM2[1-2]](https://github.com/raulmur/ORB_SLAM2)**.
The ORB-SLAM2 is a versatile and accurate visual SLAM method that has been popularly applied in single robot applications. However, this method cannot provide support to multi-robot cooperation in environmental mapping.
The CORB-SLAM system consists of multiple ORB_SLAM2 clients for local mapping and a central server for global map fusion. Specifically, we extend each of the ORB_SLAM2 clients with a memory managing module that organizes the local map and communicates with the central server. In the central server, we detect the overlaps of multiple local maps by the DBoW method, and fuse these maps by utilizing the PnP method and global optimization through bundle adjustment.

<!-- <div align=center> <img src="https://github.com/lifunudt/M2SLAM/blob/master/images/framework.png" alt="M2SLAM" height="180" align=center /> </div> -->

## 0. Related Publications

[1] F. Li, S. Yang, X. Yi, X. Yang. CORB-SLAM: a Collaborative Visual SLAM System for Multiple Robots. submitted.

## 1. Prerequisites

### 1.0 requirements
  * ubuntu 14.04/16.04
  * ROS indigo/kinetic
  * ORBSLAM2
  * boost
  * pcl-1.7
  * OpenCV >= 2.4.3
  * Eigen3 >= 3.1
  * Pangolin
  * g2o
  * DBoW2

### 1.1 ROS install

Inatall ROS kinetic according to the instructions in [ROS wiki](http://wiki.ros.org/indigo/Installation).

### 1.2 ORBSLAM2 and its dependencies

CORB-SLAM system is build on the foundation of [ORB_SLAM2(https://github.com/raulmur/ORB_SLAM2). 

### 1.3 Boost library install
Boost library is as to serialize and deserialize the data.

```bash
sudo apt-get install libboost-all-dev
```

## 2. Compile

```
cd CORB-SLAM
cd src/corbslam_client/Thirdparty/DBoW2
mkdir build && cd build && cmake .. && make
cd ../../g2o/
mkdir build && cd build && cmake .. && make
cd ../../../../../
catkin_make
```

 input the following command in `~/.bashrc` , and the PATH/ is the absolute path of CORB-SLAM.

```
source PATH/CORB-SLAM/devel/setup.bash
```

## 3. Run

#### 3.1 start ros core
```
roscore
```
#### 3.2 start corbslam_servcer
```
rosrun corbslam_servcer PATH_TO_VOCABULARY/ORBvoc.txt PATH_TO_CONFIGURATION/KITTIX.yaml
```
#### 3.3 run multiple corbslam_client in different datasets

1. run KITTI datasets

Download the dataset (grayscale images) from http://www.cvlibs.net/datasets/kitti/eval_odometry.php

Execute the following command. Change `KITTIX.yaml`to KITTI00-02.yaml, KITTI03.yaml or KITTI04-12.yaml for sequence 0 to 2, 3, and 4 to 12 respectively. Change `PATH_TO_DATASET_FOLDER` to the uncompressed dataset folder. Change `SEQUENCE_NUMBER` to 00, 01, 02,.., 11.

```
rosrun corbslam_client_stereo_kitti corbslam_client_stereo_kitti Vocabulary/ORBvoc.txt Examples/Stereo/KITTIX.yaml PATH_TO_DATASET_FOLDER CLIENT_ID
```

## Reference
[1] Mur-Artal R, Montiel J M M, Tardos J D. ORB-SLAM: a versatile and accurate monocular SLAM system[J]. IEEE Transactions on Robotics, 2015, 31(5): 1147-1163.

[2] Mur-Artal R, Tardos J D. ORB-SLAM2: an Open-Source SLAM System for Monocular, Stereo and RGB-D Cameras[J]. arXiv preprint arXiv:1610.06475, 2016.

## License
CORB-SLAM is released under a [GPLv3 license](https://github.com/lifunudt/M2SLAM/blob/master/License-gpl.txt).

For a closed-source version of M2SLAM for commercial purposes, please contact the authors.
