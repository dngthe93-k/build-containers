# syntax=docker/dockerfile:1

FROM ubuntu:24.04

RUN --mount=type=cache,id=apt-cache-ubuntu-24.04,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,id=apt-lib-ubuntu-24.04,target=/var/lib/apt,sharing=locked \
    set -eux; \
    export DEBIAN_FRONTEND=noninteractive; \
    \
    # 1) Enable apt cache retention (temporary)
    mv /etc/apt/apt.conf.d/docker-clean /tmp/; \
    echo 'Binary::apt::APT::Keep-Downloaded-Packages "true";' \
        > /etc/apt/apt.conf.d/keep-cache; \
    \
    # 2) Install required packages
    apt-get update; \
    apt-get install -y --no-install-recommends \
        # build
        git \
        cmake \
        make \
        ninja-build \
        pkg-config \
        build-essential \
        ca-certificates \
        file \
        # aarch64 cross toolchain
        gcc-aarch64-linux-gnu \
        g++-aarch64-linux-gnu \
        binutils-aarch64-linux-gnu \
        # run aarch64 binaries on x86_64 host
        qemu-user-static \
        binfmt-support; \
    \
    # 3) Restore default apt clean behavior
    rm -f /etc/apt/apt.conf.d/keep-cache; \
    mv /tmp/docker-clean /etc/apt/apt.conf.d/

COPY <<'EOF' /opt/toolchains/aarch64-linux-gnu.cmake
set(CMAKE_SYSTEM_NAME      Linux)
set(CMAKE_SYSTEM_PROCESSOR aarch64)

set(CMAKE_C_COMPILER   aarch64-linux-gnu-gcc)
set(CMAKE_CXX_COMPILER aarch64-linux-gnu-g++)

set(CMAKE_FIND_ROOT_PATH /usr/aarch64-linux-gnu)

set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
EOF

ENV AARCH64_SYSROOT=/usr/aarch64-linux-gnu \
    AARCH64_CMAKE_TOOLCHAIN_FILE=/opt/toolchains/aarch64-linux-gnu.cmake
