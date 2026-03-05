This documentation file, `cephfs/fs-volumes.rst`, is the definitive guide for managing **CephFS Volumes, Subvolumes, and Subvolume Groups** via the Ceph Manager (`ceph-mgr`) `volumes` module. It serves as the bridge between Ceph’s internal storage logic and external orchestrators like OpenStack Manila or the Ceph CSI (Container Storage Interface).

### 1. Primary Purpose
The file documents the `volumes` module of the Ceph Manager daemon. Its primary role is to provide a "single source of truth" and a standardized CLI for creating and managing CephFS exports. It abstracts complex file system operations into manageable units (Volumes > Groups > Subvolumes) suitable for multi-tenant environments and containerized workloads.

### 2. Key Topics Covered
*   **Volume Management**: Lifecycle operations for entire CephFS file systems (create, rm, ls, rename, info).
*   **Subvolume Groups**: Managing directory-level abstractions used for organizational grouping and policy enforcement.
*   **Subvolumes**: Creating independent directory trees, which are the primary units used for Manila shares or CSI volumes.
*   **Snapshots & Cloning**: Procedures for asynchronous data copying from snapshots to new subvolumes, including status tracking and cancellation.
*   **Security & Access**: Authorizing and evicting CephX IDs for specific subvolumes.
*   **Advanced Configuration**:
    *   **Quotas**: Resizing volumes and subvolumes.
    *   **Pinning**: Distributing load across MDS ranks using export/distributed/random policies.
    *   **Features**: Unicode normalization, case sensitivity, and "earmarking" (tagging subvolumes for NFS/SMB).
    *   **Snapshot Visibility**: Hiding `.snap` directories from specific clients.
*   **Subvolume Quiesce**: A sophisticated mechanism for pausing I/O across sets of subvolumes to ensure application-level consistency during backups.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph fs volume`, `ceph fs subvolumegroup`, `ceph fs subvolume`, `ceph fs clone`, `ceph fs quiesce`.
*   **Configuration Options**: `mgr/volumes/max_concurrent_clones`, `mgr/volumes/pause_purging`, `client_respect_subvolume_snapshot_visibility`.
*   **Technical Terms**: CephX, MDS (Metadata Server), RADOS namespaces, Quotas, Charmap (normalization/casesensitive), Earmarks, Quiesce Set, Ephemeral Pinning.
*   **States**: `pending`, `in-progress`, `complete`, `failed`, `canceled` (for clones); `QUIESCING`, `QUIESCED`, `TIMEDOUT`, `EXPIRED` (for quiesce sets).

### 4. Target Audience
*   **Storage Administrators**: Managing CephFS clusters and resource allocation.
*   **Cloud Architects**: Integrating Ceph with OpenStack (Manila).
*   **Kubernetes Operators**: Using Ceph CSI to provide persistent storage for containers.
*   **Developers**: Building automation tools that interact with the `ceph-mgr` volumes interface.

### 5. Related Concepts
*   **Ceph Manager Orchestrator**: For MDS deployment and pool management during volume creation.
*   **CephFS Quotas**: The underlying mechanism for subvolume "sizing."
*   **CephFS Snapshots**: The technical foundation for the cloning and quiesce features.
*   **MDS Internals**: Specifically regarding pinning and the `quiesce db` service.
*   **External Systems**: OpenStack Manila and Ceph CSI.

### Maintenance Guide: When to Update this File
This file should be updated if any of the following code changes occur in the Ceph repository:
*   **CLI Changes**: Any modification to the `volumes` module subcommands in `ceph-mgr`.
*   **New Features**: Addition of new subvolume properties (e.g., new earmarks, new normalization forms, or additional metadata fields).
*   **Internal State Machine Changes**: Changes to how asynchronous cloning or quiescing transitions between states.
*   **Compatibility Notes**: When a feature (like group snapshots) is deprecated or when a feature requires a specific minimum Ceph release (e.g., Squid for Quiesce).
*   **Configurables**: Introduction of new `mgr/volumes/` configuration flags.
*   **Integration Shifts**: Changes in how the module interacts with the Orchestrator (Rook/cephadmn) or MDS daemons.