FROM ubuntu:noble

# Set environment variables
ENV ANDROID_SDK_ROOT=/usr/lib/android-sdk

ENV FLUTTER_SDK_ROOT=/usr/lib/flutter
ENV FLUTTER_SDK_VERSION=3.38.7-stable

ENV ANDROID_CMDLINE_TOOLS_VERSION=14742923_latest
ENV ANDROID_API_LEVEL=36
ENV ANDROID_BUILD_TOOLS_VERSION=36.0.0

# Include flutter and android tools in path
ENV PATH="${PATH}:${FLUTTER_SDK_ROOT}/bin:${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin:${ANDROID_SDK_ROOT}/platform-tools"

# Enable noninteractive installation
ENV DEBIAN_FRONTEND=noninteractive

# Install dependencies and tools
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y \
    watchman \
    openjdk-17-jdk \
    android-sdk \
    wget \
    unzip \
    zip \
    cmake \
    ninja-build \
    clang \
    pkg-config \
    libgtk-3-dev \
    curl \
    avahi-utils \
    git-all \
    xz-utils \
    libglu1-mesa \
    libc6:amd64 libstdc++6:amd64 lib32z1 libbz2-1.0:amd64 && \
    rm -rf /var/lib/apt/lists/*

# Add flutter to safe directory in gitf;
RUN git config --global --add safe.directory /usr/lib/flutter



# Download and install Android Command Line Tools
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    wget -O commandlinetools.zip "https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_CMDLINE_TOOLS_VERSION}.zip" && \
    unzip commandlinetools.zip -d ${ANDROID_SDK_ROOT}/cmdline-tools && \
    mv ${ANDROID_SDK_ROOT}/cmdline-tools/cmdline-tools ${ANDROID_SDK_ROOT}/cmdline-tools/latest && \
    rm commandlinetools.zip

# Accept Android SDK licenses and install necessary components
RUN yes | ${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin/sdkmanager --licenses && \
    ${ANDROID_SDK_ROOT}/cmdline-tools/latest/bin/sdkmanager "platform-tools" "platforms;android-${ANDROID_API_LEVEL}" "build-tools;${ANDROID_BUILD_TOOLS_VERSION}"


# Insall Flutter SDK
RUN mkdir -p ${FLUTTER_SDK_ROOT} && \
    cd ${FLUTTER_SDK_ROOT} && \
    curl -OL https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_${FLUTTER_SDK_VERSION}.tar.xz && \
    tar -xf flutter_linux_${FLUTTER_SDK_VERSION}.tar.xz -C /usr/lib/ && \
    rm -rf flutter_linux_${FLUTTER_SDK_VERSION}.tar.xz

# Disable Flutter telemetry
RUN flutter --disable-analytics

# Default shell to bash
SHELL ["/bin/bash", "-c"]