### Documentation Summary: `cephadm/install.rst`

#### 1. Primary Purpose
The file serves as the definitive guide for deploying a new Ceph cluster from scratch using **cephadm**. It covers the entire lifecycle from initial host preparation and tool installation to bootstrapping the first node and expanding the cluster with additional services.

#### 2. Key Topics Covered
*   **System Requirements**: Hardware/software prerequisites (Python 3.6+, Systemd, Podman/Docker, LVM2).
*   **Installation Methods**: Comparison of distribution-specific packages vs. the curl-based standalone executable.
*   **Bootstrapping**: Detailed steps to initialize the first Monitor and Manager daemons.
*   **Post-Bootstrap Configuration**: Enabling the Ceph CLI (via `shell` or `ceph-common`) and adding administrative labels.
*   **Cluster Expansion**: Adding new hosts, deploying additional Monitors, and provisioning storage (OSDs).
*   **Special Deployment Scenarios**: 
    *   Single-host (non-production) setups.
    *   Air-gapped (isolated) environments using custom/local registries.
    *   Custom SSH configurations (user-specified keys or CA-signed certificates).
*   **Performance Tuning**: Automatic OSD memory tuning and adjustments for hyperconverged infrastructure.

#### 3. Technical Keywords
*   **Commands**: `cephadm bootstrap`, `cephadm shell`, `cephadm add-repo`, `ceph orch host label add`, `ceph orch apply osd`.
*   **Configuration Options**: `--mon-ip`, `--cluster-network`, `--ssh-user`, `--registry-json`, `--single-host-defaults`, `--ssh-signed-cert`.
*   **Daemons/Services**: Monitor (MON), Manager (MGR), Object Storage Daemon (OSD).
*   **APIs/Tools**: Orchestrator CLI (`ceph orch`), Podman, Docker, SSH, LVM2.
*   **Files**: `/etc/ceph/ceph.conf`, `/etc/ceph/ceph.client.admin.keyring`, `ceph.pub`.

#### 4. Target Audience
*   **System Administrators**: Responsible for initial Ceph setup and host management.
*   **DevOps Engineers**: Automating storage deployments in containerized environments.
*   **SREs**: Troubleshooting installation issues or configuring air-gapped clusters.

#### 5. Related Concepts
*   **Ceph Orchestrator**: The underlying framework `cephadm` uses to manage services.
*   **Containerization**: Heavy reliance on Podman/Docker for daemon isolation.
*   **RADOS**: The base storage layer established during bootstrap.
*   **Monitoring Stack**: Deployment of Prometheus, Grafana, and Alertmanager as part of the bootstrap process.

---

### Update Triggers for AI Systems
This file should be updated if code changes occur in the following areas:

*   **Dependency Shifts**: Changes in minimum required Python versions, supported container runtimes (Podman/Docker), or mandatory system tools (LVM2).
*   **Bootstrap Logic**: Changes to the `cephadm bootstrap` command-line arguments, default daemon placement, or the automated generation of SSH keys and config files.
*   **Packaging & Distribution**: Updates to the URL structure for fetching `cephadm` via curl or changes in supported Linux distributions/package managers.
*   **Security/Authentication**: Modifications to how SSH certificates or CA-signed keys are handled, or changes to container registry authentication flow.
*   **Resource Management**: Adjustments to default memory autotuning ratios (`osd_memory_target_autotune`) or how hyperconverged flags affect resource allocation.
*   **CLI Tooling**: Changes in how `cephadm shell` or `ceph-common` interact with the host or the container environment.