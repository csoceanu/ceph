This documentation file, `cephadm/install.rst`, serves as the primary technical guide for deploying a new Ceph cluster using the **cephadm** orchestrator. It covers the end-to-end lifecycle of the initial installation, from host preparation and tool acquisition to the "bootstrap" process and initial service scaling.

### 1. Primary Purpose
The file documents the **standard deployment workflow** for Ceph via `cephadm`. Its goal is to guide a user through setting up a functional cluster, starting with a single host and expanding to a multi-node, production-ready state.

### 2. Key Topics Covered
*   **System Requirements:** Prerequisites including Python 3.6+, container engines (Podman/Docker), time sync (Chrony), and LVM2.
*   **Installation Methods:** Comparing distribution-native packages (apt, dnf, zypper) vs. the standalone curl-based binary.
*   **Bootstrap Process:** Detailed breakdown of the `cephadm bootstrap` command, which initializes the first Monitor and Manager.
*   **Cluster Expansion:** Adding additional hosts, scaling Monitors, and provisioning OSDs (storage).
*   **Management Interfaces:** Enabling the Ceph CLI and using `cephadm shell`.
*   **Advanced Deployment Scenarios:** 
    *   Single-host setups for testing.
    *   **Air-gapped (isolated)** environments using local container registries.
    *   Custom SSH configurations (user-specified keys and CA-signed certificates).
*   **Memory Autotuning:** Configuration of OSD memory targets.

### 3. Technical Keywords
*   **APIs/Commands:** `cephadm bootstrap`, `cephadm shell`, `ceph orch apply osd`, `ceph orch host label add`, `cephadm add-repo`.
*   **Configuration Options:** `--mon-ip`, `--ssh-user`, `--registry-json`, `--single-host-defaults`, `--ssh-signed-cert`, `--log-to-file`.
*   **Daemons:** Monitor (MON), Manager (MGR), Object Storage Daemon (OSD).
*   **Storage/Network:** LVM2, CIDR notation (subnetting), `osd_memory_target_autotune`.
*   **Security:** `authorized_keys`, CA signed SSH keys, `client.admin.keyring`.

### 4. Target Audience
*   **System Administrators** and **Storage Engineers** responsible for deploying and maintaining Ceph clusters.
*   **DevOps Engineers** automating infrastructure setup.
*   **Developers** needing a local single-node Ceph environment for testing.

### 5. Related Concepts
*   **Containerization:** Heavily relies on Podman/Docker.
*   **Orchestration:** Directly relates to the `ceph orch` (Orchestrator CLI).
*   **Monitoring Stack:** Integrates with Prometheus, Grafana, and Alertmanager (especially in air-gapped sections).
*   **Sub-systems:** Connects to specific documentation for CephFS, RGW (Object Gateway), RBD, and iSCSI once the base cluster is live.

---

### Update Triggers for AI Maintenance
This file should be updated if any of the following code or environmental changes occur:
*   **Dependency Shifts:** If the minimum Python version increases or if a new system dependency (like a different volume manager) replaces LVM2.
*   **CLI Syntax Changes:** If `cephadm` subcommands or flags are renamed, deprecated, or added (especially in the `bootstrap` or `install` logic).
*   **Container Runtime Compatibility:** If specific versions of Podman or Docker are no longer supported or require new configuration tweaks (e.g., Live Restore).
*   **Default Behavior Changes:** If the default memory autotune ratios change or if the bootstrap process generates different sets of default files (e.g., moving away from `/etc/ceph`).
*   **Security Protocols:** Changes to how `cephadm` handles SSH, user permissions, or container registry authentication.
*   **New Distribution Support:** When new Linux distributions are officially supported or the package manager commands (apt/dnf) change for existing ones.