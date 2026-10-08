# Ansible OpenStack Additional Services

Ansible roles that install the OpenStack services Neutron (networking), Horizon (dashboard), and Cinder (block storage) on an Ubuntu server.

## What this covers
- One role per service: `Neutron`, `Horizon`, `Cinder`
- Package installation with `apt` and base configuration files written with the `copy` module
- Reloading Apache after installing the Horizon dashboard
- A `pre_tasks` system update before the role play

## Lab environment
- Control node: Ubuntu workstation running Ansible
- Managed node: one Ubuntu server (VirtualBox, host-only network)

## Repository structure
```
.
├── ansible.cfg
├── inventory
├── site.yml            # pre_tasks + role play
└── roles/
    ├── Cinder/tasks/main.yml
    ├── Horizon/tasks/main.yml
    └── Neutron/tasks/main.yml
```

## Usage
```bash
ansible-playbook --ask-become-pass site.yml
```

## Verification
The playbook completed with no failures (`ok=11 changed=8 failed=0`).

| Service | Check | Result |
|---------|-------|--------|
| Horizon | Browse to `http://<host>/horizon` | OpenStack Dashboard login page loads |
| Cinder | `systemctl status cinder-backup` | active (running) |
| Neutron | `systemctl status neutron-linuxbridge-agent` | active (running) |

## Notes and next steps
- The configuration files written by the Cinder and Neutron roles are minimal stubs. The full settings from the OpenStack install guide (database connections, message queue, service credentials) are not yet automated.
- Service credentials should come from Ansible Vault variables rather than the role files.
- Only part of each service is installed here (for example the Neutron Linux bridge agent and Cinder API/backup), without the full controller and storage-node packages. Separate controller, compute, and storage plays are the next step.
- Everything targets a single `Ubuntu` host group.
