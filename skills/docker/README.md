# Docker

Use the rootless Docker engine already provided to this sandbox. Use when building, running, or managing containers and images.

Forked from [OpenHands/extensions](https://github.com/OpenHands/extensions); the upstream version tells
the agent to start its own daemon with `sudo`, which cannot work in a sandbox that is already
given a sidecar engine.

## Triggers

This skill is activated by the following keywords:

- `docker`
- `container`

## Details

See [SKILL.md](SKILL.md). Summary: the engine comes from `DOCKER_HOST`; never start a daemon, never use
`sudo`, verify with `docker info`, and remember that bind-mounted paths are resolved on the engine side,
not in this sandbox.
