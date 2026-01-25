# MAN Documentation Index

## Overview
This documentation area contains the reference manuals (man pages) for the Ceph distributed storage system. It covers command-line utilities for cluster administration, daemon management (OSD, MON, MDS, RGW), storage provisioning (RBD, CephFS), and low-level troubleshooting tools for data structures and object stores.

## Files Summary
*   **ceph-bluestore-tool.rst**: Low-level administrative tool for BlueStore instances, handling repairs, migrations, and device labeling.
*   **ceph-diff-sorted.rst**: Optimized utility for comparing large, lexically sorted files with minimal memory overhead.
*   **ceph-volume.rst**: The primary tool for deploying and inspecting OSDs using LVM or simple directory scans.
*   **rbd-nbd.rst**: Client for mapping RADOS Block Device (RBD) images to Network Block Devices.
*   **rgw-policy-check.rst**: Syntax verification utility for RADOS Gateway bucket policies.
*   **ceph-fuse.rst**: FUSE-based client for mounting Ceph Filesystems in userspace.
*   **ceph-syn.rst**: Synthetic workload generator for testing CephFS performance and stability.
*   **cephfs-shell.rst**: Interactive shell for performing POSIX-like operations directly on a Ceph Filesystem.
*   **cephfs-mirror.rst**: Daemon responsible for asynchronous mirroring of CephFS snapshots between clusters.
*   **rbd.rst**: Comprehensive tool for manipulating RADOS Block Device (RBD) images, snapshots, and encryption.
*   **cephadm.rst**: Management tool for the cephadm orchestrator, handling containerized daemon lifecycles on local hosts.
*   **mount.fuse.ceph.rst**: Helper for mounting CephFS via FUSE using `/etc/fstab` entries.
*   **rgw-restore-bucket-index.rst**: Emergency utility to restore bucket indices by scanning data pools.
*   **ceph-objectstore-tool.rst**: Tool for offline modification and examination of OSD object state and metadata.
*   **ceph-immutable-object-cache.rst**: Daemon for local caching of immutable RADOS objects to improve read performance.
*   **rbd-replay-many.rst**: Utility to replay RBD workloads across multiple clients simultaneously.
*   **ceph-debugpack.rst**: Utility to collect logs, configurations, and core dumps for developer analysis.
*   **ceph-clsinfo.rst**: Displays metadata (name, version, arch) for RADOS class objects.
*   **rgw-orphan-list.rst**: Experimental tool to find RADOS objects not referenced by RGW bucket indexes.
*   **ceph-monstore-tool.rst**: Tool for offline manipulation of the Monitor's RocksDB store (monmap, osdmap, etc.).
*   **crushdiff.rst**: Test utility to predict the impact of CRUSH map changes on data movement.
*   **rbd-replay.rst**: Tool for replaying captured RBD workloads against a cluster.
*   **rbd-replay-prep.rst**: Pre-processor for raw RBD traces to prepare them for the replay tool.
*   **ceph-rbdnamer.rst**: Udev helper used to generate naming symlinks for RBD devices.
*   **rbd-mirror.rst**: Daemon for asynchronous mirroring of RBD images between pools/clusters.
*   **radosgw-admin.rst**: Primary administrative tool for RGW user, bucket, quota, and multisite management.
*   **ceph-conf.rst**: Utility for parsing and retrieving values from local Ceph configuration files.
*   **cephfs-top.rst**: Real-time metrics display utility for Ceph Filesystem clients.
*   **ceph-authtool.rst**: Keyring manipulation tool for managing CephX authentication keys and capabilities.
*   **ceph-create-keys.rst**: Utility to generate bootstrap keyrings for new monitors.
*   **ceph-osd.rst**: Manual for the Object Storage Daemon, covering initialization and local storage management.
*   **ceph-kvstore-tool.rst**: Generic tool for manipulating offline Key-Value stores (RocksDB) used by Ceph.
*   **mount.ceph.rst**: Helper for mounting the Ceph Filesystem via the Linux kernel client.
*   **osdmaptool.rst**: Utility for manipulating and simulating OSD maps and PG balancing.
*   **radosgw.rst**: Manual for the RGW RESTful gateway daemon and its web server integration.
*   **rbd-fuse.rst**: FUSE client to expose RBD images as regular files in a local directory.
*   **ceph-dencoder.rst**: Debugging utility to test encoding/decoding of Ceph data structures.
*   **ceph-mds.rst**: Manual for the Metadata Server daemon managing CephFS namespaces.
*   **rgw-gap-list.rst**: Utility to find bucket index entries with missing RADOS backing objects.
*   **ceph-volume-systemd.rst**: Helper tool for systemd to trigger OSD activation.
*   **monmaptool.rst**: Utility to create and modify monitor cluster maps (monmaps).
*   **ceph-post-file.rst**: Secure upload utility for sending diagnostic data to Ceph developers.
*   **ceph-run.rst**: Restart wrapper for daemons to handle unexpected crashes.
*   **librados-config.rst**: Displays version and configuration information for the librados library.
*   **rbdmap.rst**: Shell script for automating RBD device mapping at system boot.
*   **crushtool.rst**: Comprehensive utility for compiling, testing, and building CRUSH maps.
*   **rbd-ggate.rst**: Client for mapping RBD images using the FreeBSD GEOM Gate subsystem.
*   **ceph.rst**: The main CLI administration tool for interacting with a running Ceph cluster.
*   **ceph-mon.rst**: Manual for the Cluster Monitor daemon that maintains cluster state and quorum.

## Code Changes That Would Require Documentation Updates
*   **CLI Argument Changes**: Adding, renaming, or deprecating flags in any of the listed utilities (e.g., `ceph`, `rbd`, `radosgw-admin`).
*   **New Subcommands**: Implementation of new logic in modular tools like `ceph-volume` or `cephadm`.
*   **Data Structure Modifications**: Changes to `struct` definitions in the C++ codebase that affect `ceph-dencoder` or `ceph-objectstore-tool`.
*   **Configuration Logic**: Adding new global or daemon-specific configuration options that need to be parsed by `ceph-conf` or updated in `ceph-osd`/`ceph-mon`.
*   **Multisite/Replication Features**: Changes to the RGW multisite protocol or RBD/CephFS mirroring logic.
*   **CRUSH Algorithm Updates**: Modifications to the CRUSH mapping logic that would change `crushtool` simulation outputs.
*   **Storage Backend Changes**: Updates to BlueStore or FileStore internals (e.g., new labeling, sharding, or repair logic in `ceph-bluestore-tool`).
*   **Authentication/Capabilities**: Updates to CephX capability grammar or subsystem permissions (affects `ceph-authtool`).
*   **Kernel/FUSE Client Options**: New mount options for the kernel client (`mount.ceph`) or FUSE client (`ceph-fuse`).
*   **Orchestrator Integration**: Changes in how `cephadm` interacts with containers or systemd.

## Key Technical Concepts
*   **Storage Backends**: BlueStore, RocksDB, OMAP, WAL, DB, Filestore.
*   **Cluster Maps**: monmap, osdmap, crushmap, FSMap, service-map.
*   **Resource Specs**: image-spec, snap-spec, pool-name, namespace, pgid.
*   **Authentication**: CephX, keyrings, capabilities (caps), entity names.
*   **Deployment Styles**: cephadm, legacy, LVM, containerized.
*   **Placement Groups (PGs)**: sharding, balancing, upmap, pg_temp.
*   **RADOS Gateway**: Multi-site, realms, periods, zonegroups, zones, bucket indices.
*   **Protocols**: Messenger v1 (6789), Messenger v2 (3300), S3/Swift APIs.

## Related Components
*   **Core Daemons**: `ceph-mon`, `ceph-osd`, `ceph-mds`, `ceph-mgr`.
*   **Gateways**: `radosgw` (RGW).
*   **Libraries**: `librados`, `librbd`, `libcephfs`.
*   **Orchestration**: `cephadm`, `systemd`.
*   **Storage Drivers**: `krbd` (Kernel RBD), `nbd` (Network Block Device), `ggate` (FreeBSD GEOM).
*   **Filesystem Clients**: `ceph-fuse`, Linux Kernel Client.