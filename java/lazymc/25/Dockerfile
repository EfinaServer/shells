# syntax=docker/dockerfile:1@sha256:572cebbc2b15e6f84c077742952674889cb63cd5e9b80abdffcbe587b6f19183

# ── Stage 1: download ─────────────────────────────────────────────
FROM alpine@sha256:f59abe656b692d901e184be3445e084786602108599d2a51d229c0f5a48c71bf AS downloader

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
FROM ghcr.io/pelican-eggs/yolks:java_25@sha256:6fceb622ed4913ebe950012b95d841c6f71e13b69fdb7f7f5374dc86e5d35d2c

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
