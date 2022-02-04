# Introduction
This repository extends the Toradex Torizon Debian `bullseye` docker images.

The `torizon-ros-base` image is build with a custom `boost` 1.72 version and `ROS Noetic` built from source. 

## How to build
The `torizon-ros-base` image is rebuilt on each update. 

The custom `boost` and `ROS Noetic` images are not built automatically as they take a long time and will not be rebuilt that often. Under the `Actions` tab you can find two workflows that can be launched manually on a desired branch in order to rebuild this docker images. These two docker images contain the binaries which are copied into the `torizon-ros-base` docker image.

To properly rebuild, make sure to **first** run the `boost` workflow and then the `ROS` workflow since these are dependent on each other. These two workflows will update the `torizon-ros-base:boost1.72-binaries` and `torizon-ros-base:noetic-binaries` tags of the docker image.
