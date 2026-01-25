# CEPHFS Documentation Index

## Overview
This documentation covers the Ceph File System (CephFS), a POSIX-compliant file system built on the RADOS object store. It provides technical details on the Metadata Server (MDS) architecture, client mounting procedures (Kernel, FUSE, and Windows), administrative operations, and advanced features like directory fragmentation, snapshots, and disaster recovery.

## Files Summary
*   **purge-queue.rst**: Explains how the MDS manages parallel file deletion and provides tunable configurations for reclaiming space.
*   **upgrading.rst**: Provides sequences for upgrading MDS clusters and migrating old directory formats (TMAPs).
*   **cache-configuration.rst**: Details MDS memory limits, cache trimming, and client capability recall mechanisms.
*   **mount-prerequisites.rst**: Lists required configuration files, keyrings, and permissions for all CephFS clients.
*   **dirfrags.rst**: Describes automatic directory splitting and merging based on size and activity thresholds.
*   **mount-using-kernel-driver.rst**: Instructions and troubleshooting for mounting via the Linux kernel driver.
*   **app-best-practices.rst**: Guidance for developers to optimize application performance on distributed file systems.
*   **recover-fs-after-mon-store-loss.rst**: Steps to rebuild the File System Map (FSMap) after catastrophic monitor failure.
*   **capabilities.rst**: Deep dive into the granular permissions (caps) granted to clients for metadata and data access.
*   **dynamic-metadata-management.rst**: Explains Dynamic Subtree Partitioning and the exporter/importer logic during migration.
*   **multimds.rst**: Configuration for active-active MDS clusters, including pinning and ephemeral partitioning policies.
*   **fs-volumes.rst**: Documentation for the `mgr/volumes` module used to manage subvolumes and groups for OpenStack and CSI.
*   **cephfs-journal-tool.rst**: Syntax and use cases for the expert tool used to inspect and repair MDS journals.
*   **troubleshooting.rst**: Guide for resolving slow requests, stuck recoveries, and client eviction issues.
*   **disaster-recovery.rst**: High-level approach to repairing metadata damage and identifying files affected by PG loss.
*   **cephfs-io-path.rst**: Visual and textual representation of how data and metadata flow between clients, MDS, and OSDs.
*   **mds-config-ref.rst**: Reference list of configuration options for the Metadata Server.
*   **ceph-dokan.rst**: Instructions for mounting CephFS on Windows using the Dokan userspace driver.
*   **eviction.rst**: Policies and commands for terminating unresponsive or misbehaving client sessions.
*   **administration.rst**: Standard management commands for creating, deleting, and modifying file systems.
*   **nfs.rst**: Exporting CephFS namespaces via NFS-Ganesha.
*   **quota.rst**: Setting and enforcing byte and file count limits on directory trees.
*   **mount-using-fuse.rst**: Instructions for using the FUSE-based userspace client.
*   **mantle.rst**: Documentation for the Lua-programmable metadata load balancer.
*   **cephfs-top.rst**: Usage of the curses-based performance monitoring utility.
*   **standby.rst**: Terminology and configuration for high-availability MDS standby and standby-replay modes.
*   **mds-states.rst**: Definitions of MDS states (active, standby, replay, etc.) and their transition logic.
*   **health-messages.rst**: Reference for `MDS_` health codes and their underlying causes.
*   **client-auth.rst**: Restricting client access via path-based capabilities, root squash, and network restrictions.
*   **lazyio.rst**: Instructions for enabling and using relaxed POSIX semantics for parallel applications.
*   **charmap.rst**: Configuration for Unicode normalization and case-insensitive directory lookups.
*   **full.rst**: System behavior and ENOSPC handling when the underlying RADOS cluster reaches capacity.
*   **index.rst**: Main landing page and table of contents.
*   **posix.rst**: Detailed list of divergences from strict POSIX semantics.
*   **createfs.rst**: Best practices for pool creation and initial file system setup.
*   **snap-schedule.rst**: Management of automated snapshot creation and retention policies.
*   **client-config-ref.rst**: Reference for client-side configuration options.
*   **snapshots.rst**: Basic mechanics of the `.snap` directory for snapshot management.
*   **scrub.rst**: Commands for forward and backward consistency checking of the file system.
*   **mds-journaling.rst**: Internal logic of how metadata events are streamed to RADOS.
*   **file-layouts.rst**: Controlling how files are striped across OSD objects and pools.
*   **kernel-features.rst**: Comparison of feature support between the Kernel and FUSE drivers.
*   **journaler.rst**: Reference for internal journaling configuration.
*   **metrics.rst**: Definitions of performance counters for clients and subvolumes.
*   **disaster-recovery-experts.rst**: Low-level repair procedures using `cephfs-data-scan` and `cephfs-table-tool`.
*   **experimental-features.rst**: Status and risks of unstable features like Inline Data.
*   **fscrypt.rst**: Client-side encryption for directories using the fscrypt kernel/userspace framework.
*   **cephfs-mirroring.rst**: Asynchronous push-based replication of snapshots to remote clusters.
*   **multifs.rst**: Supporting multiple independent CephFS instances on a single cluster.
*   **mdcache.rst**: Theory of operation for the distributed metadata cache.
*   **add-remove-mds.rst**: Manual deployment guide for MDS daemons.
*   **api/libcephfs-java.rst**: Status and javadoc links for Java bindings.
*   **api/libcephfs-py.rst**: API reference for the Python `cephfs` module.

## Code Changes That Would Require Documentation Updates
*   **MDS State Machine**: Any change to `MDSMap.h` or the transition logic in `MDS.cc` (Ref: `mds-states.rst`).
*   **Config Options**: Adding or changing defaults for options in `common/config_opts.h` starting with `mds_` or `client_` (Ref: `mds-config-ref.rst`, `client-config-ref.rst`).
*   **New CLI Commands**: Changes to `MDSMonitor.cc` or `Mgr` modules that add subcommands for `fs`, `mds`, or `volume` (Ref: `administration.rst`, `fs-volumes.rst`).
*   **Capability Logic**: Changes to how caps are issued, revoked, or the bitmask definitions in `include/ceph_fs.h` (Ref: `capabilities.rst`).
*   **Journal Formats**: Modifying `Journaler.cc` or adding new `LogEvent` types (Ref: `mds-journaling.rst`, `cephfs-journal-tool.rst`).
*   **Feature Support**: Adding support for new features in the kernel client vs FUSE (Ref: `kernel-features.rst`).
*   **Health Checks**: Adding new `MDS_HEALTH_` codes in the MDS or Monitor (Ref: `health-messages.rst`).
*   **Scaling/Balancing**: Changes to the balancer logic, `Mantle` Lua integration, or directory fragmentation algorithms (Ref: `mantle.rst`, `dirfrags.rst`, `dynamic-metadata-management.rst`).
*   **Protocol Versions**: Updating `required_client_features` or `reply_encoding` (Ref: `administration.rst`).

## Key Technical Concepts
*   **MDS Ranks**: The sharding unit of metadata.
*   **FSCID**: File System Cluster ID.
*   **Capabilities (Caps)**: `PIN`, `AUTH`, `LINK`, `XATTR`, `FILE` (shared/exclusive).
*   **Subtree Partitioning**: Exporting/Importing metadata between ranks.
*   **Ephemeral Pinning**: Distributed vs Random automatic partitioning.
*   **Path Restriction**: Using `allow rw path=/dir` in CephX.
*   **Purge Queue**: Background deletion of RADOS objects.
*   **Scrub**: Forward/Backward consistency checks.
*   **Virtual Xattrs**: `ceph.dir.layout`, `ceph.quota.max_bytes`, `ceph.dir.pin`.
*   **Standby-Replay**: Hot-standby MDS following a journal.
*   **LazyIO**: Non-POSIX shared-write consistency.

## Related Components
*   **MDS (Metadata Server)**: The primary daemon for CephFS.
*   **RADOS**: The underlying object store.
*   **OSD**: Where file data and metadata objects reside.
*   **MON (Monitor)**: Manages the FSMap and MDS cluster state.
*   **MGR (Manager)**: Hosts the `volumes`, `mirroring`, `stats`, and `snap_schedule` modules.
*   **Libcephfs**: The userspace library used by FUSE, Python, and Java bindings.
*   **Kernel Client**: The `ceph.ko` Linux module.
*   **NFS-Ganesha**: The gateway for NFS access.