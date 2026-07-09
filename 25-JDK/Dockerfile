# ----------------------------------
# Pterodactyl Core Dockerfile
# Environment: Java
# Minimum Panel Version: 1.7.0
# ----------------------------------
FROM ubuntu:26.04

ARG TARGETPLATFORM
# Define both the release tag and the exact version in the filename
ARG GRAAL_TAG=25.1.3
ARG GRAAL_VERSION=25i1-25.0.3
ARG JAVA_VERSION=25

# Swap MAINTAINER for LABEL to clear up that build warning too!
LABEL maintainer="mani1232 <support@worldmandia>"

ENV DEBIAN_FRONTEND=noninteractive
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

RUN apt-get update -y \
  && apt-get install -y --no-install-recommends \
     curl ca-certificates openssl git tar sqlite3 fontconfig tzdata locales iproute2 \
  && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
  && locale-gen en_US.UTF-8 \
  && case ${TARGETPLATFORM} in \
         "linux/amd64")  ARCH=x64  ;; \
         "linux/arm64")  ARCH=aarch64  ;; \
         *)              echo "Unsupported architecture: ${TARGETPLATFORM}"; exit 1 ;; \
    esac \
  && curl --retry 3 -Lfso /tmp/graalvm.tar.gz https://github.com/graalvm/graalvm-ce-builds/releases/download/jdk-${GRAAL_TAG}/graalvm-community-jdk-${GRAAL_VERSION}_linux-${ARCH}_bin.tar.gz \
  && mkdir -p /opt/java/graalvm \
  && tar -xf /tmp/graalvm.tar.gz -C /opt/java/graalvm --strip-components=1 \
  && rm -rf /var/lib/apt/lists/* /tmp/graalvm.tar.gz

ENV JAVA_HOME=/opt/java/graalvm \
    PATH="/opt/java/graalvm/bin:$PATH"

# Step 2 - add pterodactyl stuff
RUN useradd -d /home/container -m container

USER        container
ENV         USER=container HOME=/home/container

WORKDIR     /home/container

COPY        ./../entrypoint.sh /entrypoint.sh

CMD         ["/bin/bash", "/entrypoint.sh"]
