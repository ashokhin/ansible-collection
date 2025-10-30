# ansible-collection

Lightweight Ansible Collection providing reusable roles for installation and configuration of monitoring stack of:
- Prometheus
- Alertmanager
- Node Exporter
- Grafana
- HAProxy


## Contents
- Roles
    - `prometheus`: install and manage Prometheus monitoring system
    - `alertmanager`: install and manage Alertmanager for Prometheus
    - `node_exporter`: install and manage Prometheus Node Exporter
    - `grafana`: install and manage Grafana dashboarding system
    - `haproxy`: install and manage HAProxy proxy server


## Requirements
- Ansible 2.10+
- Python 3.8+
- Supported OS: Ubuntu 20.04+, Debian 10+


## Usage

Add the inventory files. Example present in [001_inventory_example/](/001_inventory_example/) directory.

Add hosts to relevant groups in the inventory file [001_inventory_example/hosts](/001_inventory_example/hosts).

Run the playbook to apply the roles.

For example, for the Prometheus installation run:
```bash
ansible-playbook -i 001_inventory_example install_prometheus.yml
```

For example, for all stack components, run:
```bash
ansible-playbook -i 001_inventory_example install_all.yml
```


## Role defaults
Each role exposes sensible defaults; override via role vars or play vars.
Check for details in each role's README below:
- [prometheus/README.md](prometheus/README.md)
- [alertmanager/README.md](alertmanager/README.md)
- [node_exporter/README.md](node_exporter/README.md)
- [grafana/README.md](grafana/README.md)
- [haproxy/README.md](haproxy/README.md)


## Contributing
- Fork the repo, create a feature branch and submit a pull request.
- Keep changes small and focused, include tests for new functionality.
- Follow existing code and role layout conventions.


## License
MIT License — see [LICENSE](LICENSE) file.


## Maintainer
GitHub: [@ashokhin](https://github.com/ashokhin)
