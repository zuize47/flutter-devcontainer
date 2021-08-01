
FROM ubuntu:focal

ENV LANG C.UTF-8

# Hack to get openjdk to install in a container
RUN mkdir -p /usr/share/man/man1 \
    && mkdir -p /usr/share/man/man7

ARG USERNAME=dev
ARG USER_UID=1001
ARG USER_GID=$USER_UID     

# Apt   
RUN apt update \
    && apt install -y sudo curl wget git xz-utils lib32stdc++6 unzip openjdk-8-jdk-headless --no-install-recommends \
    # Clean Up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* \
    && groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && mkdir /apps \
    && chown $USERNAME:$USERNAME /apps
    

USER dev 

# Android SDK
RUN cd /apps && \
    wget https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip -P /apps \
    && mkdir android-sdk \
    && unzip sdk-tools-linux-4333796.zip -d android-sdk \
    && rm sdk-tools-linux-4333796.zip

ENV ANDROID_HOME="/apps/android-sdk"
ENV PATH="/apps/android-sdk/tools/bin:/apps/android-sdk/build-tools:/apps/android-sdk/platform-tools:${PATH}"

# SDK manager
RUN yes | sdkmanager --licenses \
    && sdkmanager "platforms;android-29" "platform-tools" "build-tools;29.0.3"

# Flutter
RUN cd /apps \
    && wget https://storage.googleapis.com/flutter_infra_release/releases/stable/linux/flutter_linux_2.2.3-stable.tar.xz \
    && tar xf flutter_linux_2.2.3-stable.tar.xz \
    && rm flutter_linux_2.2.3-stable.tar.xz
ENV PATH="/apps/flutter/bin:${PATH}"

RUN flutter config --no-analytics


# Set a useful default shell
ENV SHELL /bin/bash
