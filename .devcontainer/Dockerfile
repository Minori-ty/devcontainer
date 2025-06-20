FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=en_US.UTF-8
ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk
ENV ANDROID_SDK_ROOT=/opt/android-sdk
ENV ANDROID_HOME=$ANDROID_SDK_ROOT
ENV ANDROID_NDK_HOME=$ANDROID_SDK_ROOT/ndk/27.1.12297006
ENV PATH=$PATH:$JAVA_HOME/bin:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$ANDROID_NDK_HOME
ENV NODE_ENV=production
# 安装基础工具和Java 17
RUN apt-get update && apt-get install -y curl unzip git tzdata sudo

RUN curl -sSL https://raw.githubusercontent.com/version-fox/vfox/main/install.sh | bash
ENV PATH="/root/.vfox/bin:$PATH"
RUN vfox add nodejs && vfox install nodejs@20.19.2

# 验证Node.js安装
RUN vfox list nodejs

# RUN vfox use -g nodejs@20.19.2
# 升级 npm & 安装 pnpm
# RUN npm install -g npm@latest pnpm

# 环境变量
# ENV GRADLE_VERSION=8.13 \
#     GRADLE_HOME=/opt/gradle

# # 安装 Gradle 8.13
# RUN curl -fsSL https://services.gradle.org/distributions/gradle-8.13-bin.zip -o gradle.zip && \
#     unzip gradle.zip -d /opt && \
#     ln -s /opt/gradle-8.13 /opt/gradle && \
#     ln -s /opt/gradle/bin/gradle /usr/bin/gradle && \
#     rm gradle.zip

# WORKDIR /app

# # 用系统安装的 Gradle 创建 wrapper
# RUN gradle init --type basic --project-name temp-project && \
#     gradle wrapper --gradle-version ${GRADLE_VERSION} --distribution-type bin && \
#     rm -rf build src *.gradle settings.gradle

# 安装 Android SDK 和 NDK
RUN mkdir -p $ANDROID_SDK_ROOT && cd $ANDROID_SDK_ROOT && \
    curl -O https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip && \
    unzip commandlinetools-linux-*.zip -d $ANDROID_SDK_ROOT/cmdline-tools && \
    mv $ANDROID_SDK_ROOT/cmdline-tools/cmdline-tools $ANDROID_SDK_ROOT/cmdline-tools/latest && \
    rm commandlinetools-linux-*.zip

# 安装 SDK components
# RUN yes | sdkmanager --licenses && \
#     sdkmanager "platform-tools" \
#     "platforms;android-35" \
#     "build-tools;35.0.0" \
#     "ndk;27.1.12297006"

# RUN npm i -g eas-cli expo expo-doctor typescript
