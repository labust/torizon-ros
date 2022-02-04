ARG IMAGE_ARCH=linux/arm64
ARG BASE_NAME=torizon-ros-base
ARG IMAGE_TAG=noetic-build-base
ARG DOCKER_REGISTRY=ghcr.io/labust

FROM --platform=$IMAGE_ARCH $DOCKER_REGISTRY/$BASE_NAME:$IMAGE_TAG AS devbase

RUN apt-get -y update && apt-get install -y --no-install-recommends \
    iputils-ping \
    git \
    minicom \
    python3-pip \
    python3-catkin-tools \
    python3-wstool \
    python3-dev \
    vim \
    && apt-get clean && apt-get autoremove && rm -rf /var/lib/apt/lists/*

RUN apt-get -y update && apt-get install -y \
	libgstreamer1.0-0 \
	gstreamer1.0-plugins-base \
	gstreamer1.0-plugins-good \
	gstreamer1.0-plugins-bad \
	gstreamer1.0-plugins-ugly \
	gstreamer1.0-libav \
	gstreamer1.0-doc \
	gstreamer1.0-tools \
	gstreamer1.0-x \
	gstreamer1.0-alsa \
	gstreamer1.0-gl \
	gstreamer1.0-gtk3 \
	gstreamer1.0-pulseaudio \
	&& if [ "${IMAGE_ARCH}" = "linux/arm64/v8" ]; then \
		apt-get install -y \
		gstreamer1.0-qt5; fi \
	&& apt-get clean && apt-get autoremove && rm -rf /var/lib/apt/lists/*

COPY --from=ghcr.io/labust/torizon-ros-base:noetic-binaries /opt/ros/noetic /opt/ros/noetic
COPY --from=ghcr.io/labust/torizon-ros-base:noetic-binaries /opt/rosws/install_dependencies.sh /opt/ros/noetic/install_dependencies.sh

RUN apt-get -y update && opt/ros/noetic/install_dependencies.sh \
    && apt-get install -y --no-install-recommends \ 
        libopencv-gapi4.4 \
    && apt-get remove libboost* \
    && apt-get clean && apt-get autoremove && rm -rf /var/lib/apt/lists/*

COPY --from=ghcr.io/labust/torizon-ros-base:boost1.72-binaries /opt/boost/build/include /usr/include
COPY --from=ghcr.io/labust/torizon-ros-base:boost1.72-binaries /opt/boost/build/lib /usr/lib

COPY OpenCVModules-release.cmake /usr/lib/aarch64-linux-gnu/cmake/opencv4/

WORKDIR /root