This documentation outlines the architecture, configuration, and management of **CephFS Mirroring**, a feature introduced in the Ceph "Pacific" release for asynchronous, snapshot-based replication between CephFS clusters.

### 1. Primary Purpose
The file documents the **`cephfs-mirror`** tool and the associated **Manager mirroring module**. It provides a guide for setting up push-based replication of specific directory snapshots from a primary (local) CephFS file system to a secondary (remote) CephFS file system.

### 2. Key Topics Covered
*   **Core Mechanism**: Utilization of the `CephFS Snapdiff Feature` to identify changes between snapshots and synchronize data via bulk copying.
*   **Security & RBAC**: Specific capabilities (caps) required for local mirror users and remote peer users.
*   **Daemon Management**: Instructions for running `cephfs-mirror` via `systemctl` or as a foreground process.
*   **Configuration Workflow**: Enabling the manager module, adding/bootstrapping peers, and marking specific directories for mirroring.
*   **Lifecycle Management**: Handling snapshot "incarnations" (recreated snapshots with the same name) and snapshot synchronization ordering.
*   **Monitoring & Status**: Using the Ceph CLI and admin sockets (`asok`) to track synchronization states (`idle`, `syncing`, `failed`).
*   **High Availability**: How the Manager module balances directory assignments across multiple mirror daemons.

### 3. Technical Keywords
*   **Daemons/Services**: `cephfs-mirror`, `ceph-mgr` (mirroring plugin), `MDS`, `OSD`.
*   **Commands**:
    *   `ceph mgr module enable mirroring`
    *   `ceph fs snapshot mirror enable/disable`
    *   `ceph fs snapshot mirror peer_bootstrap create/import`
    *   `ceph fs snapshot mirror add/remove <path>`
*   **APIs/Structures**: `SnapInfo` (MDS metadata), `Snapdiff`, `RADOS instance-id`.
*   **Config Options**: 
    *   `cephfs_mirror_max_consecutive_failures_per_directory`
    *   `cephfs_mirror_retry_failed_directories_interval`
*   **States**: `idle`, `syncing`, `failed`, `stalled`, `mapped`.

### 4. Target Audience
*   **Storage Administrators**: Tasked with disaster recovery (DR) setup and multi-site replication.
*   **System Operators**: Responsible for deploying and monitoring Ceph services.
*   **Developers**: Working on CephFS features or integration tools that require understanding of snapshot metadata.

### 5. Related Concepts
*   **CephFS Snapshots**: The underlying mechanism that identifies point-in-time file system states.
*   **Cephadm**: The future/intended orchestrator for deploying mirror daemons.
*   **RBD Mirroring**: A parallel concept for block devices (though CephFS mirroring is directory-based).
*   **Disaster Recovery (DR)**: The primary use case for maintaining a synchronized remote copy of data.

---

### Update Triggers for AI Systems
This documentation should be updated if code changes occur in the following areas:
1.  **CLI Syntax Changes**: Any modifications to the `ceph fs snapshot mirror` command subtree.
2.  **Logic in Snapdiff**: If the method for identifying differences between snapshots changes (e.g., moving away from bulk copying).
3.  **Deployment Tooling**: When `cephadm` support for `cephfs-mirror` is fully implemented (as noted in the "Mirroring Module and Interface" section).
4.  **Metadata Schema**: Changes to how `SnapInfo` stores `snap-id` or synchronization metadata on the MDS.
5.  **Scaling Constraints**: If the "untested" status of multiple mirror daemons changes or if the "single peer" limitation is lifted.
6.  **Feature Support**: If support for **hardlinks** is added (currently listed as unsupported).