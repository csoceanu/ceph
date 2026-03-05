This documentation file, `cephfs/administration.rst`, serves as the authoritative reference for the **Command Line Interface (CLI) and administrative lifecycle of the Ceph File System (CephFS)**. It defines how operators create, configure, troubleshoot, and decommission file systems and Metadata Server (MDS) daemons.

### 1. Primary Purpose
The file documents the **control plane for CephFS**. It provides the syntax and operational context for managing the relationship between the Ceph storage cluster (RADOS pools) and the CephFS abstraction (filesystems, MDS ranks, and client sessions).

### 2. Key Topics Covered
*   **Filesystem Lifecycle**: Creation (`fs new`), listing, renaming, and deletion (`fs rm`).
*   **Pool Management**: Associating metadata pools and adding/removing multiple data pools for tiered storage or file layouts.
*   **Disaster Recovery**: Swapping filesystem names/IDs (`fs swap`) to handle reconstruction, and "failing" filesystems to prevent automatic standby activation.
*   **MDS Daemon Management**: Controlling MDS ranks, handling failed daemons, and sending direct commands via `ceph tell`.
*   **Performance & Limits**: Configuring `max_file_size` and understanding its impact on MDS metadata overhead (e.g., during deletions).
*   **Operational State**: Taking a cluster down gracefully (flushing journals) versus rapid/forced shutdowns.
*   **Client Compatibility**: Managing "required client features" to ensure clients support specific protocols (e.g., encryption, async file creation) before being allowed to connect.

### 3. Technical Keywords
*   **Commands**: `ceph fs new`, `ceph fs dump`, `ceph fs set`, `ceph fs swap`, `ceph mds fail`, `ceph tell mds.*`, `ceph fs required_client_features`.
*   **Configuration/Flags**: `enable_multiple`, `max_mds`, `max_file_size`, `joinable`, `refuse_client_sessions`, `mon_fsmap_prune_threshold`.
*   **Data Structures**: `FSMap` (File System Map), `FSCID` (File System Cluster ID), `ranks`, `standby daemons`.
*   **Feature Bits**: `deleg_ino`, `alternate_name`, `reclaim_client`, `reply_encoding`.

### 4. Target Audience
*   **Storage Administrators**: For day-to-day management and scaling of CephFS.
*   **Systems Architects**: For designing multi-filesystem or disaster recovery workflows.
*   **Site Reliability Engineers (SREs)**: For troubleshooting MDS failures and performing emergency cluster shutdowns.
*   **Orchestrator Developers (e.g., Rook/K8s)**: To understand the low-level CLI calls required to automate CephFS provisioning.

### 5. Related Concepts
*   **RADOS Pools**: CephFS depends on underlying OSD pools for data and metadata.
*   **MDS (Metadata Server)**: The daemon whose state and "ranks" are governed by these commands.
*   **CephX**: Authorization and security IDs that must be updated when filesystems are renamed or swapped.
*   **MDS Journaling**: The mechanism for flushing metadata to disk during a controlled shutdown.
*   **NFS-Ganesha**: Specifically related to the `reclaim_client` feature bit for NFS failover.

---

### Triggering Updates: When to modify this file
This file must be updated if code changes occur in the following areas:
1.  **MDS Feature Bits**: If a new feature is added to the MDS/Client protocol (e.g., for new kernel versions), it must be added to the "Required Client Features" table.
2.  **CLI Syntax**: If the `ceph-mgr` or `ceph-mon` modules change the arguments for `fs` or `mds` subcommands.
3.  **Default Values**: If the default `max_file_size` or default behaviors of `max_mds` change.
4.  **Recovery Procedures**: If a new specialized recovery command (like the recent addition of `fs swap`) is implemented.