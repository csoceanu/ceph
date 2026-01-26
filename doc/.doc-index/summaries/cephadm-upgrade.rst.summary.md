This documentation file, `cephadm/upgrade.rst`, serves as the definitive guide for performing automated and manual software upgrades of a Ceph cluster managed by `cephadm`.

### 1. Primary Purpose
The file documents the lifecycle of a Ceph upgrade, including pre-upgrade prerequisites, execution steps (both automated and staggered), monitoring progress, and troubleshooting common failure states. It ensures users can move between point releases or major versions while maintaining cluster availability.

### 2. Key Topics Covered
*   **Upgrade Logic & Order**: Explains the safe sequencing of daemon restarts (MGR $\to$ MON $\to$ OSD $\to$ MDS, etc.).
*   **Cluster Health Management**: Recommendations for handling PG Autoscalers and CephFS MDS daemons during the upgrade window.
*   **Execution Methods**: Standard automated upgrades versus "Staggered Upgrades" (filtering by host, service, or daemon type).
*   **Status Monitoring**: Using the CLI to track progress bars and logs.
*   **Error Handling**: Specific solutions for common alerts like `UPGRADE_NO_STANDBY_MGR` and `UPGRADE_FAILED_PULL`.
*   **Container Management**: Transitioning from Docker Hub to Quay.io and using customized/CI container images.

### 3. Technical Keywords
*   **Commands**: `ceph orch upgrade start`, `ceph orch upgrade status`, `ceph orch upgrade stop`, `ceph orch daemon redeploy`, `ceph config set mgr mgr/orchestrator/fail_fs true`.
*   **Configuration Options**: `--ceph-version`, `--image`, `noautoscale`, `max_mds`, `container_image_base`.
*   **States/Alerts**: `HEALTH_WARNING`, `UPGRADE_NO_STANDBY_MGR`, `UPGRADE_FAILED_PULL`, `ENOENT`.
*   **Staggered Parameters**: `--daemon-types`, `--services`, `--hosts`, `--limit`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for maintaining cluster health and security patches.
*   **DevOps/SREs**: Integrating Ceph upgrades into broader infrastructure automation.
*   **Support Engineers**: Troubleshooting failed upgrades or stuck orchestrator states.

### 5. Related Concepts
*   **Cephadm Orchestrator**: The underlying framework that executes the upgrade.
*   **Container Registries**: Quay.io and Docker Hub repository management.
*   **Placement Specs**: How `cephadm` targets specific nodes for daemon deployment.
*   **MGR Modules**: Specifically how the orchestrator module interacts with the cluster.

---

### Maintenance Triggers: When to Update This File
This file should be updated if any of the following code-level changes occur:
1.  **Upgrade Sequencing**: If the internal `cephadm` logic for daemon upgrade order changes (e.g., a new daemon type like `smb` or `nvmeof` is added to the sequence).
2.  **CLI Syntax**: If new flags are added to `ceph orch upgrade` or if existing flags are deprecated.
3.  **Default Registries**: If the default container registry or image path for Ceph components is relocated.
4.  **Health Check Logic**: If new `HEALTH_WARN` codes specific to upgrades are introduced in the MGR orchestrator module.
5.  **Prerequisites**: If a new version of Ceph introduces a mandatory pre-upgrade step (like the `noautoscale` recommendation).