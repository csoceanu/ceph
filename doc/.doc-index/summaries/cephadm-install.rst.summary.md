This documentation file, `cephadm/install.rst`, serves as the primary guide for deploying a new Ceph cluster using the `cephadm` orchestrator. It covers the end-to-end lifecycle of initial setup, from host preparation to the creation of the first monitor and the expansion of the cluster.

### 1. Primary Purpose
The file provides the authoritative instructions for **bootstrapping a new Ceph cluster**. It explains how to install the `cephadm` tool, prepare the first host, and execute the bootstrap process to establish a working management plane (Monitor and Manager daemons).

### 2. Key Topics Covered
*   **System Requirements**: Dependencies like Python 3.6+, Systemd, Container Engines (Podman/Docker), and LVM2.
*   **Installation Methods**: Binary acquisition via distribution-specific package managers (`apt`, `dnf`, `zypper`) or manual `curl` downloads.
*   **The Bootstrap Process**: Using `cephadm bootstrap` to initialize the first node, generate SSH keys, and create the `client.admin` keyring.
*   **CLI Access**: Methods to access the Ceph shell via containers or host-installed packages.
*   **Cluster Expansion**: Basic workflows for adding hosts, additional Monitors, and storage (OSDs).
*   **Deployment Scenarios**: 
    *   Single-host configurations for testing.
    *   **Air-gapped/Isolated Environments**: Using custom/local container registries.
    *   **Custom/Advanced SSH**: Using specific private keys or CA-signed SSH certificates for inter-node communication.
*   **Resource Management**: OSD memory autotuning and converged infrastructure settings.

### 3. Technical Keywords
*   **Tools/Commands**: `cephadm`, `cephadm bootstrap`, `cephadm shell`, `ceph orch`, `podman`, `docker`, `ceph-common`.
*   **Configuration Files**: `ceph.conf`, `ceph.pub`, `ceph.client.admin.keyring`, `initial-ceph.conf`.
*   **Parameters**: `--mon-ip`, `--cluster-network`, `--ssh-user`, `--registry-json`, `--single-host-defaults`, `--ssh-signed-cert`.
*   **Environment**: CIDR notation, Python 3.6+, LVM2, `journald`.

### 4. Target Audience
*   **System Administrators**: Responsible for deploying and managing storage infrastructure.
*   **DevOps Engineers**: Integrating Ceph into automated deployment pipelines or containerized environments.
*   **Site Reliability Engineers (SREs)**: Setting up production-grade clusters with specific security requirements (CA-signed keys, air-gapping).

### 5. Related Concepts
*   **Ceph Orchestrator (cephadm)**: The underlying framework that manages the lifecycle of all Ceph services.
*   **Containerization**: Since Ceph daemons run as containers, this relates to Podman/Docker lifecycle management.
*   **RADOS**: The underlying storage architecture that is established during this install process.
*   **Monitoring Stack**: Prometheus, Grafana, and Alertmanager (mentioned in the context of air-gapped image configuration).

---

### Maintenance Triggers: When to update this file
An AI system should flag this file for updates if code changes occur in the following areas:
1.  **Dependency Changes**: If the minimum Python version increases (currently 3.6) or if a new system dependency is added (e.g., moving away from LVM2).
2.  **CLI Argument Changes**: If the `bootstrap` or `add-repo` subcommands gain new required arguments or if existing flags (like `--mon-ip`) are deprecated.
3.  **Default Value Shifts**: If the default memory autotune ratio (currently 0.7) or default OSD settings change.
4.  **Distribution Support**: If new Linux distributions are supported or if the package names (e.g., `centos-release-ceph-quincy`) change for new releases.
5.  **Security/SSH Logic**: If the method for handling SSH keys or the directory structure for `/etc/ceph` is modified in the source code.
6.  **Container Logic**: If the way `cephadm` interacts with registries or handles image pulling (e.g., `--registry-json` structure) is updated.