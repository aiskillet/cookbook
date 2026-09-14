---
name: kubernetes-basics
description: Deploy and run apps on Kubernetes correctly — Deployments, Services, probes, resources, and config. Use when writing manifests, debugging a pod, or reviewing a k8s setup.
---

# Kubernetes Basics

Kubernetes rewards a few correct habits and punishes their absence with 3am pages. Get probes, resources, and config right and most production pain disappears.

## When to Activate
- Writing/reviewing Deployment/Service manifests
- Debugging a crashing or unreachable pod
- Setting resource limits, health checks, or config

## The core objects
- **Pod** — one or more containers (the unit that runs). You rarely create these directly.
- **Deployment** — manages a ReplicaSet of pods; handles rollouts and self-healing. Use this for stateless apps.
- **Service** — stable network endpoint / load balancer for a set of pods.
- **ConfigMap / Secret** — config and credentials, injected as env vars or files (never bake into the image).

## Get these right
- **Health probes:** `readinessProbe` (ready for traffic?) and `livenessProbe` (needs restart?). Without readiness, traffic hits pods before they're up.
- **Resource requests + limits** for CPU/memory. Requests drive scheduling; limits prevent a noisy neighbor. Missing requests = unpredictable scheduling and OOM kills.
- **Rolling updates** with `maxUnavailable`/`maxSurge`; set them so you don't drop capacity mid-deploy.
- **Config from ConfigMaps/Secrets**, not hardcoded; secrets from a real secret store, not plain manifests in git.
- **One process per container**; let k8s restart on failure rather than baking in supervisors.

## Debugging a pod
```
kubectl get pods                    # status: CrashLoopBackOff? Pending? ImagePullBackOff?
kubectl describe pod <name>         # events explain scheduling/pull/probe failures
kubectl logs <name> [-p]            # -p = previous crashed container
```
Common causes: bad image/tag, missing config/secret, failing probe, insufficient resources (Pending), OOMKilled.

## Checklist
- [ ] Deployment (not bare pods) for stateless apps
- [ ] Readiness + liveness probes set
- [ ] CPU/memory requests **and** limits
- [ ] Config/secrets injected, not hardcoded
- [ ] Rolling-update budget keeps capacity during deploys
