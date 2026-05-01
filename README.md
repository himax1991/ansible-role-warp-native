# Ansible Role: Cloudflare WARP Native

[![Ansible Galaxy](https://img.shields.io/badge/ansible--galaxy-himax1991.warp__native-blue.svg)](https://galaxy.ansible.com/himax1991/warp_native)
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)](LICENSE)

An Ansible role that installs and configures Cloudflare WARP using wgcf and WireGuard. It sets up the network interface and verifies handshake connectivity. Supports two states: `present` and `absent`.

**🇷🇺 [Русская версия документации](README_ru.md)**

## Requirements

- Ansible >= 2.14
- Target systems: Debian 11/12, Ubuntu 20.04/22.04/24.04

## Role Variables

### Main Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `warp_native_state` | `present` | State of WARP installation (`present`/`absent`) |
| `warp_native_modify_resolv` | `false` | Whether to modify /etc/resolv.conf |
| `warp_native_temp_nameservers` | `["1.1.1.1", "8.8.8.8"]` | Temporary DNS servers during setup |

### wgcf Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `warp_native_wgcf_version` | `latest` | Version of wgcf to install |
| `warp_native_wgcf_install_path` | `/usr/local/bin/wgcf` | Installation path for wgcf binary |

### WireGuard Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `warp_native_conf_dir` | `/etc/wireguard` | WireGuard configuration directory |
| `warp_native_conf_name` | `warp.conf` | WireGuard configuration file name (also determines interface name) |
| `warp_native_enable` | `true` | Enable WireGuard service on boot |
| `warp_native_keepalive` | `25` | WireGuard keepalive interval |
| `warp_native_table_off` | `true` | Disable WireGuard routing table |

> **Note:** The `warp_native_conf_name` variable determines both the configuration file name and the WireGuard interface name. For example, `warp.conf` creates interface `warp`, and `custom-vpn.conf` creates interface `custom-vpn`.

### Verification Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `warp_native_check_retries` | `10` | Number of connection check retries |
| `warp_native_check_delay` | `1` | Delay between retries (seconds) |

### Cleanup Settings

| Variable | Default | Description |
|----------|---------|-------------|
| `warp_native_purge_all` | `false` | Remove all WARP-related files on uninstall |
| `warp_native_remove_wireguard_pkg` | `false` | Remove WireGuard package on uninstall |
| `warp_native_cleanup_dns_backup` | `false` | Clean up DNS backup files |

## Installation

```bash
ansible-galaxy install himax1991.warp_native
```

## Example Usage

### Basic playbook

```yaml
- hosts: warp_servers
  become: yes
  roles:
    - himax1991.warp_native
```

### Group variables

Create group variables in `group_vars/warp_servers.yml`:

```yaml
# Install WARP
warp_native_state: present

# Optional: Modify DNS settings
warp_native_modify_resolv: true
warp_native_temp_nameservers:
  - "1.1.1.1"
  - "8.8.8.8"

# Optional: Custom WireGuard settings
warp_native_keepalive: 30
warp_native_conf_name: "company-warp.conf"  # Creates interface 'company-warp'
```

For removal, set `warp_native_state: absent` in group variables.

## License

MIT

## Author Information

This role was created by [Melbine](https://github.com/himax1991).
