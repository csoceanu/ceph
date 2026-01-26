### Documentation Summary: `cephadm/operations.rst`

This document serves as the operational manual for managing a Ceph cluster deployed via **cephadm**. It focuses on the day-to-day lifecycle management of daemons, log collection, health monitoring, and host-level configurations.

#### 1. Primary Purpose
To provide administrators with the commands and configuration procedures for managing Ceph daemons, troubleshooting via logs, maintaining cluster health, and handling hardware transitions (like disk replacement) within a cephadm-managed environment.

#### 2. Key Topics Covered
*   **Log Management**: Real-time monitoring of cephadm cluster logs, debug-level logging, and switching between journald and file-based logging.
*   **Daemon Control**: Starting, stopping, restarting, redeploying, and reconfiguring specific daemons or entire services.
*   **Security & Auth**: Rotating daemon authentication keys and managing client keyrings.
*   **Data Layout**: Standardized file system paths for daemon data, logs, and crash reports.
*   **Health Checks**: Descriptions of cephadm-specific health warnings (e.g., stray hosts/daemons) and optional cluster configuration consistency checks (MTU, Kernel version, etc.).
*   **Host Administration**: Managing `/etc/ceph/ceph.conf` distribution, limiting password-less `sudo` for the cephadm user, and cluster purging.
*   **Hardware Maintenance**: The automated workflow for replacing OSD devices.

#### 3. Technical Keywords
*   **Commands**: `ceph orch daemon`, `ceph orch client-keyring`, `ceph orch device replace`, `cephadm rm-cluster`, `journalctl`, `ceph -W cephadm`.
*   **Configuration Keys**: `mgr/cephadm/log_to_cluster_level`, `log_to_file`, `mgr/cephadm/config_checks_enabled`, `mgr/cephadm/manage_etc_ceph_ceph_conf`.
*   **Paths**: `/var/log/ceph/`, `/var/lib/ceph/<fsid>/`, `/etc/ceph/`.
*   **Health Codes**: `CEPHADM_STRAY_HOST`, `CEPHADM_PAUSED`, `CEPHADM_CHECK_MTU`, `CEPHADM_CHECK_KERNEL_LSM`.
*   **Orchestrator Components**: `mgr/cephadm`, `placement spec`, `serviceid`, `daemonid`.

#### 4. Target Audience
*   **Ceph Cluster Administrators**: Responsible for monitoring and maintaining cluster uptime.
*   **Site Reliability Engineers (SREs)**: Automating cluster maintenance and troubleshooting performance issues.
*   **Infrastructure Engineers**: Managing host-level prerequisites and storage hardware.

#### 5. Related Concepts
*   **Ceph Orchestrator**: The abstract layer (CLI) that cephadm implements.
*   **Containerization**: Since cephadm is container-based, logging and redeployment relate heavily to podman/docker runtimes.
*   **CRUSH Maps**: Mentioned regarding the safety of restarting OSDs.
*   **Host Management**: SSH access and host labels (e.g., `_admin`) used for deployment.

---

### When to Update This File (for AI/DevOps)
This file must be updated if code changes occur in the following areas:

1.  **Orchestrator CLI Syntax**: If arguments for `ceph orch` (daemon control, keyring management, or device replacement) are added, deprecated, or modified.
2.  **Health Check Logic**: If new cephadm health checks are added to the `mgr/cephadm` module or if the criteria for existing checks (like MTU or Kernel consistency) change.
3.  **Default File Paths**: If the standardized locations for logs (`/var/log/ceph`) or daemon data (`/var/lib/ceph`) are altered in the cephadm bootstrap or deployment code.
4.  **Logging Mechanism**: If the default logging target changes (e.g., moving away from journald) or if new log-level configuration keys are introduced.
5.  **Security/Sudo Requirements**: If the cephadm binary requires new Linux commands to be added to the `sudoers` list for non-root execution.
6.  **Bootstrap/Config Logic**: If the distribution logic for `ceph.conf` or client keyrings is modified (e.g., new default locations or new placement logic).