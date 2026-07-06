# syntax=docker/dockerfile:1@sha256:87999aa3d42bdc6bea60565083ee17e86d1f3339802f543c0d03998580f9cb89

# ── Stage 1: download ─────────────────────────────────────────────
FROM alpine@sha256:28bd5fe8b56d1bd048e5babf5b10710ebe0bae67db86916198a6eec434943f8b AS downloader

ARG TARGETARCH
ARG LAZYMC_VERSION=0.2.21

RUN apk add --no-cache curl; \
    case "${TARGETARCH}" in \
        amd64)  ARCH_TRIPLET="x86_64-unknown-linux-gnu"  ;; \
        arm64)  ARCH_TRIPLET="aarch64-unknown-linux-gnu" ;; \
        *)      echo "Unsupported architecture: ${TARGETARCH}" >&2; exit 1 ;; \
    esac; \
    ASSET="lazymc-v${LAZYMC_VERSION}-${ARCH_TRIPLET}.tar.gz"; \
    URL="https://github.com/EfinaServer/lazymc/releases/download/v${LAZYMC_VERSION}/${ASSET}"; \
    curl -fsSL "${URL}" -o /tmp/lazymc.tar.gz; \
    tar -xzf /tmp/lazymc.tar.gz -C /tmp; \
    find /tmp -maxdepth 2 -name "lazymc" -type f -exec cp {} /usr/local/bin/lazymc \;

# ── Stage 2: final ────────────────────────────────────────────────
FROM ghcr.io/pelican-eggs/yolks:java_21@sha256:0a391827c300af182b0a36e18ff8340bf8ccba123d297f43e0373e25f2fcead9

COPY --from=downloader --chmod=755 /usr/local/bin/lazymc /usr/local/bin/lazymc

# Inherited from ghcr.io/pelican-eggs/yolks:java_21:
#   USER      container
#   HOME      /home/container
#   WORKDIR   /home/container
#   ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
#   CMD        ["/entrypoint.sh"]
#
# In your Pelican/Pterodactyl egg, set the STARTUP variable to:
#   lazymc start
