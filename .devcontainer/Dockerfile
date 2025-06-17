FROM ubuntu:22.04

# 基础环境
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    curl wget unzip git zip tar \
    openjdk-21-jdk \
    nodejs npm \
    gnupg ca-certificates lsb-release \
    && npm install -g npm@latest pnpm \
    && rm -rf /var/lib/apt/lists/*

# 安装 Gradle 8.13
RUN wget https://services.gradle.org/distributions/gradle-8.13-bin.zip -P /tmp && \
    unzip -d /opt/gradle /tmp/gradle-8.13-bin.zip && \
    ln -s /opt/gradle/gradle-8.13/bin/gradle /usr/bin/gradle

ENV GRADLE_HOME=/opt/gradle/gradle-8.13
ENV PATH="${GRADLE_HOME}/bin:${PATH}"

# 安装 Android SDK
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV ANDROID_HOME=$ANDROID_SDK_ROOT
ENV ANDROID_CMDLINE_TOOLS_VERSION=11076708_latest

RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    cd ${ANDROID_SDK_ROOT}/cmdline-tools && \
    wget https://dl.google.com/android/repository/commandlinetools-linux-${ANDROID_CMDLINE_TOOLS_VERSION}.zip -O cmdline-tools.zip && \
    unzip cmdline-tools.zip && \
    rm cmdline-tools.zip && \
    mv cmdline-tools latest

ENV PATH="$ANDROID_HOME/cmdline-tools/latest/bin:$PATH"

# 安装 SDK components
RUN yes | sdkmanager --sdk_root=$ANDROID_HOME --licenses && \
    sdkmanager --sdk_root=$ANDROID_HOME \
    "platform-tools" \
    "build-tools;34.0.0" \
    "platforms;android-34" \
    "ndk;25.2.9519653"

# 设置 NDK 路径
ENV ANDROID_NDK_HOME=$ANDROID_HOME/ndk/25.2.9519653
ENV PATH="$ANDROID_NDK_HOME:$PATH"
