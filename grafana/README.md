<p><img src="https://cdn.worldvectorlogo.com/logos/grafana.svg" alt="grafana logo" title="grafana" align="right" height="60" /></p>


# Ansible Role: grafana


## Description

Deploy [Grafana](https://github.com/grafana/grafana) dashboarding system using Ansible.


## Requirements

- Ansible >= "2.10" (It might work on previous versions, but we cannot guarantee it)


## Role Variables

Variables that are present in [defaults/main.yml](defaults/main.yml):

| Variable | Default Value | Description |
|---|---|---|
| `grafana_apt_gpg_key_url` | `"https://apt.grafana.com/gpg.key"` | URL to download Grafana APT GPG key from |
| `grafana_apt_repo_url` | `"https://apt.grafana.com"` | Grafana APT repository URL |
| `grafana_package` | `"grafana-enterprise"` | Name of the Grafana package to install |
| `grafana_version` | `"latest"` | Version of Grafana to install (set to "latest" to install the latest available version) |
| `grafana_apt_gpg_key_path` | `"/etc/apt/keyrings/grafana.asc"` | Path to store Grafana APT GPG key |
| `grafana_apt_repo_suite` | `"stable"` | Grafana APT repository suite |
| `grafana_apt_repo_component` | `"main"` | Grafana APT repository component |
| `grafana_service_name` | `"grafana-server"` | Name of the Grafana's systemd service |
| `grafana_user` | `"grafana"` | System user to run Grafana service |
| `grafana_group` | `"grafana"` | System group to run Grafana service |
| `grafana_files_permissions` | `"0644"` | Permissions for Grafana config files |
| `grafana_directories_permissions` | `"0755"` | | Permissions for Grafana directories |
| `grafana_conf_path` | `"/etc/grafana/grafana.ini"` | Path to Grafana main configuration file |
| `grafana_provisioning_dir` | `"/etc/grafana/provisioning"` | Path to Grafana provisioning directory |
| `grafana_datasources` | `{}` | Grafana datasources |
| `grafana_config_default` | yaml structure | Grafana main configuration yaml structure described [below](#grafana-default-configuration) |


### Grafana Default Configuration
`grafana_config_default` variable contains Grafana main configuration in yaml structure. Below is a default configuration used in this role:

```yaml
grafana_config_default:
  app_mode: "production"
  instance_name: "${HOSTNAME}"

  paths:
    logs: "/var/log/grafana"
    data: "/var/lib/grafana"

  server:
    http_addr: "0.0.0.0"
    http_port: 3000
    domain: "{{ ansible_facts['fqdn'] | default(ansible_host) | default('localhost') }}"
    protocol: http
    enforce_domain: false
    socket: ""
    cert_key: ""
    cert_file: ""
    enable_gzip: false
    static_root_path: public
    router_logging: false
    serve_from_sub_path: false

  security:
    admin_user: admin
    admin_password: ""

  database:
    type: sqlite3

  users:
    allow_sign_up: false
    auto_assign_org_role: Viewer
    default_theme: dark

  auth: {}
```


## Example Playbook

```yaml
---
- name: Grafana role
  hosts: all
  roles:
    - role: grafana
...
```


## Example Run

```bash
ansible-playbook -i 001_inventory_example install_grafana.yml
```


## License
MIT License — see [LICENSE](LICENSE) file.


## Maintainer
GitHub: [@ashokhin](https://github.com/ashokhin)
