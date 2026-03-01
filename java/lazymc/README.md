# Java + LazyMC

Docker images combining Java runtimes with [LazyMC](https://github.com/EfinaServer/lazymc) — a lazy-loading proxy that keeps Minecraft servers dormant until the first player connects.

## Available Images

Images are published to the GitHub Container Registry under `ghcr.io/efinaserver/shells/java`.

| Image | Java Version | Tag |
|-------|-------------|-----|
| `ghcr.io/efinaserver/shells/java:8-lazymc` | Java 8 | `8-lazymc` |
| `ghcr.io/efinaserver/shells/java:11-lazymc` | Java 11 | `11-lazymc` |
| `ghcr.io/efinaserver/shells/java:21-lazymc` | Java 21 | `21-lazymc` |
| `ghcr.io/efinaserver/shells/java:25-lazymc` | Java 25 | `25-lazymc` |

All images support **linux/amd64** and **linux/arm64**.

## What is LazyMC?

LazyMC is a proxy that puts your Minecraft server to sleep when no players are online. When a player connects, LazyMC wakes the server up and forwards the connection. This reduces resource usage for servers that are not always active.

These images bundle [EfinaServer modified fork of LazyMC](https://github.com/EfinaServer/lazymc), based on the original [timvisee/lazymc](https://github.com/timvisee/lazymc).

## Usage

### Installing the Egg

The recommended way to use these images is through the official egg provided in [EfinaServer/carton](https://github.com/EfinaServer/carton/tree/main/minecraft/lazymc).

**Pterodactyl:**

1. Go to **Admin → Nests → Import Egg**.
2. Upload `egg-lazymc.json` from the [carton repository](https://github.com/EfinaServer/carton/tree/main/minecraft/lazymc).
3. When creating a server, select the imported egg and choose one of the images above for your desired Java version.

**Pelican:**

1. Go to **Admin → Eggs → Import**.
2. Upload `egg-lazymc.json` from the [carton repository](https://github.com/EfinaServer/carton/tree/main/minecraft/lazymc).
3. When creating a server, select the imported egg and choose one of the images above for your desired Java version.

The egg pre-configures all required environment variables (ports, timeouts, join behaviour, MOTD, etc.) and sets the correct startup command automatically.

## Image Details

Each image is built in two stages:

1. **Download stage** — uses Alpine Linux to fetch the appropriate LazyMC binary from [EfinaServer/lazymc](https://github.com/EfinaServer/lazymc) releases and place it at `/usr/local/bin/lazymc`.
2. **Runtime stage** — uses [`ghcr.io/pelican-eggs/yolks:java_<version>`](https://github.com/pelican-eggs/yolks) as the base, with the LazyMC binary copied in.

The working directory is `/home/container` and the default user is `container`, matching the Pelican/Pterodactyl standard.

## Building Locally

```bash
docker build -t java-21-lazymc java/lazymc/21/
```

To build for multiple platforms:

```bash
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t java-21-lazymc \
  java/lazymc/21/
```
