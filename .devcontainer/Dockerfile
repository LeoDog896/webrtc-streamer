FROM mcr.microsoft.com/devcontainers/cpp:0-ubuntu-22.04

ARG REINSTALL_CMAKE_VERSION_FROM_SOURCE="3.22.2"

# Optionally install the cmake for vcpkg
COPY ./reinstall-cmake.sh /tmp/

RUN if [ "${REINSTALL_CMAKE_VERSION_FROM_SOURCE}" != "none" ]; then \
        chmod +x /tmp/reinstall-cmake.sh && /tmp/reinstall-cmake.sh ${REINSTALL_CMAKE_VERSION_FROM_SOURCE}; \
    fi \
    && rm -f /tmp/reinstall-cmake.sh

# [Optional] Uncomment this section to install additional vcpkg ports.
# RUN su vscode -c "${VCPKG_ROOT}/vcpkg install <your-port-name-here>"

# install build packages
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends libssl-dev libasound2 \
    libgtk-3-0 libxtst6 libpulse0 librtmp1 avahi-utils ca-certificates \
    wget git python3 python3-pkg-resources g++ autoconf automake libtool \
    xz-utils libpulse-dev libasound2-dev libgtk-3-dev libxtst-dev libssl-dev \
    librtmp-dev cmake make pkg-config p7zip-full g++-10

# install depot_tools
RUN git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git /depot_tools \
	&& export PATH=/depot_tools:$PATH \
	&& mkdir /webrtc \
	&& cd /webrtc \
	&& fetch --no-history --nohooks webrtc \
	&& sed -i -e "s|'src/resources'],|'src/resources'],'condition':'rtc_include_tests==true',|" src/DEPS \
	&& gclient sync