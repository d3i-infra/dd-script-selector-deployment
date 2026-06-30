# ADR 0004 — dd-script-selector runs as a Docker container

**Status:** Accepted. Source: conversation.

## Decision

The dd-script-selector Phoenix app is cloned from GitHub, built into a Docker image on the VM, and run as a container with `--restart always` and `--network=host` on port 4000.

## Implication

Deployment rebuilds the image from source on every run (`docker build`), stops and removes the existing container, then starts a fresh one. 
There is no image registry — the image exists only on the VM. Configuration is passed via environment variables (`PHX_HOST`, `SECRET_KEY_BASE`, `BUILDER_BASE`, `PLATFORMS`).
