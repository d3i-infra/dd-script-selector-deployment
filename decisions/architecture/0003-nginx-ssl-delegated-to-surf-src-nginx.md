# ADR 0003 — Nginx SSL delegated to SURF SRC-NGINX

**Status:** Accepted. Source: conversation.

## Decision

SSL termination is handled by SURF Research Cloud's `SRC-NGINX` component, which owns `/etc/nginx/conf.d/ssl_main.conf`. This playbook does not manage certificates or the SSL server block itself — it patches the existing file by injecting an Ansible-managed `location /` block and proxy headers into it. Nginx logs are redirected to rsyslog (`facility=local7,tag=nginx`), also expected by the SRC environment.

## Implication

Do not replace or recreate `ssl_main.conf`; only ever patch it. Removing the SRC-NGINX component from the workspace will break SSL.
