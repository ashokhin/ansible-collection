<p><img src="https://cdn.worldvectorlogo.com/logos/prometheus.svg" alt="prometheus logo" title="prometheus" align="right" height="60" /></p>


# Ansible Role: prometheus


## Description

Deploy [Prometheus](https://github.com/prometheus/prometheus) monitoring system using Ansible.


## Requirements

- Ansible >= "2.10" (It might work on previous versions, but we cannot guarantee it)
- gnu-tar on Mac deployer host (`brew install gnu-tar`)


## Role Variables

Variables that are present in [defaults/main.yml](defaults/main.yml):
| Variable | Default Value | Description |
|---|---|---|
| `prometheus_download_base_url` | `"https://github.com/prometheus/prometheus/releases/download"` | Base URL to download Prometheus binaries from |
| `prometheus_version` | `"3.5.0"` | Version of Prometheus to install |
| `prometheus_archive_checksum` | `"sha256:e811827af26d822afb09a4f28314f61b618b12cff5369835a67f674d8b46f39a"` | Checksum of the Prometheus archive for verification |
| `prometheus_keep_last_versions_number` | `3` | Number of last Prometheus versions to keep on the system |
| `prometheus_service_name` | `"prometheus"` | | Name of the Prometheus's systemd service |
| `prometheus_user` | `"prometheus"` | System user to run Prometheus service |
| `prometheus_group` | `"prometheus"` | System group to run Prometheus service |
| `prometheus_files_permissions` | `"0644"` | Permissions for Prometheus config files |
| `prometheus_directories_permissions` | | `"0755"` | Permissions for Prometheus directories |
| `prometheus_base_path` | `"/opt/prometheus"` | Base installation path for Prometheus |
| `prometheus_releases_path` | `"{{ prometheus_base_path }}/releases"` | Path to store Prometheus releases |
| `prometheus_conf_path` | `"{{ prometheus_base_path }}/conf"` | Path to store Prometheus configuration files |
| `prometheus_data_path` | `"{{ prometheus_base_path }}/data"` | Path to store Prometheus data |
| `prometheus_alert_rules_path` | `"{{ prometheus_conf_path }}/alert_rules"` | Path to store Prometheus alert rules |
| `prometheus_tsdb_retention_time` | `"365d"` | TSDB data retention period |
| `prometheus_web_listen_port` | `"9090"` | Port for Prometheus web interface |
| `prometheus_web_listen_address` | `"0.0.0.0:{{ prometheus_web_listen_port }}"` | Address for Prometheus web interface |
| `prometheus_web_external_url` | `""` | External URL for Prometheus web interface |
| `prometheus_config` | yaml structure | Prometheus configuration yaml structure described [below](#prometheus-default-configuration) |


### Prometheus Default Configuration
`prometheus_config` variable contains Prometheus configuration in yaml structure. Below is a default configuration used in this role:

```yaml
prometheus_config:
  global:
    scrape_interval: 15s
    evaluation_interval: 15s
  alerting:
    alertmanagers:
      - static_configs:
          - targets:
  rule_files:
  scrape_configs:
    - job_name: "prometheus"
      static_configs:
        - targets: ["localhost:9090"]
          labels:
            app: "prometheus"
```


## Example Playbook

```yaml
---
- name: Prometheus role
  hosts: all
  roles:
    - role: prometheus
...
```


## Example Run

```bash
ansible-playbook -i 001_inventory_example install_prometheus.yml
```


## License
MIT License — see [LICENSE](LICENSE) file.


## Maintainer
GitHub: [@ashokhin](https://github.com/ashokhin)
