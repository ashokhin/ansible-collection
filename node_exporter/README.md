# Ansible Role: node_exporter


## Description

Deploy [Node Exporter](https://github.com/prometheus/node_exporter) monitoring system using Ansible.


## Requirements

- Ansible >= "2.10" (It might work on previous versions, but we cannot guarantee it)
- gnu-tar on Mac deployer host (`brew install gnu-tar`)


## Role Variables

Variables that are present in [defaults/main.yml](defaults/main.yml):
| Variable | Default Value | Description |
|---|---|---|
| `node_exporter_download_base_url` | `"https://github.com/prometheus/node_exporter/releases/download"` | Base URL to download Node Exporter binaries from |
| `node_exporter_version` | `"1.9.1"` | Version of Node Exporter to install |
| `node_exporter_archive_checksum` | `"sha256:becb950ee80daa8ae7331d77966d94a611af79ad0d3307380907e0ec08f5b4e8"` | Checksum of the Node Exporter archive for verification |
| `node_exporter_service_name` | `"node-exporter"` | Name of the Node Exporter's systemd service |
| `node_exporter_user` | `"prom-exporter"` | System user to run Node Exporter service |
| `node_exporter_group` | `"prom-exporter"` | System group to run Node Exporter service |
| `node_exporter_files_permissions` | `"0644"` | Permissions for Node Exporter config files |
| `node_exporter_directories_permissions` | `"0755"` | Permissions for Node Exporter directories |
| `node_exporter_base_path` | `"/opt/node_exporter"` | Base installation path for Node Exporter |
| `node_exporter_releases_path` | `"{{ node_exporter_base_path }}/releases"` | Path to store Node Exporter releases |
| `node_exporter_conf_path` | `"{{ node_exporter_base_path }}/conf"` | Path to store Node Exporter configuration files |
| `node_exporter_data_path` | `"{{ node_exporter_base_path }}/data"` | Path to store Node Exporter data |
| `node_exporter_alert_rules_path` | `"{{ node_exporter_conf_path }}/alert_rules"` | Path to store Node Exporter alert rules |
| `node_exporter_web_listen_port` | `"9100"` | Port for Node Exporter web interface |
| `node_exporter_web_listen_address` | `"0.0.0.0:{{ node_exporter_web_listen_port }}"` | Address for Node Exporter web interface |
| `node_exporter_web_external_url` | `""` | External URL for Node Exporter web interface |


## Example Playbook

```yaml
---
- name: Node Exporter role
  hosts: all
  roles:
    - role: node_exporter
...
```


## Example Run

```bash
ansible-playbook -i 001_inventory_example install_node_exporter.yml
```


## License

MIT
