ARG IMAGE_ARCH=linux/arm64
ARG BASE_NAME=qt5-wayland-examples-vivante
ARG IMAGE_TAG=2
ARG DOCKER_REGISTRY=torizon

FROM --platform=$IMAGE_ARCH $DOCKER_REGISTRY/$BASE_NAME:$IMAGE_TAG AS rosdevbase

RUN apt-get -y update && apt-get install -y --no-install-recommends \
    gnupg \
    ca-certificates \
    && apt-get clean && apt-get autoremove && rm -rf /var/lib/apt/lists/*

RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu buster main" > /etc/apt/sources.list.d/ros-latest.list' \
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

RUN apt-get -y update && apt-get install -y --no-install-recommends \
    imx-gpu-viv-wayland-dev \ 
    python3-rosdep \
    python3-rosinstall-generator \ 
    build-essential \
    python3-vcstool \
    && apt-get clean && apt-get autoremove && rm -rf /var/lib/apt/lists/*

RUN rosdep init && rosdep --os=debian:buster --rosdistro=noetic update