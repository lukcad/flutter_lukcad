FROM ubuntu:20.04
# export timezone - for python3.9-dev install
ENV TZ=Europe/London

# place timezone data /etc/timezone
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# install
RUN apt-get update && apt-get -y install --no-install-recommends python3.9-dev

# Prerequisites
RUN apt update && apt install -y curl git unzip xz-utils zip libglu1-mesa openjdk-8-jdk wget clang cmake ninja-build pkg-config libgtk-3-dev

# Set up new user
RUN useradd -ms /bin/bash developer
USER developer
WORKDIR /home/developer

# Prepare Android directories and system variables
RUN mkdir -p Android/sdk
ENV ANDROID_SDK_ROOT /home/developer/Android/sdk
RUN mkdir -p .android && touch .android/repositories.cfg

# Set up Android SDK
RUN wget -O sdk-tools.zip https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip
RUN unzip sdk-tools.zip && rm sdk-tools.zip
RUN mv tools Android/sdk/tools
RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --licenses
RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --list
RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --update
RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --install "cmdline-tools;latest" 
RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --install "build-tools;29.0.2" 
#RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --install "patcher;v4" 
RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --install "platform-tools" 
RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --install "platforms;android-29" 
RUN cd Android/sdk/tools/bin && yes | ./sdkmanager --install "sources;android-29"
ENV PATH "$PATH:/home/developer/Android/sdk/platform-tools"

# Download Flutter SDK
RUN git clone https://github.com/flutter/flutter.git
ENV PATH "$PATH:/home/developer/flutter/bin"

# Run basic check to download Dark SDK
RUN flutter doctor