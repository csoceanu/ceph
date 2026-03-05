This documentation file, `rbd-snapshot.rst`, serves as the definitive guide for managing **Ceph Block Device (RBD) snapshots and clones**. It defines the lifecycle of point-in-time image copies and the hierarchical relationship between parent snapshots and child clones.

### 1. Primary Purpose
The file documents the conceptual framework and operational commands for **RBD snapshots** (read-only checkpoints) and **RBD layering** (copy-on-write clones). It ensures users understand how to preserve state, recover data, and efficiently distribute VM templates within a Ceph cluster.

### 2. Key Topics Covered
*   **Snapshot Fundamentals**: Definitions of snapshots as read-only, crash-consistent logical copies.
*   **Data Integrity**: Warnings regarding I/O quiescing, the use of `fsfreeze`, and `qemu-guest-agent` for filesystem consistency.
*   **Authentication**: Instructions for using `cephx` credentials with RBD commands.
*   **Basic Operations**: Procedures for creating, listing, rolling back, deleting, and purging snapshots.
*   **Layering & Cloning**: The workflow for creating writable "Child" images from "Parent" snapshots using Copy-on-Write (COW) technology.
*   **Clone Management**: Detailed steps for protecting/unprotecting snapshots and "flattening" clones (removing the parent dependency).

### 3. Technical Keywords
*   **Core Commands**: `rbd snap create`, `rbd snap ls`, `rbd snap rollback`, `rbd snap rm`, `rbd snap purge`, `rbd snap protect`, `rbd snap unprotect`, `rbd clone`, `rbd children`, `rbd flatten`.
*   **Configuration & Auth**: `cephx`, `--id`, `--keyring`, `CEPH_ARGS`.
*   **Technical Concepts**: `crash-consistent`, `quiesce`, `snaptrim` (asynchronous deletion), `COW (Copy-on-Write)`, `RBD format 2`.
*   **External Tools**: `fsfreeze`, `qemu-guest-agent`, `libvirt`, `OpenStack`.

### 4. Target Audience
*   **Storage Administrators**: Managing Ceph cluster capacity and data retention.
*   **Cloud Architects**: Designing VM templating strategies for OpenStack or CloudStack.
*   **DevOps/System Engineers**: Automating backup and recovery workflows for block storage.

### 5. Related Concepts
*   **Cephx Authentication**: Required for executing the documented commands.
*   **RBD Mirroring**: Snapshots are often the basis for asynchronous replication.
*   **Pool Management**: Snapshots and clones exist within or across Ceph pools.
*   **QEMU/KVM Integration**: The primary use case for high-performance cloning in virtualization.

---

### Maintenance Note: When to Update this File
This documentation should be updated if code changes occur in the following areas:
1.  **CLI Syntax**: Changes to the `rbd` CLI arguments or the introduction of new `snap` subcommands.
2.  **Default Behavior**: Changes to the default RBD format (currently format 2) or default `cephx` requirements.
3.  **Performance/Logic**: Changes to the "snaptrim" process or the underlying Copy-on-Write logic.
4.  **Compatibility**: Updates to supported Linux kernel versions (e.g., changes beyond the mentioned 3.10 release) or supported hypervisor versions.
5.  **New Features**: Introduction of "Group Snapshots" or scheduled snapshot mirroring features.