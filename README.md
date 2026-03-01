# shells

A collection of Docker images for [Pterodactyl](https://pterodactyl.io/) / [Pelican](https://pelican.dev/) game server panels.

All images are published to the GitHub Container Registry under `ghcr.io/efinaserver/shells` and support **linux/amd64** and **linux/arm64**.

## Images

| Category | Description | Docs |
|----------|-------------|------|
| `java/*-lazymc` | Java runtimes bundled with LazyMC for lazy-loading Minecraft servers | [java/lazymc](java/lazymc/README.md) |

## CI / CD

Images are built and pushed automatically via GitHub Actions on every push to `main`. [Renovate](https://docs.renovatebot.com/) keeps base image digests and tool versions up to date automatically.

## License

[MIT](LICENSE) © 2026 EfinaServer
