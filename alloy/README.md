# Alloy

Ansible role to install and configure [Grafana Alloy](https://grafana.com/docs/alloy/latest/) — the official successor to Promtail and Grafana Agent.

## Requirements

None.

## Role Variables

| Variable | Default | Description |
|---|---|---|
| `alloy_download_base_url` | `"https://github.com/grafana/alloy/releases/download"` | Base URL to download Alloy binaries from |
| `alloy_version` | `"1.17.0"` | Alloy version to install |
| `alloy_archive_checksum` | `"sha256:a905..."` | SHA256 checksum from [SHA256SUMS](https://github.com/grafana/alloy/releases) |
| `alloy_keep_last_versions_number` | `3` | Number of release directories to keep |
| `alloy_service_name` | `"alloy"` | Systemd service name |
| `alloy_user` | `"alloy"` | Service user (added to `adm` and `systemd-journal` groups) |
| `alloy_group` | `"alloy"` | Service group |
| `alloy_files_permissions` | `"0644"` | Permissions for config files |
| `alloy_directories_permissions` | `"0755"` | Permissions for directories |
| `alloy_base_path` | `"/opt/alloy"` | Base installation directory |
| `alloy_releases_path` | `"{{ alloy_base_path }}/releases"` | Path to store release directories |
| `alloy_conf_path` | `"{{ alloy_base_path }}/conf"` | Path to configuration files |
| `alloy_data_path` | `"{{ alloy_base_path }}/data"` | Path to runtime data (WAL, etc.) |
| `alloy_http_listen_port` | `"12345"` | HTTP metrics listen port |
| `alloy_http_listen_address` | `"0.0.0.0:{{ alloy_http_listen_port }}"` | HTTP listen address |
| `alloy_loki_url` | `"http://localhost:3100/loki/api/v1/push"` | Loki push endpoint |
| `alloy_extra_config` | `""` | Raw Alloy River config appended to the generated `config.alloy` |

## Default config

The default `config.alloy` contains:
- Alloy self-metrics exposed on `alloy_http_listen_port` (for Prometheus scraping)
- `loki.write "default"` component pointed at `alloy_loki_url`

Use `alloy_extra_config` to add custom sources, processors, and pipelines per inventory group.

## Dependencies

None.

## Example Playbook

```yaml
- name: Alloy role
  hosts: alloy_hosts
  roles:
    - role: alloy
```

## License

MIT
