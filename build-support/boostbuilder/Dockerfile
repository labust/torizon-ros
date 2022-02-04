ARG IMAGE_ARCH=linux/arm64
ARG BASE_NAME=qt5-wayland-examples-vivante
ARG IMAGE_TAG=2
ARG DOCKER_REGISTRY=torizon

## Build custom boost 1.72 (to support current version GeminiSDK)
FROM --platform=$IMAGE_ARCH $DOCKER_REGISTRY/$BASE_NAME:$IMAGE_TAG as boostbuilder

RUN apt-get -y update && apt-get install -y --no-install-recommends \
        wget \
        bzip2 \
        build-essential \
        python3-dev \
    && apt-get clean && apt-get autoremove && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /opt/boost/build && cd /opt/boost \
    && wget https://boostorg.jfrog.io/artifactory/main/release/1.72.0/source/boost_1_72_0.tar.bz2  \
    && tar --bzip2 -xf boost_1_72_0.tar.bz2 && cd /opt/boost/boost_1_72_0 \
    && ./bootstrap.sh --prefix=/opt/boost/build/ --with-libraries=all --with-python=/usr/bin/python3 \
    && ./b2 install \
    && cd /opt/boost && rm -rf /opt/boost/boost_1_72_0 boost_1_72_0.tar.bz2
