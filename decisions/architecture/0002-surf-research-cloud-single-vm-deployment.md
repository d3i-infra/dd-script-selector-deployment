# ADR 0002 — SURF Research Cloud single-VM deployment

**Status:** Accepted. Source: conversation.

## Decision

The system is deployed on a single SURF Research Cloud VM. SURF Research Cloud's provisioning mechanism executes Ansible playbooks locally on the VM (`connection: local`, `hosts: 127.0.0.1`), which is the native style for SRC components. All components (Nginx, dd-script-selector, dd-script-builder, data-donation-task) run on that one VM.

## Implication

Playbooks must be written in SRC's local-execution style — `hosts: 127.0.0.1`, `connection: local`, `become: yes`. There is no remote control node. Horizontal scaling is out of scope for this research tool.
