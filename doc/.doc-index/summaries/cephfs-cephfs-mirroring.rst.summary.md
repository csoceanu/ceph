This documentation describes the **CephFS Snapshot Mirroring** system, a mechanism introduced in the Ceph "Pacific" release for asynchronous, push-based replication of file system snapshots from a primary Ceph cluster to a secondary (remote) Ceph cluster.

### 1. Primary Purpose
The file serves as the definitive guide for configuring and managing the `cephfs-mirror` tool. Its goal is to enable disaster recovery and data redundancy by synchronizing specific CephFS directories and their snapshots across geographically or logically separated clusters.

### 2. Key Topics Covered
*   **Architecture & Logic**: Explains the push-based synchronization of snapshot data and the subsequent creation of identical snapshots on the remote target.
*   **User/Auth Management**: Detailed RBAC requirements for the mirror daemon (primary) and the mirror peer (secondary).
*   **Daemon Management**: Instructions for deploying `cephfs-mirror` via `cephadm` or `systemd`, including high-availability scaling (M/N policy).
*   **Mirroring Module**: Management of the Manager (`mgr`) module that handles directory-to-daemon assignments and rebalancing.
*   **Peer Bootstrapping**: A secure method for linking clusters using tokens rather than manual configuration/keyring sharing.
*   **Operational Commands**: Enabling/disabling mirroring, adding/removing peer clusters, and tracking specific directory paths.
*   **Monitoring & Health**: Methods for checking status via the CLI, admin sockets (`.asok`), and exported Prometheus/Performance metrics.
*   **Conflict Handling**: Rules regarding directory nesting (ancestor/subtree restrictions) and handling metadata mismatches.

### 3. Technical Keywords
*   **Components**: `cephfs-mirror`, `mgr mirroring module`, `MDS`, `RADOS`, `systemd`, `cephadm`.
*   **APIs/Commands**: 
    *   `ceph fs snapshot mirror enable/disable`
    *   `ceph fs snapshot mirror peer_bootstrap create/import`
    *   `ceph fs snapshot mirror add/remove <path>`
    *   `ceph fs snapshot mirror daemon status`
*   **Configuration Options**: `cephfs_mirror_max_consecutive_failures_per_directory`, `cephfs_mirror_retry_failed_directories_interval`, `cephfs_mirror_max_concurrent_directory_syncs`.
*   **States**: `idle`, `syncing`, `failed`.
*   **Metrics**: `snaps_synced`, `sync_bytes`, `mirroring_peers`, `avg_sync_time`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for disaster recovery (DR) planning and cross-site replication.
*   **Site Reliability Engineers (SREs)**: Monitoring the health and performance of data synchronization.
*   **Developers**: Working on CephFS features or integrations that rely on snapshot consistency.

### 5. Related Concepts
*   **CephFS Snapshots**: The underlying technology used to capture point-in-time states.
*   **Ceph RBAC (Capabilities)**: Required for the complex permissions used by mirror users.
*   **Ceph Manager (mgr) Modules**: The architectural framework the mirroring module resides within.
*   **Multi-site/Geo-Replication**: The broader category of data protection this feature belongs to.
*   **Snap-Schedule**: Mentioned as a conflicting feature that should not be run on the remote peer.

---

### When to update this file
This documentation should be updated if code changes occur in the following areas:
1.  **Command Syntax**: Any changes to the `fs snapshot mirror` or `fs mirror` command structures.
2.  **Logic Constraints**: If the "Single Peer" limitation or "Ancestry/Subtree" restriction is lifted or modified.
3.  **Default Values**: Changes to the default intervals or failure thresholds in the C++ configuration files.
4.  **Metric Exports**: If new performance counters are added to the mirror daemon or mirroring module.
5.  **Daemon Scaling**: Changes to the rebalancing policy (currently `M/N`) or high-availability logic.
6.  **Deprecations**: When the deprecated `peer_add` command is finally removed in favor of `peer_bootstrap`.