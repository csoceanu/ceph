This documentation file, `cephadm/upgrade.rst`, is the definitive guide for managing the lifecycle of a Ceph cluster's software versioning using the `cephadm` orchestrator.

### 1. Primary Purpose
The file documents the **automated and manual procedures for upgrading a Ceph cluster** using `cephadm`. It ensures that upgrades are performed according to "best practices" (e.g., specific daemon ordering) to maintain cluster availability and data integrity.

### 2. Key Topics Covered
*   **Upgrade Logic**: The automated sequence of upgrades (Managers → Monitors → OSDs → etc.).
*   **Operational Procedures**: How to start, monitor, and cancel upgrades.
*   **Safety Preparations**: Recommendations such as disabling the PG autoscaler and checking cluster health.
*   **CephFS Specifics**: Handling MDS upgrades by failing the FS to avoid performance degradation during the transition.
*   **Staggered Upgrades**: Methods to limit upgrades to specific hosts, services, or daemon types rather than the whole cluster at once.
*   **Troubleshooting**: Common error codes like `UPGRADE_NO_STANDBY_MGR` and `UPGRADE_FAILED_PULL`.
*   **Container Management**: Transitioning from Docker Hub to Quay.io and using custom/development container images.

### 3. Technical Keywords
*   **Commands**: `ceph orch upgrade start`, `ceph orch upgrade status`, `ceph orch upgrade stop`, `ceph orch apply mgr`, `ceph orch daemon redeploy`.
*   **Configuration/Options**: `--ceph-version`, `--image`, `--daemon-types`, `--services`, `--hosts`, `--limit`, `fail_fs`, `noautoscale`.
*   **Alerts/Errors**: `UPGRADE_NO_STANDBY_MGR`, `UPGRADE_FAILED_PULL`, `ENOENT: Module not found`.
*   **Registries**: `quay.io`, `docker.io`.
*   **Config Values**: `mon_target_pg_per_osd`, `container_image_base`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for maintaining cluster health and software versions.
*   **DevOps/SREs**: Using automation to manage infrastructure.
*   **Support Engineers**: Troubleshooting failed upgrade states.

### 5. Related Concepts
*   **Ceph Orchestrator (`cephadm`)**: The underlying framework managing containers.
*   **Placement Specs**: Used for targeting specific hosts during staggered upgrades.
*   **PG Autoscaler**: A feature that interacts with upgrades by potentially causing heavy I/O (splitting/merging) during version changes.
*   **MDS/CephFS**: Specific handling required due to the stateful nature of metadata servers.

---

### Maintenance Trigger for AI Systems
This file should be updated if code changes occur in the following areas:
1.  **Orchestrator Logic**: Changes to the mandatory upgrade order (e.g., adding a new daemon type like `smb` or `nvmeof`).
2.  **CLI Syntax**: Modifications to `ceph orch upgrade` subcommands or new flags for filtering (staggering) upgrades.
3.  **Default Registries**: Changes to where Ceph pulls default images (e.g., moving away from `quay.io`).
4.  **MDS Upgrade Workflow**: If the logic for handling `max_mds` or `fail_fs` during upgrades is modified.
5.  **Health Check Names**: If new `HEALTH_WARN` codes or upgrade-specific alerts are added to the Manager's orchestrator module.
6.  **Dependency Changes**: If the version requirements for `cephadm` or `ceph-common` packages on the host change relative to the cluster version.