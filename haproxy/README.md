<p><img src="https://raw.githubusercontent.com/haproxy/haproxy/master/doc/HAProxyCommunityEdition_60px.png" alt="haproxy logo" title="haproxy" align="right" height="60" /></p>

# Ansible Role: haproxy

## Description

Deploy [HAProxy](https://github.com/haproxy/haproxy) - free, very fast and reliable reverse-proxy offering high availability, load balancing, and proxying for TCP and HTTP-based applications.

## Requirements

- Ansible >= "2.10" (It might work on previous versions, but we cannot guarantee it)


## Role Variables

Variables that are present in [defaults/main.yml](defaults/main.yml):
| Variable | Default Value | Description |
|---|---|---|
| `haproxy_package` | `"haproxy"` | Name of the HAProxy package to install |
| `haproxy_version` | `"latest"` | Version of HAProxy to install (set to "latest" to install the latest available version) |
| `haproxy_service_name` | `"haproxy"` | Name of the HAProxy's systemd service |
| `haproxy_user` | `"haproxy"` | System user to run HAProxy service |
| `haproxy_group` | `"haproxy"` | System group to run HAProxy service |
| `haproxy_files_permissions` | `"0644"` | Permissions for HAProxy config files |
| `haproxy_directories_permissions` | `"0755"` | Permissions for HAProxy directories |
| `haproxy_conf_path` | `"/etc/haproxy/haproxy.cfg"` | Path to HAProxy main configuration file |
| `haproxy_config_default` | yaml structure | HAProxy main configuration yaml structure described [below](#haproxy-default-configuration) |


### HAProxy Default Configuration
`haproxy_config_default` variable contains HAProxy main configuration in yaml structure. Below is a default configuration used in this role:


```yaml
haproxy_config_default:
  global:
    params: |-
        log /dev/log    local0
        log /dev/log    local1 notice
        chroot /var/lib/haproxy
        stats socket /run/haproxy/admin.sock mode 660 level admin
        stats timeout 30s
        user haproxy
        group haproxy
        daemon
        ca-base /etc/ssl/certs
        crt-base /etc/ssl/private
        # See: https://ssl-config.mozilla.org/#server=haproxy&server-version=2.0.3&config=intermediate
        ssl-default-bind-ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
        ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
        ssl-default-bind-options ssl-min-ver TLSv1.2 no-tls-tickets
  defaults:
    params: |-
        log     global
        mode    http
        option  httplog
        option  dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
        errorfile 400 /etc/haproxy/errors/400.http
        errorfile 403 /etc/haproxy/errors/403.http
        errorfile 408 /etc/haproxy/errors/408.http
        errorfile 500 /etc/haproxy/errors/500.http
        errorfile 502 /etc/haproxy/errors/502.http
        errorfile 503 /etc/haproxy/errors/503.http
        errorfile 504 /etc/haproxy/errors/504.http
  userlists: []
  frontends: []
  backends: []
```


## Example Playbook

```yaml
---
- name: HAProxy role
  hosts: all
  roles:
    - role: haproxy
...
```


## Example Inventory

```yaml
---
haproxy_user: "haproxy"
haproxy_group: "haproxy"

haproxy_config:
  global:
    params: |-
        log /dev/log    local0
        log /dev/log    local1 notice
        chroot /var/lib/haproxy
        stats socket /run/haproxy/admin.sock mode 660 level admin
        stats timeout 30s
        user {{ haproxy_user }}
        group {{ haproxy_group }}
        daemon
        ca-base /etc/letsencrypt/live/mymonitoring.example.com
        crt-base /etc/letsencrypt/live/mymonitoring.example.com
        # See: https://ssl-config.mozilla.org/#server=haproxy&server-version=2.0.3&config=intermediate
        ssl-default-bind-ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384
        ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
        ssl-default-bind-options ssl-min-ver TLSv1.2 no-tls-tickets
  userlists:
    - name: "monitoring"
      params: |
        user monitor password 5up3r53cr3t
  frontends:
    - name: "haproxy_exporter"
      params: |
        bind 127.0.0.1:9101
        mode http
        http-request use-service prometheus-exporter
        no log
    - name: "fe_443"
      params: |
          bind *:443 ssl crt /etc/haproxy/ssl/fullchain.pem
          use_backend be_prometheus
  backends:
    - name: "be_prometheus"
      params: |
        option httpchk GET /-/healthy
        server server1 192.168.1.2:9090 check
        server server2 192.168.1.3:9090 check
```


## Example Run

```bash
ansible-playbook -i 001_inventory_example install_haproxy.yml
```


## License

MIT
