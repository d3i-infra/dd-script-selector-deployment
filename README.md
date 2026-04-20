# dd-script-selector-deployment

Ansible deployment repo for the dd-script-selector system, intended for deployment on a [SURF Research Cloud](https://www.surf.nl/en/surf-research-cloud) workspace (single VM).

## Components

- **Nginx** — reverse proxy, handles SSL
- **dd-script-selector** — Phoenix web app (Docker)
- **dd-script-builder** — FastAPI service (systemd)
- **data-donation-task** — task definitions mounted into the Phoenix container
