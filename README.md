# <Issue/Discussion number — fill in after creating the issue>

First, read the [Renovate minimal reproduction instructions](https://github.com/renovatebot/renovate/blob/main/docs/development/minimal-reproductions.md).

## Current behavior

Renovate crashes/fails when scanning a `docker-compose.yml` that contains a `traefik` image with a `v`-prefixed semver tag combined with a digest pin:

```yaml
image: traefik:v2.11.45@sha256:4c27a775e8820b7de04a3624b196cf1c5f6b935cfe322df4be13eb24a5179962
```

The `traefik` Docker Hub repository publishes both `v2.11.45` and `2.11.45` as separate tags for the same image. Renovate's version resolution fails when encountering the `v`-prefixed tag, causing the scan to crash.

## Expected behavior

Renovate should successfully scan and process the `traefik:v2.11.45@sha256:...` image in `docker-compose.yml` without crashing, preserving the `v` prefix in any updated tag.

## Link to the Renovate issue or Discussion

<!-- Add your GitHub issue/discussion link here after filing it -->
