# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Ansible deployment repo for the dd-script-selector system, deployed on a single VM.

## Architecture

On the VM:

- **Nginx** — reverse proxy, handles SSL, forwards traffic to the Phoenix app
- **dd-script-selector** — Phoenix web app (Docker, port 4000). Users interact with it to generate a script config.
- **dd-script-builder** — FastAPI/Python API (systemd + uvicorn, port 8000). Called by the Phoenix app when a user finalizes a config; produces a zip file that is served back to the user.
- **data-donation-task** — cloned from GitHub; used by dd-script-builder at runtime.

## Running the playbooks

Playbooks are executed on the VM itself (connection: local). Requires `domain_name` to be set.

```bash
# Full deploy (all components)
ansible-playbook playbooks/main.yaml -e domain_name=<your-fqdn>

# Deploy only dd-script-builder
ansible-playbook playbooks/main.yaml --tags dd_script_builder_tag -e domain_name=<your-fqdn>

# Deploy only nginx + dd-script-selector (Docker)
ansible-playbook playbooks/main.yaml --tags nginx_tag,docker_tag -e domain_name=<your-fqdn>

# Deploy only data-donation-task (clone + install daily sync timer)
ansible-playbook playbooks/main.yaml --tags data_donation_task_tag -e domain_name=<your-fqdn>

# Override branch (default: development)
ansible-playbook playbooks/main.yaml --tags data_donation_task_tag -e domain_name=<your-fqdn> -e data_donation_task_branch=main
```

## Playbook structure

```
playbooks/
  main.yaml                         # nginx, docker, dd-script-builder, dd-script-selector, data-donation-task
  roles/
    nginx/    # redirects nginx logs to rsyslog (expects SRC-NGINX from SURF)
    docker/   # installs Docker via get.docker.com
    dd-script-builder/
      tasks/  # installs Python 3.14, Node v24, pnpm, poetry; clones + venvs + systemd
      templates/
        dd-script-builder.service.j2
      defaults/main.yaml            # repo URL, dirs, port (8000)
    data-donation-task/
      tasks/  # clones repo, installs sync service + timer
      templates/
        data-donation-task-sync.service.j2
        data-donation-task-sync.timer.j2
      defaults/main.yaml            # repo URL, branch (development), dir
```

## Notes

- Nginx config assumes SURF Research Cloud's `SRC-NGINX` component manages `/etc/nginx/conf.d/ssl_main.conf`
- `secret_key_base` is hardcoded in `main.yaml` — rotate if needed
- `dd-script-builder` exposes `REPO_SOURCE` env var pointing to `data_donation_task_dir`
