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
### 相机内参中的 **K** 和 **D**

在相机的内参中，**K** 和 **D** 是描述相机光学特性的重要矩阵和向量，它们直接影响成像的几何效果和校正。以下是详细解释：

---

### **1. 相机内参矩阵 \( K \)**

**K** 是相机内参矩阵，用于描述相机的固有参数，即光学和几何特性。这是一个 \( 3 \times 3 \) 的矩阵，形式如下：

\[
K = 
\begin{bmatrix}
f_x & 0 & c_x \\
0 & f_y & c_y \\
0 & 0 & 1
\end{bmatrix}
\]

其中：
- **\( f_x, f_y \)**：表示相机的焦距，分别在 x 和 y 方向的像素尺度上的焦距（通常不同是因为像素可能非正方形）。
- **\( c_x, c_y \)**：表示主点（principal point）的位置，也即图像的光学中心，通常以像素为单位。
- **1**：是齐次坐标系的缩放因子。

#### **影响效果：**
- 焦距 \( f_x, f_y \)：决定了图像的视场（Field of View, FOV）。焦距越大，视场越窄，图像看起来更“放大”；焦距越小，视场越宽，图像覆盖范围更大。
- 主点 \( c_x, c_y \)：如果与图像中心不重合，会导致几何偏移，使得图像有非对称的畸变。

---

### **2. 畸变系数 \( D \)**

**D** 是畸变系数向量，用来描述相机镜头的非线性失真。常见的失真类型包括径向畸变和切向畸变。

典型的 \( D \) 表示为：
\[
D = [k_1, k_2, p_1, p_2, k_3, \dots]
\]

- **\( k_1, k_2, k_3 \)**：径向畸变系数，用于描述图像从中心向边缘的放大或缩小。 
  - **正畸变**（桶形失真，Barrel Distortion）：边缘向外弯曲。
  - **负畸变**（枕形失真，Pincushion Distortion）：边缘向内弯曲。
- **\( p_1, p_2 \)**：切向畸变系数，描述镜头与图像平面不平行时的失真。

#### **影响效果：**
- **径向畸变**：图像会显得弯曲，特别是在广角镜头下边缘部分失真显著。
- **切向畸变**：图像的形状会有非对称性，例如某些部分看起来拉长或压缩。

---

### **总结**

1. **K（内参矩阵）**：影响成像的几何比例、视角和中心点位置。
2. **D（畸变系数）**：影响图像的非线性畸变，主要在广角或廉价镜头中更明显。

通过相机标定（Calibration），可以确定 \( K \) 和 \( D \)，并使用这些参数进行畸变校正（Distortion Correction）和图像校准（Image Rectification），从而生成几何上更准确的图像。
