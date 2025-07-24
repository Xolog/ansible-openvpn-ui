# Ansible Role: openvpn-ui
=========

This Ansible role installs and configures [OpenVPN-UI](https://github.com/d3vilh/openvpn-ui) — a web-based UI for managing an existing OpenVPN server — as a **standalone binary** with a **systemd service**.

## Features
------------

- Downloads and installs Go (default: `1.21.5`)
- Clones and builds `openvpn-ui` and `qrencode`
- Creates `conf/app.conf` from template
- Creates systemd service for `openvpn-ui`
- Exports initial admin credentials via environment vars
- Enables and starts the service

Requirements
------------

- Target host must be Debian-based (Ubuntu/Debian). Tested only Ubuntu 22.04
- OpenVPN must already be installed and configured
- Python 3 and Ansible >= 2.9 on the control node

Role Variables
--------------
You can override the following defaults:

```yaml
# Go installation
go_version: "1.21.5"
go_tarball: "go{{ go_version }}.linux-amd64.tar.gz"
go_url: "https://golang.org/dl/{{ go_tarball }}"
go_install_dir: "/usr/local/go"

# Directories
build_dir: "/opt/openvpn-ui"
qrencode_dir: "{{ build_dir }}/qrencode"

# OpenVPN-UI config
openvpn_path: /etc/openvpn
easyrsa_path: /usr/share/easy-rsa/
enable_admin: true
run_mode: dev
openvpn_ui_user: openvpn-ui

# Environment variables for Go
go_env:
  PATH: "/usr/local/go/bin:{{ ansible_env.HOME }}/go/bin:{{ ansible_env.PATH }}"
  HOME: "{{ ansible_env.HOME }}"

# Initial UI credentials
OPENVPN_ADMIN_USERNAME: admin
OPENVPN_ADMIN_PASSWORD: password

# Optional (Google OAuth)
GOOGLE_CLIENT_ID: ""
GOOGLE_CLIENT_SECRET: ""
GOOGLE_REDIRECT_URL: ""
ALLOWED_DOMAINS: ""
```

Dependencies
------------

This role assumes OpenVPN is already deployed via sorrowless.ansible_openvpn [sorrowless.ansible_openvpn](https://github.com/sorrowless/ansible_openvpn), but not required. Maybe other way for OpenVPN.

Example Playbook
----------------

```yaml
- name: Install and configure OpenVPN-UI
  hosts: openvpn
  become: yes
  roles:
    - openvpn-ui
```

License
-------

Apache 2.0

Author Information
------------------

[Xolog](https://github.com/Xolog)
