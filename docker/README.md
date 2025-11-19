<p><img src="https://cdn.worldvectorlogo.com/logos/docker-4.svg" alt="docker logo" title="docker" align="right" height="60" /></p>


# Ansible Role: docker


## Description

Deploy [Docker](https://github.com/docker/docker) engine on Linux hosts using Ansible.


## Requirements

- Ansible >= "2.10" (It might work on previous versions, but we cannot guarantee it)


## Role Variables

Variables that are present in [defaults/main.yml](defaults/main.yml):

| Variable | Default Value | Description |
|---|---|---|
| `docker_apt_gpg_key_url` | `"https://download.docker.com/linux/ubuntu/gpg"` | URL to download Docker APT GPG key from |
| `docker_apt_repo_url` | `"https://download.docker.com/linux/ubuntu"` | URL of Docker APT repository |
| `docker_apt_gpg_key_path` | `"/etc/apt/keyrings/docker.asc"` | Path to store Docker APT GPG key |
| `docker_apt_repo_component` | `"stable"` | Docker APT repository component |
| `docker_apt_repo_architecture` | `"amd64"` | Docker APT repository architecture |
| `docker_version` | `"latest"` | Version of Docker to install (set to "latest" to install the latest version) |
| `docker_packages` | [See below](#docker_packages-list) | List of Docker packages to install |

#### `docker_packages` list:
```yaml
docker_packages:
  - "docker-ce"
  - "docker-ce-cli"
  - "containerd.io"
  - "docker-buildx-plugin"
  - "docker-compose-plugin"
```

## Example Playbook

```yaml
---
- name: Docker role
  hosts: all
  roles:
    - role: docker
...
```


## Example Run

```bash
ansible-playbook -i 001_inventory_example install_docker.yml
```


## License
MIT License — see [LICENSE](LICENSE) file.


## Maintainer
GitHub: [@ashokhin](https://github.com/ashokhin)
