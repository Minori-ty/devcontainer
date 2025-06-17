FROM alpine:latest

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=en_US.UTF-8
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV ANDROID_NDK_HOME=$ANDROID_SDK_ROOT/ndk/27.1.12297006
ENV PATH=$PATH:$JAVA_HOME/bin:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$ANDROID_NDK_HOME
ENV NODE_ENV=production
# 安装基础工具和Java 17
RUN apk update && apk add --no-cache \
    bash curl unzip git openjdk17 tzdata nodejs npm

# 升级 npm & 安装 pnpm
RUN npm install -g npm@latest pnpm

# 环境变量
ENV GRADLE_VERSION=8.13 \
    GRADLE_HOME=/opt/gradle

# 安装依赖
RUN apk add --no-cache openjdk17 curl unzip bash

# 解压 gradle 到 GRADLE_HOME（提前安装）
RUN mkdir -p ${GRADLE_HOME} && \
    curl -fsSL https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle.zip && \
    unzip -q gradle.zip -d /opt && \
    mv /opt/gradle-${GRADLE_VERSION}/* ${GRADLE_HOME}/ && \
    rm -rf /opt/gradle-${GRADLE_VERSION} gradle.zip

ENV PATH="${GRADLE_HOME}/bin:${PATH}"

# 设置工作目录
WORKDIR /app

# 手动构建 gradle wrapper 结构
RUN mkdir -p gradle/wrapper && \
    curl -fsSL https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle/wrapper/gradle-${GRADLE_VERSION}-bin.zip && \
    echo "distributionUrl=https\\://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" > gradle/wrapper/gradle-wrapper.properties && \
    curl -fsSL https://raw.githubusercontent.com/gradle/gradle/v${GRADLE_VERSION}/gradle/wrapper/gradle-wrapper.jar -o gradle/wrapper/gradle-wrapper.jar && \
    curl -fsSL https://raw.githubusercontent.com/gradle/gradle/v${GRADLE_VERSION}/gradlew -o gradlew && \
    chmod +x gradlew

# 安装 Android SDK 和 NDK
RUN mkdir -p $ANDROID_SDK_ROOT && cd $ANDROID_SDK_ROOT && \
    curl -O https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip && \
    unzip commandlinetools-linux-*.zip -d $ANDROID_SDK_ROOT/cmdline-tools && \
    mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest && \
    rm commandlinetools-linux-*.zip

# 安装 SDK components
RUN yes | sdkmanager --licenses && \
    sdkmanager "platform-tools" \
    "platforms;android-35" \
    "build-tools;35.0.0" \
    "ndk;27.1.12297006"

RUN npm i -g eas-cli