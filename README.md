# Elevation Mapping cupy
[![Build Status](https://ci.leggedrobotics.com/buildStatus/icon?job=bitbucket_leggedrobotics/elevation_mapping_cupy/master)](https://ci.leggedrobotics.com/job/<repo_host>_leggedrobotics/job/elevation_mapping_cupy/job/master/)

## Overview
This is a ros package of elevation mapping on GPU.  
Code are written in python and uses cupy for GPU calculation.  
![screenshot](doc/real.png)

## Installation

### CUDA & cuDNN
The tested versions are CUDA10.0, cuDNN7.

#### CUDA
You can download CUDA10.0 from [here](https://developer.nvidia.com/cuda-10.0-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=deblocal).  
You can follow the instruction.
```bash
sudo dpkg -i cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda
```

#### cuDNN
You can download specific version from [here](https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/).  
For example, the tested version is with `libcudnn7-dev_7.5.1.10-1+cuda10.0_amd64.deb` and `libcudnn7_7.5.1.10-1+cuda10.0_amd64.deb`.

Then install them using the command below.
```bash
sudo dpkg -i libcudnn7_7.5.1.10-1+cuda10.0_amd64.deb
sudo dpkg -i libcudnn7-dev_7.5.1.10-1+cuda10.0_amd64.deb
```

#### Other information
[CUDA](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#ubuntu-installation)  
[cuDNN](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html#install-linux).


#### On Jetson
`CUDA` and `cuDNN` can be installed via apt.

### Python dependencies
- [numpy==1.17.4](https://www.numpy.org/)
- [scipy==1.2.3](https://www.scipy.org/)
- [cupy==6.7.0](https://cupy.chainer.org/)
- [chainer==6.7.0](https://chainer.org/)
- [shapely==1.6.4](https://github.com/Toblerity/Shapely)

```bash
pip3 install numpy scipy cupy chainer shapely
```
On jetson, pip3 builds the packages from source so it would take time.

Also, on jetson you need fortran (should already be installed).
```bash
sudo apt install gfortran
```

cupy can be installed with specific CUDA versions. (On jetson, only "from source" i.e. via pip3 could work)
> (For CUDA 9.0)
> % pip install cupy-cuda90
>
> (For CUDA 9.1)
> % pip install cupy-cuda91
>
> (For CUDA 9.2)
> % pip install cupy-cuda92
>
> (For CUDA 10.0)
> % pip install cupy-cuda100
>
> (Install CuPy from source)
> % pip install cupy

#### On Jetson
Since NVIDIA does not officially support focal yet, the libraries need the bionic compilers. CUDA C code needs to be compiled with gcc-7 or earlier.
In your catkin workspace set:
```
catkin config --cmake-args -DCMAKE_C_COMPILER=/usr/bin/gcc-7
```

Link `gcc-7` and `g++-7` into CUDA installation. Install with `apt` if its not installed:
```
sudo ln -s /usr/bin/gcc-7 /usr/local/cuda/bin/gcc
sudo ln -s /usr/bin/g++-7 /usr/local/cuda/bin/g++
```

### ROS package dependencies
- [pybind11_catkin](https://github.com/ipab-slmc/pybind11_catkin)
- [grid_map_msgs](https://github.com/ANYbotics/grid_map)
```
sudo apt install ros-noetic-pybind11-catkin
sudo apt install ros-noetic-grid-map-msgs
```

#### On Jetson
```bash
pip3 install catkin_pkg
```

## Usage
### Build
```bash
catkin build elevation_mapping_cupy
```

### Run
```bash
roslaunch elevation_mapping_cupy elevation_mapping_cupy.launch
```
### Subscribed Topics

* topics specified in **`pointcloud_topics`** in **`parameters.yaml`** ([sensor_msgs/PointCloud2])

    The distance measurements.

* **`/tf`** ([tf/tfMessage])

    The transformation tree.


### Published Topics

* **`elevation_map_raw`** ([grid_map_msg/GridMap])

    The entire elevation map.


* **`elevation_map_recordable`** ([grid_map_msg/GridMap])

    The entire elevation map with slower update rate for visualization and logging.
