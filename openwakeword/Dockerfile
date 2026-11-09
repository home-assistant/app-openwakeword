# Base image updated by Renovate, update versionCompatibility on Debian base bump
ARG BUILD_FROM=ghcr.io/home-assistant/base-debian:trixie-2026.11.0@sha256:d996c8f8ef0096a1ac3756766149b31b8171afc663068cc192f2bcab20f1b8c9
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install openWakeWord
WORKDIR /usr/src
ARG WYOMING_OPENWAKEWORD_VERSION=2.1.0

RUN \
    apt-get update \
    && apt-get install -y --no-install-recommends \
        netcat-traditional \
        python3 \
        python3-pip \
        python3-venv \
    \
    && python3 -m venv .venv \
    && .venv/bin/pip3 install --no-cache-dir -U \
        setuptools \
        wheel \
    && .venv/bin/pip3 install --no-cache-dir \
        --extra-index-url https://www.piwheels.org/simple \
        "wyoming[zeroconf]>=1.8,<2" \
        "wyoming-openwakeword @ https://github.com/rhasspy/wyoming-openwakeword/archive/refs/tags/v${WYOMING_OPENWAKEWORD_VERSION}.tar.gz" \
    \
    && rm -rf /var/lib/apt/lists/*

ENV PATH="/usr/src/.venv/bin:$PATH"
WORKDIR /
COPY rootfs /

HEALTHCHECK --start-period=10m \
    CMD echo '{ "type": "describe" }' \
        | nc -w 1 localhost 10400 \
        | grep -iq "openWakeWord" \
        || exit 1

LABEL \
    org.opencontainers.image.title="Home Assistant App: openWakeWord" \
    org.opencontainers.image.description="openWakeWord using the Wyoming protocol" \
    org.opencontainers.image.source="https://github.com/home-assistant/app-openwakeword" \
    org.opencontainers.image.licenses="Apache License 2.0"
