# ADR 0006 — data-donation-task kept current via daily clean re-clone

**Status:** Accepted. Source: conversation.

## Decision

The `data-donation-task` repo is not bundled with either application. 
It is kept on the VM as a live clone and refreshed daily by a systemd timer that deletes the existing checkout (`rm -rf`) and re-clones from scratch, followed by `pnpm install --dangerously-allow-all-builds`. The default branch is `development`, overridable at deploy time.

## Implication

On every timer fire the directory is unavailable for a short window — dd-script-builder must tolerate this. `--dangerously-allow-all-builds` is required because some dependencies (esbuild, better-sqlite3) run native lifecycle scripts that pnpm blocks by default. Do not replace the clean re-clone with `git pull`; the wipe-and-clone approach avoids stale files, lock conflicts, and partial dependency trees.
