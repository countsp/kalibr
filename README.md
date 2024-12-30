![Kalibr](https://raw.githubusercontent.com/wiki/ethz-asl/kalibr/images/kalibr_small.png)

[![ROS1 Ubuntu 20.04](https://github.com/ethz-asl/kalibr/actions/workflows/docker_2004_build.yaml/badge.svg)](https://github.com/ethz-asl/kalibr/actions/workflows/docker_2004_build.yaml)
[![ROS1 Ubuntu 18.04](https://github.com/ethz-asl/kalibr/actions/workflows/docker_1804_build.yaml/badge.svg)](https://github.com/ethz-asl/kalibr/actions/workflows/docker_1804_build.yaml)
[![ROS1 Ubuntu 16.04](https://github.com/ethz-asl/kalibr/actions/workflows/docker_1604_build.yaml/badge.svg)](https://github.com/ethz-asl/kalibr/actions/workflows/docker_1604_build.yaml)

## Introduction
Kalibr is a toolbox that solves the following calibration problems:

1. **Multi-Camera Calibration**: Intrinsic and extrinsic calibration of a camera-systems with non-globally shared overlapping fields of view with support for a wide range of [camera models](https://github.com/ethz-asl/kalibr/wiki/supported-models).
1. **Visual-Inertial Calibration (CAM-IMU)**: Spatial and temporal calibration of an IMU w.r.t a camera-system along with IMU intrinsic parameters
1. **Multi-Inertial Calibration (IMU-IMU)**: Spatial and temporal calibration of an IMU w.r.t a base inertial sensor along with IMU intrinsic parameters (requires 1-aiding camera sensor).
1. **Rolling Shutter Camera Calibration**: Full intrinsic calibration (projection, distortion and shutter parameters) of rolling shutter cameras.

To install follow the [install wiki page](https://github.com/ethz-asl/kalibr/wiki/installation) instructions for which you can either use Docker or install from source in a ROS workspace.
Please find more information on the [wiki pages](https://github.com/ethz-asl/kalibr/wiki) of this repository.
For questions or comments, please open an issue on Github.

# 教程 

https://download.csdn.net/blog/column/10095893/107353480


**camchain.yaml文件内容说明：**

```
CAMERA_MODEL
camera_model（pinhole / omni）（针孔、全向）

intrinsics
包含给定投影类型的内部参数的向量。要素如下：
pinhole：[fu fv pu pv]
omn​​i：[xi fu fv pu pv]
ds：[xi alpha fu fv pu pv]
eucm：[alpha beta fu fv pu pv]

distortion_model
distortion_model（radtan /equidistant）

distortion_coeffs
失真模型的参数向量

T_cn_cnm1
相机外在转换，总是相对于链中的最后一个相机
（例如cam1：T_cn_cnm1 = T_c1_c0，将cam0转换为cam1坐标）

T_cam_imu
IMU extrinsics：从IMU到相机坐标的转换（T_c_i）

timeshift_cam_imu
相机和IMU时间戳之间的时间间隔，以秒为单位（t_imu = t_cam + shift）

rostopic
摄像机图像流的主题

resolution
相机分辨率[width,height]
```

