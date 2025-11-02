# Ansible Role: airline_manager


## Description

Deploy [Airline Manager](https://github.com/ashokhin/am4b) automated bot for the game "Airline Manager".


## Requirements

- Ansible >= "2.10" (It might work on previous versions, but we cannot guarantee it)
- Docker >= 28.5.0 (installed on target hosts)


## Role Variables

Variables that are present in [defaults/main.yml](defaults/main.yml):
| Variable | Default Value | Description |
|---|---|---|
| `airline_manager_image_name` | `"ashokhin/am4bot"` | Docker image name for Airline Manager bot |
| `airline_manager_image_version` | `"latest"` | Docker image version for Airline Manager bot |
| `airline_manager_service_name` | `"ambot"` | Systemd service name for Airline Manager bot |
| `airline_manager_user` | `"airlinemanager"` | System user to run Airline Manager bot |
| `airline_manager_group` | `"airlinemanager"` | System group to run Airline Manager bot |
| `airline_manager_files_permissions` | `"0600"` | Permissions for files created by the role |
| `airline_manager_directories_permissions` | `"0755"` | Permissions for directories created by the role |
| `airline_manager_conf_path` | `"/opt/ambot/conf"` | Path to Airline Manager bot configuration directory |
| `airline_manager_conf_file_name` | `"config.yaml"` | Airline Manager bot configuration file name |
| `airline_manager_web_listen_port` | `"9150"` | Port for Airline Manager bot web server |
| `airline_manager_inherit_default_config` | `true` | Whether to inherit default configuration values |
| `airline_manager_config_default` | See below | Default configuration for Airline Manager bot |

#### `airline_manager_config_default` default value:
```yaml
airline_manager_config_default:
  url: "https://www.airlinemanager.com/"
  username: ""
  password: ""
  log_level: "info"
  budget_percent:
    fuel: 70
    maintenance: 30
    marketing: 70
  good_price:
    fuel: 500
    co2: 120
  buy_catering_if_missing: true
  catering_amount_option: "20000"
  aircraft_wear_percent: 80
  aircraft_max_hours_to_check: 24
  aircraft_modify_limit: 3
  fuel_critical_percent: 20
  cron_schedule: "*/5 * * * *"
  services:
    - "company_stats"
    - "alliance_stats"
    - "staff_morale"
    - "hubs"
    - "buy_fuel"
    - "marketing"
    - "ac_maintenance"
    - "depart"
  timeout_seconds: 120
  chrome_headless: true
  prometheus_address: ":9150"
```

> [!NOTE]
>
> Check latest config options at https://github.com/ashokhin/am4b?tab=readme-ov-file#configuration

> [!WARNING]
>
> It is recommended to set at least `username` and `password` in your inventory or group_vars.


## Example Playbook

```yaml
---
- name: Airline Manager role
  hosts: all
  roles:
    - role: docker
    - role: airline_manager
...
```


## Example Run

```bash
ansible-playbook -i 001_inventory_example install_airline_manager.yml
```


## License
MIT License — see [LICENSE](LICENSE) file.


## Maintainer
GitHub: [@ashokhin](https://github.com/ashokhin)
