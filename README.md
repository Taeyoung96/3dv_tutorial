## How to build 3dv_tutorial repository in Ubuntu 18.04  

### 0. Install Ceres-Solver version 1.14.0  

먼저 [Ceres-solver](https://github.com/ceres-solver/ceres-solver/releases) 페이지에서 버전 `1.14.0`을 `tar.gz`를 클릭하여 다운받습니다.  

터미널에서 경로를 `~$`으로 만들어 놓은 다음,  
```
mkdir ceres-solver-1.14.0
cd ceres-solver-1.14.0
```  

경로를 `~/ceres-solver-1.14.0$`로 바꾸어 놓은 후, 받아놓은 `tar.gz`를 현재 경로로 옮겨놓습니다.  

다음 터미널 창에 차례대로 command를 입력합니다.  

```
tar zxf ceres-solver-1.14.0.tar.gz
mkdir ceres-bin
cd ceres-bin
cmake ../ceres-solver-1.14.0 -DCMAKE_INSTALL_PREFIX=~/ceres-solver-1.14.0
make -j3
make test
make install
```

이제 Ceres-solver를 다운받았습니다.  


### 1. Required Libraries  

|Library|Version|  
|:---:|:---:|  
|OpenCV|3.4.5|
|Ceres-solver|1.14.0|
|CMake|3.15.6|  

Ceres-solver는 버전 2가 넘어가면 안된다는 Issue가 있었습니다.  


### 2. Build this repository  

필요한 라이브러리를 다 받으면 이 repository에 있는 `CMakeLists.txt`를 다음과 같이 수정해주어야 합니다.  

먼저 `3dv_tutorial` 폴더 안으로 들어가, `CMakeLists.txt`를 클릭하고 수정해주세요.  

```cmake
cmake_minimum_required(VERSION 2.6)

project(3dv_tutorial)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BUILD_TYPE Release)
find_package(OpenCV REQUIRED)
find_package(Ceres REQUIRED)

include_directories( 
    ${OpenCV_INCLUDE_DIRS} 
    ${CERES_INCLUDE_DIRS}
)

set(SRC_DIR	"${CMAKE_SOURCE_DIR}/src")
set(BIN_DIR	"${CMAKE_SOURCE_DIR}/bin")

file(GLOB APP_SOURCES "${SRC_DIR}/*.cpp")
foreach(app_source ${APP_SOURCES})
    string(REPLACE ".cpp" "" app_name ${app_source})
    string(REPLACE "${SRC_DIR}/" "" app_name ${app_name})
    add_executable(${app_name} ${app_source})
    target_link_libraries(${app_name} ${OpenCV_LIBS} ${CERES_LIBRARIES})
    install(TARGETS ${app_name} DESTINATION ${BIN_DIR})
endforeach(app_source ${APP_SOURCES})
```  

참고 Issue : [Issue #6](https://github.com/sunglok/3dv_tutorial/issues/6)  

다음 빌드를 진행합니다.  

경로를 `~/3dv_tutorial$`로 바꾼후,  
```
mkdir build
cd build
cmake ..
make -j4
make install
```

빌드가 완료되면 `bin` 폴더에 여러 실행파일이 생긴 것을 볼 수가 있습니다.  

이제 터미널에서 실행을 시키면 됩니다!  

ex) `./vo_epipolar`  




---

## Original Readme.md

## An Invitation to 3D Vision: A Tutorial for Everyone
_An Invitation to 3D Vision_ is an introductory tutorial on 3D vision (a.k.a. geometric vision or visual geometry or multi-view geometry).
It aims to make beginners understand basic theory of 3D vision and implement their own applications using [OpenCV][].
In addition to tutorial slides, example codes are provided in the purpose of education. They include simple but interesting and practical applications. The example codes are written as short as possible (mostly __less than 100 lines__) to be clear and easy to understand.

* Download [tutorial slides](https://github.com/sunglok/3dv_tutorial/releases/download/misc/3dv_slides.pdf)
* Download [example codes in a ZIP file](https://github.com/sunglok/3dv_tutorial/archive/master.zip)
* Read [how to run example codes](https://github.com/sunglok/3dv_tutorial/blob/master/HOWTO_RUN.md)

### What does its name come from?
* The main title, _An Invitation to 3D Vision_, came from [a legendary book by Yi Ma, Stefano Soatto, Jana Kosecka, and Shankar S. Sastry](http://vision.ucla.edu/MASKS/). We wish that our tutorial will be the first gentle invitation card for beginners to 3D vision and its applications.
* The subtitle, _for everyone_, was inspired from [Prof. Kim's online lecture](https://hunkim.github.io/ml/) (in Korean). Our tutorial is also intended not only for students and researchers in academia, but also for hobbyists and developers in industries. We tried to describe important and typical problems and their solutions in [OpenCV][]. We hope readers understand it easily without serious mathematical background.

### Examples
* __Single-view Geometry__
  * Camera Projection Model
    * Object Localization and Measurement: [object_localization.cpp][] (result: [image](https://drive.google.com/open?id=10Lche-1HHazDeohXEQK443ruDTAmIO4E))
    * Image Formation: [image_formation.cpp][] (result: [image0](https://drive.google.com/file/d/0B_iOV9kV0whLY2luc05jZGlkZ2s/view), [image1](https://drive.google.com/file/d/0B_iOV9kV0whLS3M4S09ZZHpjTkU/view), [image2](https://drive.google.com/file/d/0B_iOV9kV0whLV2dLZHd0MmVkd28/view), [image3](https://drive.google.com/file/d/0B_iOV9kV0whLS1ZBR25WekpMYjA/view), [image4](https://drive.google.com/file/d/0B_iOV9kV0whLYVB0dm9Fc0dvRzQ/view))
    * Geometric Distortion Correction: [distortion_correction.cpp][] (result: [video](https://www.youtube.com/watch?v=HKetupWh4V8))
  * General 2D-3D Geometry
    * Camera Calibration: [camera_calibration.cpp][] (result: [text](https://drive.google.com/file/d/0B_iOV9kV0whLZ0pDbWdXNWRrZ00/view))
    * Camera Pose Estimation (Chessboard): [pose_estimation_chessboard.cpp][] (result: [video](https://www.youtube.com/watch?v=4nA1OQGL-ig))
    * Camera Pose Estimation (Book): [pose_estimation_book1.cpp][]
    * Camera Pose Estimation and Calibration: [pose_estimation_book2.cpp][]
    * Camera Pose Estimation and Calibration w/o Initially Given Camera Parameters: [pose_estimation_book3.cpp][] (result: [video](https://www.youtube.com/watch?v=GYp4h0yyB3Y))
* __Two-view Geometry__
  * Planar 2D-2D Geometry (Projective Geometry)
    * Perspective Distortion Correction: [perspective_correction.cpp][] (result: [original](https://drive.google.com/file/d/0B_iOV9kV0whLVlFpeFBzYWVadlk/view), [rectified](https://drive.google.com/file/d/0B_iOV9kV0whLMi1UTjN5QXhnWFk/view))
    * Planar Image Stitching: [image_stitching.cpp][] (result: [image](https://drive.google.com/file/d/0B_iOV9kV0whLOEQzVmhGUGVEaW8/view))
    * 2D Video Stabilization: [video_stabilization.cpp][] (result: [video](https://www.youtube.com/watch?v=be_dzYicEzI))
  * General 2D-2D Geometry (Epipolar Geometry)
    * Visual Odometry (Monocular, Epipolar Version): [vo_epipolar.cpp][]
    * Triangulation (Two-view Reconstruction): [triangulation.cpp][]
* __Multi-view Geometry__
  * Bundle Adjustment
    * Global Version: [bundle_adjustment_global.cpp][]
    * Incremental Version: [bundle_adjustment_inc.cpp][]
  * Structure-from-Motion
    * Global SfM: [sfm_global.cpp][]
    * Incremental SfM: [sfm_inc.cpp][]
  * Feature-based Visual Odometry and SLAM
    * Visual Odometry (Monocular, Epipolar Version): [vo_epipolar.cpp][]
    * Visual Odometry (Stereo Version)
    * Visual Odometry (Monocular, PnP and BA Version)
    * Visual SLAM (Monocular Version)
  * Direct Visual Odometry and SLAM
    * Visual Odometry (Monocular, Direct Version)
  * c.f. The above examples need [Ceres Solver][] for bundle adjustment.
* __Correspondence Problem__
  * Line Fitting with RANSAC: [line_fitting_ransac.cpp][]
  * Line Fitting with M-estimators: [line_fitting_m_est.cpp][]
* **Appendix**
  * Line Fitting
  * Planar Homograph Estimation
  * Fundamental Matrix Estimation

### Dependencies
* [OpenCV][] (> 3.0.0, 3-clause BSD License)
  * _OpenCV_ is a base of all example codes for basic computer vision algorithms, linear algebra, image/video manipulation, and GUI.
* [Ceres Solver][] (3-clause BSD License): A numerical optimization library
  * _Ceres Solver_ is additionally used by m-estimator, bundle adjustment, structure-from-motion, and visual odometry/SLAM.

### License
* [Beerware](http://en.wikipedia.org/wiki/Beerware)

### Authors
* [Sunglok Choi](http://sites.google.com/site/sunglok/) (sunglok AT hanmail DOT net)

### Acknowledgement
The authors thank the following contributors and projects.

* [Jae-Yeong Lee](https://sites.google.com/site/roricljy/): He motivated many examples.
* [Giseop Kim](https://sites.google.com/view/giseopkim): He contributed the initial version of SfM codes with [Toy-SfM](https://github.com/royshil/SfM-Toy-Library) and [cvsba](https://www.uco.es/investiga/grupos/ava/node/39).
* [The KITTI Vision Benchmark Suite](http://www.cvlibs.net/datasets/kitti/): The KITTI odometry dataset #07 was used to demonstrate visual odometry and SLAM.
* [Russell Hewett](https://courses.engr.illinois.edu/cs498dh3/fa2013/projects/stitching/ComputationalPhotograph_ProjectStitching.html): His two hill images were used to demonstrate image stitching.
* [Kang Li](http://www.cs.cmu.edu/~kangli/code/Image_Stabilizer.html): His shaking CCTV video was used to demonstrate video stabilization.
* [Richard Blais](http://www.richardblais.net/): His book cover and video in [the OpenCV tutorial](http://docs.opencv.org/3.1.0/dc/d16/tutorial_akaze_tracking.html) were used to demonstrate camera pose estimation and augmented reality.

[OpenCV]: http://opencv.org/
[Ceres Solver]: http://ceres-solver.org/

[object_localization.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/object_localization.cpp
[image_formation.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/image_formation.cpp
[distortion_correction.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/distortion_correction.cpp
[camera_calibration.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/camera_calibration.cpp
[pose_estimation_chessboard.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/pose_estimation_chessboard.cpp
[pose_estimation_book1.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/pose_estimation_book1.cpp
[pose_estimation_book2.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/pose_estimation_book2.cpp
[pose_estimation_book3.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/pose_estimation_book3.cpp
[perspective_correction.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/perspective_correction.cpp
[image_stitching.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/image_stitching.cpp
[video_stabilization.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/video_stabilization.cpp
[vo_epipolar.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/vo_epipolar.cpp
[triangulation.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/triangulation.cpp
[bundle_adjustment_global.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/bundle_adjustment_global.cpp
[bundle_adjustment_inc.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/bundle_adjustment_inc.cpp
[sfm_global.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/sfm_global.cpp
[sfm_inc.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/sfm_inc.cpp
[line_fitting_ransac.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/line_fitting_ransac.cpp
[line_fitting_m_est.cpp]: https://github.com/sunglok/3dv_tutorial/blob/master/src/line_fitting_m_est.cpp
