# ORB-SLAM2
#### Current version: 1.0.0

ORB-SLAM2 is a real-time SLAM library for **Monocular**, **Stereo** and **RGB-D** cameras, that computes the camera trajectory and a sparse 3D reconstruction (in the stereo and RGB-D case with true scale). It is able to detect loops and relocalize the camera in real time. We provide examples to run the system in the [KITTI dataset](http://www.cvlibs.net/datasets/kitti/eval_odometry.php) as stereo or monocular, and in the [TUM dataset](http://vision.in.tum.de/data/datasets/rgbd-dataset) as RGB-D or monocular. We also provide a ROS node to process live monocular or RGB-D streams. **The library can be compiled without ROS**. ORB-SLAM2 provides an interface to change between *SLAM Mode* and *Localization Mode* (only camera tracking, no map building).

**Notice for ORB-SLAM Monocular users:**
The monocular capabilities of ORB-SLAM2 compared to [ORB-SLAM Monocular](https://github.com/raulmur/ORB_SLAM), are similar. However in ORB-SLAM2 we apply a full bundle adjustment after a loop closure, the extraction of ORB is slightly different (trying to improve the dispersion on the image) and the tracking is also slightly faster. The interface of ORB-SLAM2 also provides you new capabilities as the *modes* mentioned above and a reset button. We recommend you to try this new software :)

###Related Publications:

[1] Raúl Mur-Artal, J. M. M. Montiel and Juan D. Tardós. **ORB-SLAM: A Versatile and Accurate Monocular SLAM System**. *IEEE Transactions on Robotics,* vol. 31, no. 5, pp. 1147-1163, 2015.

Link to pdf: http://webdiis.unizar.es/~raulmur/MurMontielTardosTRO15.pdf

#1. License

ORB-SLAM2 is released under a [GPLv3 license](https://github.com/raulmur/ORB_SLAM2/blob/master/License-gpl.txt). For a list of all code/library dependencies (and associated licenses), please see [Dependencies.md](https://github.com/raulmur/ORB_SLAM2/blob/master/Dependencies.md).

For a closed-source version of ORB-SLAM2 for commercial purposes, please contact the authors.

If you use ORB-SLAM2 in an academic work, please cite:

    @article{murAcceptedTRO2015,
      title={{ORB-SLAM}: a Versatile and Accurate Monocular {SLAM} System},
      author={Mur-Artal, Ra\'ul, Montiel, J. M. M. and Tard\'os, Juan D.},
      journal={IEEE Transactions on Robotics},
      volume={31},
      number={5},
      pages={1147--1163},
      doi = {10.1109/TRO.2015.2463671},
      year={2015}
     }

#2. Prerequisites

## 2.0 C++11 or C++0x Compiler
We use the new thread and chrono functions of C++11.

## 2.1 Pangolin
We use [Pangolin](https://github.com/stevenlovegrove/Pangolin) for visualization and user interface. Dowload and install instructions can be found at: https://github.com/stevenlovegrove/Pangolin.

##2.2 OpenCV
We use [OpenCV](http://opencv.org) to manipulate images and features. Dowload and install instructions can be found at: http://opencv.org. **Required at leat 2.4.3. Tested with OpenCV 2.4.11**.

##2.3 Eigen3
Required by g2o (see below). Download and install instructions can be found at: http://eigen.tuxfamily.org. **Required at least 3.1.0**.

##2.4 DBoW2 and g2o (Included in Thirdparty folder)
We use modified versions of the [DBoW2](https://github.com/dorian3d/DBoW2) library to perform place recognition and [g2o](https://github.com/RainerKuemmerle/g2o) library to perform non-linear optimizations. Both modified libraries (which are both BSD) are included in the Thirdparty folder.

##2.5 ROS (optional)
We provide some examples to process the live input of a monocular, stereo or RGB-D camera using [ROS](ros.org). Building these examples is optional. In case you want to use ROS, a version Hydro or newer is needed.

#3. Building ORB-SLAM2 library and TUM/KITTI examples

Clone the repository:
```
git clone https://github.com/ProjectARCeth/ORB_SLAM2.git
```
(I included it now in the catkin_ws/src)

We provide a script `build.sh` to build the *Thirdparty* libraries and *ORB-SLAM2*. You will need to give first execute permissions. At ORB_SLAM2 directory, execute:
```
cd ORB_SLAM2
chmod +x build.sh
./build.sh
```

This will create **libORB_SLAM.so**  at *lib* folder and the executables **mono_tum**, **mono_kitti**, **rgbd_tum**, **stereo_kitti** in *Examples* folder.

#4. ROS (Main Thing that we will work on!)

#4.1 Basics with ROS

First you should unpack the Vocabulary in ORB_SLAM2/Examples/ROS/orb_slam2/Vocabulary

1. Add the path including *Examples/ROS/ORB_SLAM2* to the ROS_PACKAGE_PATH environment variable. Open .bashrc file and add at the end the following line. Replace PATH by the folder where you cloned ORB_SLAM2:

  ```
  export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:PATH/ORB_SLAM2/Examples/ROS
  ```

2. Relaunch the Terminal in order that the exported path is included

3. Since the package is catkinized just run:

  ```
  catkin build orb_slam2
  ```

4. For a monocular input from topic `/camera/image_raw` run node orb_slam2/mono. You will need to provide the vocabulary file and a settings file. See the monocular examples above.

  ```
  rosrun orb_slam2 mono PATH_TO_VOCABULARY PATH_TO_SETTINGS_FILE
  ```

5. For a stereo input from topics `/camera/left/image_raw` and `/camera/right/image_raw` run node orb_slam2/stereo. You will need to provide the vocabulary file and a settings file. See the stereo examples above.

  ```
  rosrun orb_slam2 stereo PATH_TO_VOCABULARY PATH_TO_SETTINGS_FILE
  ```

6. For an RGB-D input from topics `/camera/rgb/image_raw` and `/camera/depth_registered/image_raw`, run node orb_slam2/rgbd. You will need to provide the vocabulary file and a settings file. See the RGB-D example above.

  ```
  rosrun orb_slam2 rgbd PATH_TO_VOCABULARY PATH_TO_SETTINGS_FILE
  ```

#4.2 Properties when modifying:

If you change standart files which are not in Examples/ROS/orb_slam2 then in order to apply changes you have to navigate to the path of ORB_SLAM2 and then do:
  ```
  sh build.sh
  ```

Else if you change something in Examples/ROS/orb_slam2 then you apply the changes with:
  ```
  catkin build orb_slam2
  ```

#5. Monocular Examples

##5.1 TUM Dataset

1. Download a sequence from http://vision.in.tum.de/data/datasets/rgbd-dataset/download and uncompress it.

2. Execute the following command. Change `TUMX.yaml` to TUM1.yaml,TUM2.yaml or TUM3.yaml for freiburg1, freiburg2 and freiburg3 sequences respectively. Change `PATH_TO_SEQUENCE_FOLDER`to the uncompressed sequence folder.
```
./Examples/Monocular/mono_tum Vocabulary/ORBvoc.txt Examples/Monocular/TUMX.yaml PATH_TO_SEQUENCE_FOLDER
```

##5.2 KITTI Dataset

1. Download the dataset (grayscale images) from http://www.cvlibs.net/datasets/kitti/eval_odometry.php

2. Execute the following command. Change `KITTIX.yaml`by KITTI00-02.yaml, KITTI03.yaml or KITTI04-12.yaml for sequence 0 to 2, 3, and 4 to 12 respectively. Change `PATH_TO_DATASET_FOLDER` to the uncompressed dataset folder. Change `SEQUENCE_NUMBER` to 00, 01, 02,.., 11.
```
./Examples/Monocular/mono_kitti Vocabulary/ORBvoc.txt Examples/Monocular/KITTIX.yaml PATH_TO_DATASET_FOLDER/dataset/sequences/SEQUENCE_NUMBER
```

#6. Stereo Example

##6.1 KITTI Dataset

1. Download the dataset (grayscale images) from http://www.cvlibs.net/datasets/kitti/eval_odometry.php

2. Execute the following command. Change `KITTIX.yaml`to KITTI00-02.yaml, KITTI03.yaml or KITTI04-12.yaml for sequence 0 to 2, 3, and 4 to 12 respectively. Change `PATH_TO_DATASET_FOLDER` to the uncompressed dataset folder. Change `SEQUENCE_NUMBER` to 00, 01, 02,.., 11.
```
./Examples/Stereo/stereo_kitti Vocabulary/ORBvoc.txt Examples/Stereo/KITTIX.yaml PATH_TO_DATASET_FOLDER/dataset/sequences/SEQUENCE_NUMBER
```

#7. RGB-D Example

##7.1 TUM Dataset

1. Download a sequence from http://vision.in.tum.de/data/datasets/rgbd-dataset/download and uncompress it.

2. Associate RGB images and depth images using the python script [associate.py](http://vision.in.tum.de/data/datasets/rgbd-dataset/tools). We already provide associations for some of the sequences in *Examples/RGB-D/associations/*. You can generate your own associations file executing:

  ```
  python associate.py PATH_TO_SEQUENCE/rgb.txt PATH_TO_SEQUENCE/depth.txt > associations.txt
  ```

3. Execute the following command. Change `TUMX.yaml` to TUM1.yaml,TUM2.yaml or TUM3.yaml for freiburg1, freiburg2 and freiburg3 sequences respectively. Change `PATH_TO_SEQUENCE_FOLDER`to the uncompressed sequence folder. Change `ASSOCIATIONS_FILE` to the path to the corresponding associations file.

  ```
  ./Examples/RGB-D/rgbd_tum Vocabulary/ORBvoc.txt Examples/RGB-D/TUMX.yaml PATH_TO_SEQUENCE_FOLDER ASSOCIATIONS_FILE
  ```


#8. Processing your own sequences
You will need to create a settings file with the calibration of your camera. See the settings file provided for the TUM and KITTI datasets for monocular, stereo and RGB-D cameras. We use the calibration model of OpenCV. See the examples to learn how to create a program that makes use of the ORB-SLAM2 library and how to pass images to the SLAM system. Stereo input must be synchronized and rectified. RGB-D input must be synchronized.

#9. Need Help?

If you have any trouble installing or using ORB-SLAM2, contact the authors.
