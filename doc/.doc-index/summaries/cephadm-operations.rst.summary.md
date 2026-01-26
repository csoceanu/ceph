### Documentation Summary: `cephadm/operations.rst`

This document serves as the operational manual for **cephadm**, the container-based orchestrator for Ceph. it provides the command-line procedures and configuration settings necessary to manage the lifecycle of a Ceph cluster, its daemons, and its underlying host infrastructure.

---

#### 1. Primary Purpose
The file documents **Day 2 operations** for Ceph clusters managed by cephadm. It focuses on administrative tasks such as service manipulation, log management, health monitoring, hardware maintenance (disk replacement), and security configuration.

#### 2. Key Topics Covered
*   **Logging Infrastructure**: How to watch real-time cluster logs, toggle debug levels, and navigate the transition from `stderr`/`journald` to file-based logging.
*   **Daemon Lifecycle**: Commands for starting, stopping, restarting, redeploying, and reconfiguring specific daemons or entire services.
*   **Health Checks**: Detailed breakdown of cephadm-specific health indicators (e.g., stray hosts, failed checks) and optional cluster-wide configuration consistency scans (MTU, kernel version, etc.).
*   **Keyring and Config Management**: Automated distribution of `ceph.conf` and client auth keys across the cluster using placement specs.
*   **Infrastructure Maintenance**: Replacing OSD devices, configuring restricted `sudo` access for security hardening, and purging entire clusters.
*   **Data Layout**: Standardized file system paths for daemon data, logs, and crash reports in a cephadm environment.

#### 3. Technical Keywords
*   **Commands**: `ceph orch`, `cephadm bootstrap`, `ceph -W cephadm`, `journalctl`, `ceph log last`, `ceph orch device replace`.
*   **Configuration Keys**: `mgr/cephadm/log_to_cluster_level`, `mgr/cephadm/cephadm_log_destination`, `manage_etc_ceph_ceph_conf`, `config_checks_enabled`.
*   **Daemons/Services**: `mon`, `mgr`, `osd`, `mds`, `rgw`.
*   **Health Codes**: `CEPHADM_PAUSED`, `CEPHADM_STRAY_HOST`, `CEPHADM_CHECK_MTU`, `CEPHADM_CHECK_LINKSPEED`.
*   **Paths**: `/var/lib/ceph/<fsid>`, `/var/log/ceph/`, `/etc/ceph/`.

#### 4. Target Audience
*   **System Administrators**: Responsible for cluster uptime and maintenance.
*   **SREs/DevOps Engineers**: Automating Ceph deployments and monitoring.
*   **Support Engineers**: Troubleshooting daemon failures or performance inconsistencies.

#### 5. Related Concepts
*   **Ceph Orchestrator (MGR module)**: The engine driving these commands.
*   **Containers (Podman/Docker)**: The underlying technology for daemon isolation.
*   **LVM**: Specifically referenced in the OSD device replacement section.
*   **CRUSH Maps**: Mentioned in the context of safety during OSD restarts.

---

### Update Triggers for AI Maintenance
This file should be updated if code changes occur in the following areas:

1.  **CLI Changes**: Any modification to `ceph orch` subcommands or arguments (e.g., adding a new flag to `redeploy`).
2.  **New Health Checks**: If the `cephadm` manager module introduces new health warning codes or configuration consistency checks.
3.  **Pathing Changes**: If the default storage locations for logs, configs, or daemon data in `/var/lib/ceph` are altered.
4.  **Logging Logic**: Changes to how `cephadm` interacts with `journald`, `syslog`, or local files.
5.  **Security/Sudo Requirements**: If the internal `cephadm` binary requires new binary execution permissions (e.g., adding `systemctl` or a new utility to the required sudoers list).
6.  **Orchestrator Logic**: Changes to how keyrings are distributed or how "stray" daemons are identified.