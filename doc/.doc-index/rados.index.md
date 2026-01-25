# RADOS Documentation Index

## Overview
This documentation covers the Reliable Autonomic Distributed Object Store (RADOS), the heart of the Ceph storage cluster. It provides technical guidance on cluster architecture, daemon configuration (Monitors, OSDs, Managers), data placement strategies (CRUSH, Placement Groups), and the various APIs (librados, SQLite VFS) used to interact directly with the object store.

## Files Summary
*   **rados/index.rst**: The entry point for RADOS documentation, outlining the cluster foundation and core daemon types.
*   **rados/api/librados-intro.rst**: A high-level guide to the `librados` API, including installation for C, C++, Python, Java, and PHP.
*   **rados/api/libradospp.rst**: Placeholder for the C++ specific `librados` documentation.
*   **rados/api/objclass-sdk.rst**: Describes the SDK for building Ceph Object Classes (CLS) out-of-tree.
*   **rados/api/python.rst**: Detailed reference for the `rados` Python module and its object-oriented interface.
*   **rados/api/index.rst**: Index of all available Storage Cluster APIs.
*   **rados/api/libcephsqlite.rst**: Documentation for the SQLite VFS that allows storing SQLite databases directly on RADOS.
*   **rados/api/librados.rst**: Reference for the low-level C `librados` API and asynchronous I/O operations.
*   **rados/configuration/msgr2.rst**: Explains the Messenger v2 protocol, including encryption, secure modes, and port 3300.
*   **rados/configuration/mclock-config-ref.rst**: Detailed guide on the mClock scheduler for Quality of Service (QoS) management.
*   **rados/configuration/journal-ref.rst**: Reference for legacy Filestore journaling settings (deprecated).
*   **rados/configuration/general-config-ref.rst**: Reference for general daemon settings like admin sockets and file descriptor limits.
*   **rados/configuration/mon-config-ref.rst**: Comprehensive guide on Monitor configuration, Paxos, and quorum requirements.
*   **rados/configuration/bluestore-config-ref.rst**: Technical reference for the BlueStore backend, including cache tuning and device management.
*   **rados/configuration/storage-devices.rst**: Overview of OSD backends (BlueStore vs. Filestore) and hardware roles.
*   **rados/configuration/filestore-config-ref.rst**: Reference for legacy Filestore settings (deprecated in Reef).
*   **rados/configuration/ceph-conf.rst**: Guide on the global configuration system, metavariables, and the Monitor config database.
*   **rados/configuration/network-config-ref.rst**: Guidelines for public and cluster network configuration, including IP table rules.
*   **rados/configuration/auth-config-ref.rst**: Reference for CephX authentication, keyrings, and message signing.
*   **rados/configuration/mon-lookup-dns.rst**: Instructions for using DNS SRV records for Monitor discovery.
*   **rados/configuration/osd-config-ref.rst**: Reference for OSD daemon configuration, scrubbing, and operations.
*   **rados/configuration/index.rst**: Navigation index for all RADOS configuration categories.
*   **rados/configuration/common.rst**: Common settings for nodes, heartbeats, and basic cluster identification.
*   **rados/configuration/mon-osd-interaction.rst**: Explains how OSDs and Monitors monitor each other through heartbeats and reports.
*   **rados/configuration/pool-pg-config-ref.rst**: Reference for Pool, Placement Group, and CRUSH default settings.
*   **rados/troubleshooting/troubleshooting-pg.rst**: Guide for resolving stuck PGs, unfound objects, and data inconsistencies.
*   **rados/troubleshooting/troubleshooting-mon.rst**: Steps for diagnosing Monitor quorum issues and clock skews.
*   **rados/troubleshooting/community.rst**: Information on Ceph mailing lists and community support channels.
*   **rados/troubleshooting/log-and-debug.rst**: Instructions for adjusting subsystem log levels and using log rotation.
*   **rados/troubleshooting/memory-profiling.rst**: Guide for using TCMalloc and Valgrind Massif for memory analysis.
*   **rados/troubleshooting/troubleshooting-osd.rst**: Diagnostics for slow OSDs, disk failures, and fullness issues.
*   **rados/troubleshooting/cpu-profiling.rst**: Instructions for using `oprofile` to analyze daemon CPU usage.
*   **rados/troubleshooting/index.rst**: Navigation index for RADOS troubleshooting guides.
*   **rados/man/index.rst**: Index for RADOS-related manual pages.
*   **rados/operations/erasure-code-jerasure.rst**: Details for the Jerasure erasure code plugin and techniques.
*   **rados/operations/user-management.rst**: Guide for CephX user creation, capabilities (caps), and authorization.
*   **rados/operations/balancer.rst**: Instructions for the Manager balancer module (upmap, crush-compat modes).
*   **rados/operations/pools.rst**: Detailed management guide for pools, including quotas and snapshots.
*   **rados/operations/monitoring.rst**: High-level cluster monitoring commands and health status interpretation.
*   **rados/operations/erasure-code-lrc.rst**: Documentation for the Locally Repairable Code (LRC) plugin.
*   **rados/operations/cache-tiering.rst**: Guide for cache tiering configurations (deprecated in Reef).
*   **rados/operations/erasure-code.rst**: Core concepts of erasure-coded pools, overhead, and overwrites.
*   **rados/operations/erasure-code-profile.rst**: How to create and manage erasure code profiles.
*   **rados/operations/change-mon-elections.rst**: Explains Monitor election strategies (classic, disallow, connectivity).
*   **rados/operations/devices.rst**: Management of physical devices, health monitoring, and failure prediction.
*   **rados/operations/add-or-rm-osds.rst**: Manual procedures for expanding or shrinking OSD count.
*   **rados/operations/crush-map.rst**: In-depth explanation of the CRUSH algorithm, buckets, and hierarchy.
*   **rados/operations/crush-map-edits.rst**: Guide for manual CRUSH map decompilation and editing.
*   **rados/operations/stretch-mode.rst**: Configuration for multi-site stretch clusters and tiebreaker monitors.
*   **rados/operations/erasure-code-shec.rst**: Details for the Shingled Erasure Code (SHEC) plugin.
*   **rados/operations/upmap.rst**: Technical guide for `pg-upmap` and offline OSD map optimization.
*   **rados/operations/data-placement.rst**: Overview of the relationship between Pools, PGs, and CRUSH.
*   **rados/operations/pg-states.rst**: Definitions for all possible Placement Group states (e.g., active, clean, peering).
*   **rados/operations/control.rst**: Reference for administrative control commands.
*   **rados/operations/monitoring-osd-pg.rst**: Detailed diagnostics for OSD and PG health.
*   **rados/operations/placement-groups.rst**: Detailed guide on PG autoscaling, target sizes, and bulk flags.
*   **rados/operations/index.rst**: Navigation index for cluster operations and data placement.
*   **rados/operations/pg-concepts.rst**: Glossary of core PG terms (Acting Set, Up Set, Peering).
*   **rados/operations/health-checks.rst**: Comprehensive list of health check identifiers and their meanings.
*   **rados/operations/read-balancer.rst**: Guide for the primary/read balancer introduced in Reef.
*   **rados/operations/erasure-code-clay.rst**: Documentation for the CLAY (coupled-layer) erasure code plugin.
*   **rados/operations/add-or-rm-mons.rst**: Manual procedures for adding or removing Monitors and changing IPs.
*   **rados/operations/erasure-code-isa.rst**: Reference for the ISA-L erasure code plugin.
*   **rados/operations/bluestore-migration.rst**: Strategies for migrating Filestore OSDs to BlueStore.
*   **rados/operations/operating.rst**: Basics of starting/stopping daemons using systemd.
*   **rados/operations/pgcalc/index.rst**: The underlying logic for the PG calculator tool.

## Code Changes That Would Require Documentation Updates
*   **Messenger Protocol**: Modifications to `msgr2` wire format, new encryption modes, or changing default ports (3300/6789).
*   **OSD Backends**: Removal of Filestore code, changes to BlueStore's RocksDB sharding, or introducing new object store types.
*   **mClock Scheduler**: Adjustments to QoS profiles (`balanced`, `high_client_ops`), or changing the way IOPS capacities are determined.
*   **CRUSH Algorithm**: Adding new bucket types, changing the `straw2` implementation, or adding new `step` types to CRUSH rules.
*   **Monitor Election Logic**: New election strategies beyond `connectivity` or changes to Paxos synchronization roles.
*   **PG Autoscaler**: Modifying the scaling threshold (default 3.0), changing the power-of-two rounding logic, or new pool flags (like `bulk`).
*   **Health Checks**: Adding or removing health check IDs in `mon` or `mgr`, or changing warning thresholds (e.g., `mon_data_size_warn`).
*   **API Changes**: Adding new functions to `librados.h`, changing the `rados_t` handle structure, or updating the SQLite VFS interface.
*   **Authentication**: Changes to CephX reclaim behavior, or introducing new auth profiles.
*   **Erasure Coding**: Adding new plugins, updating the CLAY/SHEC math, or changing default `k` and `m` values.
*   **Balancer**: Changes to `upmap` or `read-balancer` logic, or new Manager balancer modules.
*   **Stretch Mode**: Changing how tiebreaker monitors are handled or adding support for EC pools in stretch mode.

## Key Technical Concepts
*   **Daemons**: `ceph-mon`, `ceph-osd`, `ceph-mgr`, `ceph-mds`, `radosgw`.
*   **Storage Backend**: BlueStore, Filestore, RocksDB, WAL, DB device, `osd_memory_target`.
*   **Data Placement**: CRUSH, Placement Group (PG), Pool, `pg_num`, `pgp_num`, `upmap`.
*   **Networking**: Public Network, Cluster Network, msgr2, `mon_host`.
*   **Availability**: Quorum, Paxos, Peering, Acting Set, Up Set, Tiebreaker Monitor.
*   **Scheduling**: mClock, DMClock, IOPS, QoS Profiles.
*   **Erasure Coding**: `k` (data chunks), `m` (coding chunks), Jerasure, ISA-L, SHEC, CLAY, LRC.
*   **Security**: CephX, Keyring, Capabilities (caps), Message Signing.
*   **Monitoring**: `ceph status`, `ceph health`, Cluster Log, `df`, `osd tree`.

## Related Components
*   **RBD (Ceph Block Device)**: Relies on RADOS for image storage and snapshots.
*   **CephFS (Ceph File System)**: Uses RADOS for data and metadata (via MDS).
*   **RGW (RADOS Gateway)**: Translates S3/Swift requests into librados calls.
*   **Cephadm**: The orchestrator used to deploy and manage the RADOS daemons documented here.
*   **RocksDB**: The underlying key/value store for BlueStore and Monitor metadata.
*   **TCMalloc**: The memory allocator used by Ceph daemons (important for memory profiling).