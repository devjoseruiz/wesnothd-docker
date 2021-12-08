ARG WESNOTHD_BRANCH="1.12"
ARG WESNOTHD_VERSION="${WESNOTHD_BRANCH}.6"
ARG DEBIAN_FRONTEND="noninteractive"

# STAGE-1: configure & build
FROM ubuntu:bionic AS builder

# Renew args for persisting through stages
ARG WESNOTHD_BRANCH
ARG WESNOTHD_VERSION
ARG DEBIAN_FRONTEND

# Add non-privileged user
RUN adduser --shell /bin/bash --disabled-password --gecos '' builder-user
WORKDIR /home/builder-user
# Install required dependencies for building Wesnoth binaries
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl cmake gcc g++ \
    libboost-filesystem-dev libboost-locale-dev libboost-iostreams-dev \
    libboost-random-dev libboost-regex-dev libboost-program-options-dev \
    libboost-system-dev libboost-context-dev libboost-coroutine-dev \
    libboost-serialization-dev libboost-thread-dev libasio-dev libsdl-dev \
    libsdl-image1.2-dev libsdl-mixer1.2-dev libsdl-net1.2-dev \
    libsdl-ttf2.0-dev fontconfig libcairo2-dev gettext libvorbis-dev \
    libpango1.0-dev libbz2-dev libz3-dev libssl-dev && \
    apt-get clean
# Get Wesnoth source code from SourceForge
USER builder-user
RUN curl -kL "https://sourceforge.net/projects/wesnoth/files/\
wesnoth-${WESNOTHD_BRANCH}/wesnoth-${WESNOTHD_VERSION}/\
wesnoth-${WESNOTHD_VERSION}.tar.bz2/download" > wesnoth-source.tar.bz2 && \
    mkdir wesnoth-source && \
    tar -xvf wesnoth-source.tar.bz2 -C wesnoth-source --strip-components 1 && \
    rm wesnoth-source.tar.bz2
# Compile server binary only
WORKDIR /home/builder-user/wesnoth-source
RUN cmake . -DCMAKE_BUILD_TYPE=Release -DENABLE_GAME=OFF -DENABLE_SERVER=ON \
    -DCMAKE_INSTALL_PREFIX=../wesnothd && \
    make && \
    make install && \
# Clean unnecessary content
    rm -r ../wesnothd/share/wesnoth

# STAGE-2: run server
FROM ubuntu:bionic AS app

# Renew args for persisting through stages
ARG WESNOTHD_BRANCH
ARG WESNOTHD_VERSION
ARG DEBIAN_FRONTEND

LABEL name="wesnothd-${WESNOTHD_BRANCH}" \
      version="${WESNOTHD_VERSION}" \
      maintainer="joseruizdev" \
      url="https://gitlab.com/joseruizdev/wesnothd-docker" \
      description="Dockerized server for The Battle for Wesnoth"

# Add non-privileged user
RUN adduser --shell /bin/bash --disabled-password --gecos '' wesnothd
# Copy server binary from previous stage
WORKDIR /home/wesnothd
COPY --from=builder /home/builder-user/wesnothd .
# Install required dependencies for server execution
RUN apt-get update && \
    apt-get install -y --no-install-recommends libboost-filesystem1.65.1 \
    libboost-locale1.65.1 libboost-iostreams1.65.1 libssl1.1 libsdl1.2debian \
    libsdl-net1.2 && \
    apt-get clean

USER wesnothd
CMD [ "bin/wesnothd" ]
