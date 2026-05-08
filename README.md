# <Issue/Discussion number — fill in after creating the issue>

First, read the [Renovate minimal reproduction instructions](https://github.com/renovatebot/renovate/blob/main/docs/development/minimal-reproductions.md).

## Current behavior

Renovate job fails with status **FAILED / worker-signaled** when processing a `docker-compose.yml` that contains the `traefik` image with a `v`-prefixed semver tag and a digest pin:

```yaml
image: traefik:v2.11.45@sha256:4c27a775e8820b7de04a3624b196cf1c5f6b935cfe322df4be13eb24a5179962
```

**Observed symptoms:**
- Mend.io job status: `FAILED` — `worker-signaled` (process killed by signal)
- Job duration: ~1 min 6 s (well within the 30-minute limit)
- Memory used: ~2.1 GB
- The update branch `renovate/traefik-3.x` **is created** in the repository
- No PR is opened
- The Dependency Dashboard issue is **not created / not updated**

**Log trail:** The last log entry before the crash is:
```
"msg":"Fetching changelog: https://github.com/containous/traefik (v2.11.45 -> v3.7.0)"
```

Two contributing factors visible in the logs:

1. **Stale `sourceUrl`** — Renovate resolves `sourceUrl` as `https://github.com/containous/traefik`
   (the old organization; traefik has since moved to `https://github.com/traefik/traefik`).
   The changelog fetch targets the wrong/redirected URL.

2. **"Multiple changed values" warning** — log line:
   ```
   "changedCount":2, "msg":"Multiple changed values, might need special handling (#36461)"
   ```
   Both the tag (`v2.11.45` → `v3.7.0`) and the digest need to be replaced simultaneously.

## Expected behavior

Renovate should complete the job successfully:
- Open a PR for the `traefik` major version update
- Create / update the Dependency Dashboard issue
- Not crash while fetching the changelog

## Link to the Renovate issue or Discussion

<!-- Add your GitHub issue/discussion link here after filing it -->
