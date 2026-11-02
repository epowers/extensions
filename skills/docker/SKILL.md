---
name: docker
description: Use the rootless Docker engine already provided to this sandbox. Use when building, running, or managing containers and images.
triggers:
- docker
- container
---

# Docker Usage Guide

A rootless Docker engine is already running for this sandbox and is exported through `DOCKER_HOST`.
Run `docker` as the current user. Do not start a daemon and do not use `sudo`.

## Start here

```bash
docker version
docker info
docker ps
```

All three work as the current user; `DOCKER_HOST` is already exported. `docker info` is the check that
matters: it reports which engine you are talking to and where its storage lives.

## Rules

- Never run `dockerd`, `dockerd-rootless.sh` or `dockerd-rootless-setuptool.sh`. A daemon started inside
  this sandbox cannot mount its own storage layers and fails with `operation not permitted`. The working
  engine is already provided; starting another one does not help and leaves a broken daemon behind.
- Never prefix `docker` with `sudo`. `sudo` drops `DOCKER_HOST`, after which the client falls back to
  `/var/run/docker.sock`, a socket that does not exist here.
- This engine is a separate container, so a bind-mounted path is resolved in the engine's filesystem, not
  yours. A path that exists only in this sandbox (for example `/tmp/...`) appears empty inside the
  container. Check before relying on one:
  `docker run --rm -v /some/path:/mnt busybox ls /mnt`
- Files the engine's containers write are owned by the daemon's uid, not by this user.
- Do not verify the setup with `docker run hello-world`: the image registry is unreliable from here. Use
  an image already listed by `docker images`.
- `permission denied` on a `/var/run/user/<uid>/docker.sock` socket means the daemon's uid does not match
  this user. That is a host misconfiguration: report it, do not work around it with `sudo`.
