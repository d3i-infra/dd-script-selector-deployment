# ADR 0007 — Runtimes pinned and installed out of band

**Status:** Accepted. Source: conversation.

## Decision

Python 3.14 and Node.js v24 are pinned to specific versions and installed outside the system package manager. Python 3.14 comes from the deadsnakes PPA; Node.js v24.14.0 is downloaded as a pre-built tarball from nodejs.org and extracted into `/usr/local`. pnpm is installed globally via npm; poetry is installed via its official installer into `/usr/local`.

## Implication

Exact versions are hardcoded in the role tasks. Upgrading a runtime means updating the task (PPA package name or tarball URL) and re-running the playbook. Do not rely on the distro's default `python3` or `node` — they will be different versions.
