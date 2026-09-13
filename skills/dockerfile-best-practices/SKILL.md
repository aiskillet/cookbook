---
name: dockerfile-best-practices
description: Write small, fast, secure container images — multi-stage builds, layer caching, non-root users, and pinned versions. Use when writing or reviewing a Dockerfile or debugging slow/bloated builds.
---

# Dockerfile Best Practices

A good image is small, cached well, reproducible, and runs as non-root. Most Dockerfiles fail on all four.

## When to Activate
- Writing or reviewing a Dockerfile
- Build is slow or the image is huge
- Hardening a container for production

## The five defaults
1. **Multi-stage builds** — compile/install in a `builder` stage, copy only artifacts into a slim runtime. This is the biggest size win.
2. **Order layers by change frequency** — copy dependency manifests and install *before* copying source, so code changes don't bust the dependency cache.
3. **Pin versions** — base image by digest or exact tag (`node:20.20.2-slim`), not `latest`. Reproducibility.
4. **Run as non-root** — create a user, `USER app`. Never run app processes as root.
5. **Minimize the final image** — slim/distroless base, no build tools, no cache in the layer.

## Layer caching, concretely
```dockerfile
COPY package*.json ./
RUN npm ci --omit=dev        # cached unless deps change
COPY . .                     # code changes only bust from here down
```

## Also
- **`.dockerignore`** — exclude `node_modules`, `.git`, secrets, test output. Smaller context, faster builds, fewer leaks.
- **One concern per image** — no init systems running five daemons.
- **`COPY` over `ADD`** unless you need ADD's URL/tar features.
- **Combine related `RUN`s** and clean caches in the same layer (`apt-get ... && rm -rf /var/lib/apt/lists/*`).
- **No secrets in layers** — use build secrets/args that don't persist; secrets in `ENV`/layers are extractable.
- **`HEALTHCHECK`** for orchestrator liveness.

## Checklist
- [ ] Multi-stage; final image has no build tools
- [ ] Deps installed before source copy (cache-friendly)
- [ ] Base image pinned; runs as non-root
- [ ] `.dockerignore` present; no secrets baked in
- [ ] Caches cleaned within the same RUN layer
