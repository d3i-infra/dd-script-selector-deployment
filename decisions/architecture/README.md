# Architectural decisions

This directory holds the project's Architectural Decision
Records (ADRs). The framework and conventions are defined in
[`0001-adr-framework-and-conventions.md`](./0001-adr-framework-and-conventions.md);
read it first.

## Index

### Meta

- [0001 — ADR framework and conventions](./0001-adr-framework-and-conventions.md)

### Deployment topology

- [0002 — SURF Research Cloud single-VM deployment](./0002-surf-research-cloud-single-vm-deployment.md)
- [0003 — Nginx SSL delegated to SURF SRC-NGINX](./0003-nginx-ssl-delegated-to-surf-src-nginx.md)

### Components

- [0004 — dd-script-selector runs as a Docker container](./0004-dd-script-selector-docker.md)
- [0005 — dd-script-builder runs as a systemd service](./0005-dd-script-builder-systemd-service.md)
- [0006 — data-donation-task kept current via daily clean re-clone](./0006-data-donation-task-daily-clean-reclone.md)
- [0007 — Runtimes pinned and installed out of band](./0007-pinned-runtimes-installed-out-of-band.md)

## How to use this directory

When working on playbooks in this repo, load the ADR(s) whose
filenames match the area you are touching. The four-digit
prefix is for ordering and stable reference; the slug is for
topic-recognition.

When a decision surfaces in conversation that warrants an
ADR but does not yet have one, prompt the user to escalate.
Do not silently encode a decision into playbooks without an
ADR to back it.
