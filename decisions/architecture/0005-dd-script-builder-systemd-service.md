# ADR 0005 — dd-script-builder runs as a systemd service (not Docker)

**Status:** Accepted. Source: conversation.

## Decision

dd-script-builder (FastAPI/Python) runs directly on the host as a systemd service managed by uvicorn, not inside Docker. 
It is installed into a Python 3.14 virtualenv under `{{ dd_script_builder_dir }}/.venv` and started on port 8000.

## Implication

The service requires Python 3.14, Node.js v24, pnpm, and poetry to be present on the host (installed by the same role). 
The systemd unit file is templated to `/etc/systemd/system/dd-script-builder.service`. The `TASK_SOURCE` env var points to `data_donation_task_dir` so the builder can locate the task repo at runtime.
