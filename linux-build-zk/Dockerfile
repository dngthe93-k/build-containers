# syntax=docker/dockerfile:1

FROM ubuntu:24.04

RUN --mount=type=cache,id=apt-cache-ubuntu-26.04,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,id=apt-lib-ubuntu-26.04,target=/var/lib/apt,sharing=locked \
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
        # clang installation
        gnupg \
        lsb-release \
        software-properties-common \
        wget \
        # build
        git \
        make \
        ninja-build \
        curl \
        # package
        mono-complete \
        # libraries
        zlib1g-dev \
        nlohmann-json3-dev \
        libgtest-dev \
        libssl-dev; \
    \
    # 3) Install clang 21
    wget https://apt.llvm.org/llvm.sh; \
    chmod +x llvm.sh; \
    ./llvm.sh 21 all; \
    rm -f ./llvm.sh; \
    \
    # 4) Update alternatives
    update-alternatives --install /usr/bin/clang clang /usr/bin/clang-21 100; \
    update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-21 100; \
    update-alternatives --install /usr/bin/cc cc /usr/bin/clang 100; \
    update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang++ 100; \
    \
    # 5)  Install nuget latest
    curl -o /usr/local/bin/nuget.exe https://dist.nuget.org/win-x86-commandline/latest/nuget.exe; \
    printf '#!/bin/sh\nexec mono /usr/local/bin/nuget.exe "$@"' > /usr/local/bin/nuget; \
    chmod +x /usr/local/bin/nuget; \
    \
    # 6) Install cmake 4.2.3
    curl -L https://cmake.org/files/v4.2/cmake-4.2.3-linux-x86_64.tar.gz | tar -xzf - -C /usr/local/share; \
    ln -s /usr/local/share/cmake-4.2.3-linux-x86_64/bin/* /usr/local/bin/; \
    \
    # 7) Restore default apt clean behavior
    rm -f /etc/apt/apt.conf.d/keep-cache; \
    mv /tmp/docker-clean /etc/apt/apt.conf.d/
