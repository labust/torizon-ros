ARG IMAGE_ARCH=linux/arm64
ARG BASE_NAME=torizon-ros-base
ARG IMAGE_TAG=noetic-build-base
ARG DOCKER_REGISTRY=ghcr.io/labust

FROM --platform=$IMAGE_ARCH $DOCKER_REGISTRY/$BASE_NAME:$IMAGE_TAG AS rosbuilder
RUN mkdir -p /opt/rosws/src 
WORKDIR /opt/rosws

RUN mkdir -p /opt/rosws/src && cd /opt/rosws \
    && rosinstall_generator \
        ros_base \
        robot \
        image_common \
        image_pipeline \
        image_transport_plugins \
        vision_opencv \
        robot_localization \
        geographic_msgs \
        tf2_ros --rosdistro noetic --deps --tar > noetic-desktop.rosinstall \ 
    && vcs import --input noetic-desktop.rosinstall ./src 

RUN echo "#!/bin/bash \n set -e" > install_dependencies.sh && chmod +x ./install_dependencies.sh \
    && rosdep --os=debian:buster install --from-paths ./src --ignore-src --skip-keys=boost --rosdistro noetic -y -s >> install_dependencies.sh \
    && sed -i 's/-y/& --no-install-recommends/' install_dependencies.sh \
    && apt-get -y update && apt-get install -y --no-install-recommends \
        libopencv-gapi4.4 \
    && ./install_dependencies.sh \
    && apt-get remove libboost* \
    && apt-get clean && apt-get autoremove && rm -rf /var/lib/apt/lists/*

COPY --from=ghcr.io/labust/torizon-ros-base:boost1.72-binaries /opt/boost/build/include /usr/include
COPY --from=ghcr.io/labust/torizon-ros-base:boost1.72-binaries /opt/boost/build/lib /usr/lib/

COPY OpenCVModules-release.cmake /usr/lib/aarch64-linux-gnu/cmake/opencv4/

RUN ./src/catkin/bin/catkin_make_isolated -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.9 \ 
    --install --install-space /opt/ros/noetic -DCMAKE_BUILD_TYPE=Release
