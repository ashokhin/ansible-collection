# Loki

Ansible role to install and configure [Grafana Loki](https://grafana.com/docs/loki/latest/).

## Requirements

None.

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `loki_download_base_url` | `"https://github.com/grafana/loki/releases/download"` | Base URL to download Loki binaries from |
| `loki_version` | `"3.6.11"` | Loki version to install |
| `loki_archive_checksum` | `"sha256:e879..."` | SHA256 checksum from [SHA256SUMS](https://github.com/grafana/loki/releases) |
| `loki_keep_last_versions_number` | `3` | Number of release directories to keep |
| `loki_service_name` | `"loki"` | Systemd service name |
| `loki_user` | `"loki"` | Service user |
| `loki_group` | `"loki"` | Service group |
| `loki_files_permissions` | `"0644"` | Permissions for config files |
| `loki_directories_permissions` | `"0755"` | Permissions for directories |
| `loki_base_path` | `"/opt/loki"` | Base installation directory |
| `loki_releases_path` | `"{{ loki_base_path }}/releases"` | Path to store release directories |
| `loki_conf_path` | `"{{ loki_base_path }}/conf"` | Path to configuration files |
| `loki_data_path` | `"{{ loki_base_path }}/data"` | Path to runtime data |
| `loki_chunks_path` | `"{{ loki_data_path }}/chunks"` | Path to chunk storage |
| `loki_rules_path` | `"{{ loki_data_path }}/rules"` | Path to ruler rules |
| `loki_web_listen_port` | `"3100"` | HTTP listen port |
| `loki_grpc_listen_port` | `"9096"` | gRPC listen port |
| `loki_web_listen_address` | `"0.0.0.0:{{ loki_web_listen_port }}"` | HTTP listen address |
| `loki_config` | see defaults | Loki configuration dict (rendered to `loki.yml`) |

## Dependencies

None.

## Example Playbook

```yaml
- name: Loki role
  hosts: loki_hosts
  roles:
    - role: loki
```

## License

MIT
