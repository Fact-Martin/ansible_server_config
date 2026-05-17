##  Case Study: Infrastructure-as-Code vs. Traditional Administration

### The Problem with Traditional Configuration (The Manual Approach)
In a traditional enterprise setup, a Systems Administrator manually builds servers using step-by-step documentation (Runbooks). This creates severe operational bottlenecks:
* **Configuration Drift:** Over time, two identical servers drift apart in configuration as manual patches and hotfixes are applied, causing hidden application crashes.
* **Human Error:** Missing a single permission line in `/etc/samba/smb.conf` or a single semicolon in `/etc/apache2/apache2.conf` can bring down production or leak corporate data.
* **Lack of Auditability:** There is no tracking mechanism to see who changed a configuration file, when it was changed, or why.
* **Scaling Bottlenecks:** Provisioning a web, storage, and monitoring stack takes hours per machine, making rapid disaster recovery or infrastructure scaling impossible.

### The Automated Solution (This Architecture)
This project solves those limitations by treating the entire server infrastructure entirely as code (**Infrastructure-as-Code**):


| Operational Category | Traditional Manual Configuration | This Automated Pipeline |
| :--- | :--- | :--- |
| **Deployment Speed** | 2–4 hours of manual, repetitive console commands. | **< 3 minutes** via a single non-interactive terminal execution string. |
| **Consistency / State** | High risk of Configuration Drift across different nodes. | **Idempotent Execution**; forces the server to match the code exactly. |
| **Security Integrity** | Human error can leave firewalls or vulnerable protocols open. | Hardened perimeters (UFW, TLS 1.3, Fail2Ban) are **automatically locked in**. |
| **Credential Safety** | Passwords often end up in clear text on the server or documentation. | All root and application layer secrets are **AES-256 encrypted via Vault**. |
| **Audit & History** | System modifications rely on manual, unverified team notes. | Full tracking history through **Git commit logs and pull requests**. |
| **Disaster Recovery** | Rebuilding a crashed machine requires starting over from scratch. | Running the playbook on a clean OS restores the environment **instantly**. |

# Secure Infrastructure-as-Code Linux Server Pipeline

##  Infrastructure Core Capabilities
* **Security & Hardening:** Perimeter isolation via UFW Firewall, brute-force shielding via Fail2Ban, and secure SSH administrative access profiles.
* **Web Services:** Apache2 HTTP/HTTPS server tuned with the Event MPM architecture, automated log rotation, and hardened SSL/TLS parameters.
* **Network File Hub:** Samba File Server provisioning with cryptographically mapped users and isolated departmental security shares.
* **Telemetry Stack:** Full metric pipelines deploying file-provisioned Prometheus data collection engines and custom Grafana visual dashboards.
* **CI/CD Automation:** Streamlined deployment cycles driven via Git, GitHub Actions pipelines, and local self-hosted runner networks.

##  Repository Directory Layout
* `roles/common/` - Core OS baseline configuration, server patches, and custom system MOTD banners.
* `roles/apache/` - Web server profiles, virtual hosts deployment, and directory security controls.
* `roles/samba/` - Enterprise file-sharing infrastructure and access permissions.
* `roles/monitoring/` - Automated metrics scraping and analytics visualization injection.
* `vars/vault.yml` - Secure AES-256 encrypted database housing all infrastructure credentials (Ansible Vault).

##  Execution & Deployment Instructions

To execute the infrastructure pipeline and apply the secure configuration states, run the orchestrator script with local execution elevation and credential decryption flags:

```bash
ansible-playbook --ask-become-pass --ask-vault-pass ubuntu_server_config.yml
```


