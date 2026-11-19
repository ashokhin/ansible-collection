# vuln_scanner

Installs Trivy and a systemd timer that scans running Docker images, optional filesystem paths and pending apt security updates once a day.
Results are written as Prometheus metrics into the node_exporter textfile collector directory.
Alert rules live in `prometheus/files/alert_rules/vulnerabilities.yml`.

Requires the `node_exporter` role with `node_exporter_textfile_path` equal to `vuln_scanner_textfile_path`.

Only vulnerabilities that already have a fix are counted (`vuln_scanner_ignore_unfixed`).
Per-vulnerability details go to the journal (`journalctl -u vuln-scanner`).
