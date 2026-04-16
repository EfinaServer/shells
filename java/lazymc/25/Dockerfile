# syntax=docker/dockerfile:1@sha256:2780b5c3bab67f1f76c781860de469442999ed1a0d7992a5efdf2cffc0e3d769

# ── Stage 1: download ─────────────────────────────────────────────
FROM alpine@sha256:5b10f432ef3da1b8d4c7eb6c487f2f5a8f096bc91145e68878dd4a5019afde11 AS downloader

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
FROM ghcr.io/pelican-eggs/yolks:java_25@sha256:e188a85e55e24c97324c2cb5fc90f542562f338083b45a9cc574078d65b2609b

COPY --from=downloader --chmod=755 /usr/local/bin/lazymc /usr/local/bin/lazymc

# Inherited from ghcr.io/pelican-eggs/yolks:java_25:
#   USER      container
#   HOME      /home/container
#   WORKDIR   /home/container
#   ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
#   CMD        ["/entrypoint.sh"]
#
# In your Pelican/Pterodactyl egg, set the STARTUP variable to:
#   lazymc start
