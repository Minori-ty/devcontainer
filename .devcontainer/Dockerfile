FROM ubuntu:22.04

# 环境变量
ENV DEBIAN_FRONTEND=noninteractive
ENV LANG C.UTF-8
ENV JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV ANDROID_HOME=$ANDROID_SDK_ROOT
ENV PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/emulator:$ANDROID_NDK_HOME
ENV ANDROID_NDK_HOME=$ANDROID_SDK_ROOT/ndk/25.2.9519653

# 安装基础依赖
RUN apt-get update && apt-get install -y \
    curl wget unzip git zip tar \
    openjdk-21-jdk \
    nodejs npm \
    gnupg ca-certificates lsb-release && \
    npm install -g npm@latest pnpm && \
    rm -rf /var/lib/apt/lists/*

# 安装 Gradle 8.13
RUN wget https://services.gradle.org/distributions/gradle-8.13-bin.zip -P /tmp && \
    unzip -d /opt/gradle /tmp/gradle-8.13-bin.zip && \
    ln -s /opt/gradle/gradle-8.13/bin/gradle /usr/bin/gradle

# 设置 Gradle 环境变量
ENV GRADLE_HOME=/opt/gradle/gradle-8.13
ENV PATH=$PATH:$GRADLE_HOME/bin

# 安装 Android SDK
RUN mkdir -p ${ANDROID_SDK_ROOT}/cmdline-tools && \
    cd ${ANDROID_SDK_ROOT}/cmdline-tools && \
    wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -O tools.zip && \
    unzip tools.zip && rm tools.zip && \
    mv cmdline-tools latest

# 安装 SDK 组件（包括 build-tools, platform-tools, NDK 等）
RUN yes | sdkmanager --licenses && \
    sdkmanager "platform-tools" "platforms;android-34" \
    "build-tools;34.0.0" \
    "ndk;25.2.9519653" \
    "cmake;3.22.1" \
    "emulator"

# 确认版本
RUN java -version && \
    gradle -v && \
    node -v && \
    npm -v && \
    pnpm -v && \
    sdkmanager --list
