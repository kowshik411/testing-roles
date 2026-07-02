# apache2_static_deploy
=========

An Ansible role that automates the installation of the **Apache2 web server** and deploys a **custom static HTML page** to the default web root on Debian/Ubuntu-based systems. This role is ideal for quickly provisioning a web server and serving static content without any manual intervention.

---

## Overview

This role performs the following actions in order:

1. **Installs Apache2** — Uses `apt` to install the `apache2` package, ensuring the package cache is refreshed before installation.
2. **Deploys a custom index page** — Copies a pre-built `index.html` file to `/var/www/html/`, replacing the default Apache welcome page with your own static content.

The result is a fully configured, running Apache2 web server serving your custom HTML page, provisioned entirely through Ansible.

---

## Requirements

- **Target OS**: Debian or Ubuntu (uses `ansible.builtin.apt` — not compatible with RedHat/CentOS/Fedora without modification)
- **Privileges**: Tasks require elevated privileges. Run the playbook with `become: true`
- **Ansible version**: `>= 2.2`
- **Python**: Must be available on the managed node (used by Ansible for module execution)
- No additional Python packages or external dependencies are required beyond a standard Ansible installation

---

## Role Variables

This role currently has no user-configurable variables. All behavior is defined directly in the tasks.

| Variable | Location | Default | Description |
|----------|----------|---------|-------------|
| *(none)* | — | — | No variables are exposed at this time |

Future versions of this role may expose variables such as:
- `apache_document_root` — Path to the web root directory
- `apache_index_file` — Name of the index file to deploy
- `apache_package_state` — Desired state of the `apache2` package (`present`, `latest`)

---

## Dependencies

This role has **no dependencies** on other Ansible Galaxy roles. It relies solely on the `ansible.builtin.apt` and `ansible.builtin.copy` modules, which are part of `ansible-core`.

---

## Role Structure

```
first-role/
├── defaults/
│   └── main.yml          # Default variables (currently empty)
├── files/
│   └── index.html        # Static HTML page deployed to /var/www/html/
├── handlers/
│   └── main.yml          # Handlers (currently empty)
├── meta/
│   └── main.yml          # Role metadata for Ansible Galaxy
├── tasks/
│   └── main.yml          # Core tasks: install apache2, deploy index.html
├── tests/
│   ├── inventory         # Test inventory
│   └── test.yml          # Test playbook
├── vars/
│   └── main.yml          # Role variables (currently empty)
└── README.md
```

---

## Example Playbook

### Basic Usage

```yaml
---
- hosts: webservers
  become: true
  roles:
    - kowshik411.apache2_static_deploy
```

### With an Inventory File

```ini
# inventory.yml
[webservers]
web-node-1.example.com
web-node-2.example.com
```

```bash
ansible-playbook -i inventory.yml site.yml
```

### Inline Role Reference

```yaml
---
- hosts: all
  become: true
  roles:
    - role: kowshik411.apache2_static_deploy
```

---

## What Gets Deployed

The `files/index.html` served by this role is a clean, minimal static page that includes:

- A styled header announcing the purpose of the deployment
- A body section describing the Ansible learning context
- A fixed footer with copyright information

The page is deployed to `/var/www/html/index.html` with the following permissions:

| Property | Value  |
|----------|--------|
| Owner    | `root` |
| Group    | `root` |
| Mode     | `0644` |

---

## Testing

A basic test playbook is included under `tests/`:

```bash
ansible-playbook -i tests/inventory tests/test.yml
```

---

## License

MIT

---

## Author Information

**Kowshik Dabbeeru**
Engineer at PhonePe | Ansible Learner

- GitHub: [github.com/kowshik411](https://github.com/kowshik411)
- Ansible Galaxy: [galaxy.ansible.com/kowshik411](https://galaxy.ansible.com/kowshik411)
- Learning resource: [Ansible Zero to Hero by Abhishek Veeramalla](https://www.youtube.com/abhishekveeramalla)

---

*This role was created as part of an Ansible learning journey, covering practical automation of web server provisioning and static content deployment.*
