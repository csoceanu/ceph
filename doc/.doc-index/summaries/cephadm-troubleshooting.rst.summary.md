This documentation provides a technical guide for troubleshooting Ceph clusters deployed using **cephadm**, specifically focusing on the challenges of managing containerized daemons versus traditional package-based installations.

### 1. Primary Purpose
The file serves as the definitive reference for diagnosing and resolving failures in a **cephadm-managed** Ceph cluster. It outlines the specific workflows required to interact with containerized services, manage the orchestrator's behavior during crises, and recover from critical states like loss of monitor quorum.

### 2. Key Topics Covered
*   **Orchestrator Control**: Commands to pause or disable the cephadm background processes to prevent interference during manual troubleshooting.
*   **Event Logging**: How to access metadata-rich events at both the service level and individual daemon level.
*   **Log Management**: Transitioning from `/var/log/` to `journald` and using `cephadm logs` to fetch logs across hosts.
*   **Systemd Integration**: Mapping containerized services to systemd units on the host.
*   **Connectivity & Networking**: Troubleshooting SSH authentication between the manager and hosts, and resolving CIDR/public network inference errors.
*   **Advanced Recovery**: 
    *   Manual deployment of a Manager (MGR) when none exist.
    *   Restoring Monitor (MON) quorum by manually editing the monmap.
*   **Deep Debugging**: Procedures for capturing core dumps, using `gdb` inside specialized debug containers, and attaching to live processes.

### 3. Technical Keywords
*   **Orchestrator Commands**: `ceph orch pause`, `ceph orch ls`, `ceph orch ps`, `ceph orch set backend`.
*   **Cephadm CLI**: `cephadm logs`, `cephadm shell`, `cephadm enter`, `cephadm unit`, `cephadm ls`.
*   **Configuration Keys**: `mgr/cephadm/ssh_identity_key`, `mgr/cephadm/pause`, `public_network`.
*   **Tools**: `journalctl`, `podman`/`docker`, `jq`, `gdb`, `systemd-coredump`, `ceph-monstore-tool`.
*   **Files/Paths**: `/var/lib/ceph/<fsid>/<service>/unit.run`, `authorized_keys`, `ceph.cephadm.log`.

### 4. Target Audience
*   **Ceph Administrators**: Primary users responsible for cluster health.
*   **Site Reliability Engineers (SREs)**: Users automating recovery or monitoring.
*   **Support Engineers**: Users needing to perform "post-mortem" analysis on failed daemons or core dumps.

### 5. Related Concepts
*   **Container Orchestration**: Relates to how Ceph encapsulates logic within Podman/Docker.
*   **Systemd**: Relates to host-level service management and lifecycle.
*   **RADOS**: Relates to low-level monitoring via admin sockets and monitor store tools.
*   **SSH/Security**: Relates to the authentication layer required for the orchestrator to manage remote nodes.

---

### Maintenance Triggers for AI (Update Indicators)
This file should be updated if code changes occur in the following areas:
*   **Logging Backend**: If cephadm moves away from `journald` or changes the default log path.
*   **CLI Syntax**: Changes to `ceph orch` or `cephadm` subcommands or output formats (especially YAML structure).
*   **Dependency Changes**: If the container engine changes (e.g., moving from Podman to a different runtime) or if the base OS for containers changes (affecting `dnf` or `apt` commands in debugging).
*   **Recovery Procedures**: If the logic for forming a Monitor quorum or deploying a "bootstrap" Manager daemon changes.
*   **File System Hierarchy**: Changes to where `unit.run` files or admin sockets (`.asok`) are stored on the host.