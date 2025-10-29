<p><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1d/Human-dialog-warning.svg/2000px-Human-dialog-warning.svg.png" alt="alert logo" title="alert" align="right" height="60" /></p>


# Ansible Role: alertmanager


## Description

Deploy and manage Prometheus [alertmanager](https://github.com/prometheus/alertmanager) service using ansible.


## Requirements

- Ansible >= "2.10" (It might work on previous versions, but we cannot guarantee it)
- gnu-tar on Mac deployer host (`brew install gnu-tar`)


## Role Variables

Variables that are present in [defaults/main.yml](defaults/main.yml):

| Variable | Default Value | Description |
|---|---|---|
| `alertmanager_download_base_url` | `"https://github.com/prometheus/alertmanager/releases/download"`| Base URL to download Alertmanager binaries from |
| `alertmanager_version` | `"0.28.1"` | Version of alertmanager to install |
| `alertmanager_archive_checksum` | `"sha256:5ac7ab5e4b8ee5ce4d8fb0988f9cb275efcc3f181b4b408179fafee121693311"` | Checksum of the alertmanager archive for verification |
| `alertmanager_service_name` | `"alertmanager"` | Name of the alertmanager's systemd service |
| `alertmanager_user` | `"prometheus"` | System user to run alertmanager service |
| `alertmanager_group` | `"prometheus"` | System group to run alertmanager service |
| `alertmanager_files_permissions` | `"0644"` | Permissions for alertmanager config files |
| `alertmanager_directories_permissions` | `"0755"` | Permissions for alertmanager directories |
| `alertmanager_base_path` | `"/opt/alertmanager"` | Base installation path for alertmanager |
| `alertmanager_releases_path` | `"{{ alertmanager_base_path }}/releases"` | Path to store alertmanager releases |
| `alertmanager_conf_path` | `"{{ alertmanager_base_path }}/conf"` | Path to store alertmanager configuration files |
| `alertmanager_data_path` | `"{{ alertmanager_base_path }}/data"` | Path to store alertmanager data |
| `alertmanager_message_templates_path` | `"{{ alertmanager_conf_path }}/message_templates"` | Path to store alertmanager message templates |
| `alertmanager_data_retention` | `"365d"` | Data retention period for alertmanager |
| `alertmanager_web_listen_port` | `9093` | Port for alertmanager web interface |
| `alertmanager_web_listen_address` | `"0.0.0.0:{{ alertmanager_web_listen_port }}"` | Address for alertmanager web interface |
| `alertmanager_web_external_url` | `""` | External URL for alertmanager web interface (set this if Alertmanager is behind a reverse proxy or accessed via a custom domain, so links in notifications are correct) |
| `alertmanager_cluster_listen_address` | `""` | Address for alertmanager clustering |
| `alertmanager_config` | yaml structure | Alertmanager configuration yaml structure described [below](#alertmanager-default-configuration) |


### Alertmanager Default Configuration

`alertmanager_config` variable contains alertmanager configuration in yaml structure. Below is an example of default configuration used in this role:

```yaml
alertmanager_config:
  route:
    group_by: ['alertname']
    group_wait: 30s
    group_interval: 5m
    repeat_interval: 1h
    receiver: 'web.hook'
  receivers:
    - name: 'web.hook'
      webhook_configs:
        - url: 'http://127.0.0.1:5001/'
  inhibit_rules:
    - source_match:
        severity: 'critical'
      target_match:
        severity: 'warning'
      equal: ['alertname', 'dev', 'instance']

alertmanager_route:
  group_by: ['alertname']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 1h
  receiver: 'web.hook'
```


## Example Playbook

```yaml
---
- name: Alertmanager role
  hosts: all
  roles:
    - role: alertmanager
...
```


## Example Run

```bash
ansible-playbook -i 001_inventory_example install_alertmanager.yml
```


## License

MIT
