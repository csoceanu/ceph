# RADOS Documentation Index

## Overview
RADOS (Reliable Autonomic Distributed Object Store) is the core storage engine of the Ceph cluster, managing data through OSD, Monitor, and Manager daemons. This documentation provides a comprehensive guide to cluster operations, data placement logic (CRUSH, PGs), mClock-based QoS, and the `librados` API for multi-language application integration, alongside deep-level diagnostics for troubleshooting cluster health and performance.

## Files Summary
*   **rados/index.rst**: The high-level entry point for Ceph Storage Cluster documentation, outlining basic daemon types and deployment categories.
*   **rados/api/index.rst**: The master table of contents for Ceph Storage Cluster APIs.
*   **rados/api/libcephsqlite.rst**: Details the SQLite VFS implementation for RADOS, enabling decentralized database storage with guidance on locking and recovery.
*   **rados/api/librados-intro.rst**: Introduction to the RADOS API for C, C++, Python, Java, and PHP, covering cluster handles and I/O contexts.
*   **rados/api/librados.rst**: Detailed C API reference for `librados`, including synchronous and asynchronous I/O patterns.
*   **rados/api/libradospp.rst**: Placeholder/Note regarding the C++ API stability and compilation requirements.
*   **rados/api/objclass-sdk.rst**: Documentation for the SDK used to build independent Ceph Object Classes (CLS) outside the main Ceph source tree.
*   **rados/api/python.rst**: A comprehensive guide and API reference for the `rados` Python module.
*   **rados/configuration/auth-config-ref.rst**: Technical reference for CephX authentication, signatures, and daemon-specific keyring requirements.
*   **rados/configuration/bluestore-config-ref.rst**: Extensive reference for BlueStore settings, covering cache, checksums, compression, and hardware offloading.
*   **rados/configuration/ceph-conf.rst**: Detailed syntax of `ceph.conf`, metavariable expansion, and the Monitor central configuration database.
*   **rados/configuration/common.rst**: Overview of shared configuration settings including host identification and networking.
*   **rados/configuration/filestore-config-ref.rst**: Legacy reference for the deprecated Filestore backend.
*   **rados/configuration/general-config-ref.rst**: Global settings for daemon process management like admin sockets and file descriptor limits.
*   **rados/configuration/index.rst**: Main entry point and index for the RADOS configuration documentation.
*   **rados/configuration/journal-ref.rst**: Reference for Filestore journaling (deprecated).
*   **rados/configuration/mclock-config-ref.rst**: Deep dive into the mClock scheduler, QoS profiles, and OSD benchmarking.
*   **rados/configuration/mon-config-ref.rst**: Architecture of the Monitor subsystem, Paxos consistency, and quorum requirements.
*   **rados/configuration/mon-lookup-dns.rst**: Instructions for using DNS SRV records for monitor discovery.
*   **rados/configuration/mon-osd-interaction.rst**: Configures heartbeats, grace periods, and reporting intervals between OSDs and Monitors.
*   **rados/configuration/msgr2.rst**: Details the Messenger v2 protocol, secure connection modes, and port 3300 binding.
*   **rados/configuration/network-config-ref.rst**: Networking guidelines for Public vs. Cluster networks.
*   **rados/configuration/osd-config-ref.rst**: Reference for OSD daemon settings, including scrubbing and mClock-based QoS.
*   **rados/configuration/pool-pg-config-ref.rst**: Configuration for pool defaults, PG counts, and CRUSH rule assignments.
*   **rados/configuration/storage-devices.rst**: High-level comparison of OSD storage backends (BlueStore vs. FileStore).
*   **rados/man/index.rst**: A directory of manual pages for object store command-line tools (e.g., `ceph-osd`, `rados`).
*   **rados/operations/index.rst**: A categorical table of contents for cluster operations, data placement, and troubleshooting.
*   **rados/operations/add-or-rm-mons.rst**: Procedures for managing the Monitor quorum and handling unhealthy clusters.
*   **rados/operations/add-or-rm-osds.rst**: Procedures for the lifecycle of OSD hardware, including manual addition and removal.
*   **rados/operations/balancer.rst**: Describes the `ceph-mgr` balancer module and automated optimization settings.
*   **rados/operations/bluestore-migration.rst**: Guide for migrating OSDs from Filestore to BlueStore.
*   **rados/operations/cache-tiering.rst**: (Deprecated) Documentation for configuring and removing cache tiers.
*   **rados/operations/change-mon-elections.rst**: Documentation on Monitor election strategies (Classic, Disallow, Connectivity).
*   **rados/operations/control.rst**: Reference for administrative control commands across Monitor, Auth, PG, and OSD subsystems.
*   **rados/operations/crush-map.rst**: Core documentation for the CRUSH algorithm, bucket types, and placement rules.
*   **rados/operations/crush-map-edits.rst**: Technical guide for manually decompiling and editing CRUSH maps.
*   **rados/operations/data-placement.rst**: Overview of how Pools, PGs, and CRUSH work together.
*   **rados/operations/devices.rst**: Management of physical storage devices, including SMART monitoring and failure prediction.
*   **rados/operations/erasure-code.rst**: General concepts of Erasure Coding (EC), overwrites, and overhead.
*   **rados/operations/erasure-code-clay.rst**: Configuration for the Coupled-Layer (CLAY) EC plugin.
*   **rados/operations/erasure-code-isa.rst**: Configuration for the Intel ISA-L EC plugin.
*   **rados/operations/erasure-code-jerasure.rst**: Configuration for the Jerasure EC plugin.
*   **rados/operations/erasure-code-lrc.rst**: Configuration for the Locally Repairable (LRC) EC plugin.
*   **rados/operations/erasure-code-profile.rst**: Documentation for managing Erasure Code profiles and parameters.
*   **rados/operations/erasure-code-shec.rst**: Configuration for the Shingled EC (SHEC) plugin.
*   **rados/operations/health-checks.rst**: Reference for cluster health check identifiers and remediation steps.
*   **rados/operations/monitoring.rst**: Guide to cluster status monitoring, health muting, and Data Availability Scores.
*   **rados/operations/monitoring-osd-pg.rst**: High-level concepts for monitoring OSD status and PG peering.
*   **rados/operations/operating.rst**: Instructions for managing Ceph daemons using `systemd`.
*   **rados/operations/pg-concepts.rst**: Glossary of fundamental PG terms like Peering, Acting Set, and Epochs.
*   **rados/operations/pg-states.rst**: A glossary of all possible PG states.
*   **rados/operations/pgcalc/index.rst**: Utility for calculating optimal Placement Group counts.
*   **rados/operations/placement-groups.rst**: Deep dive into PG mechanics, Autoscaler, and recovery.
*   **rados/operations/pools.rst**: Management guide for logical pools, including quotas and snapshots.
*   **rados/operations/read-balancer.rst**: Explains the `pg-upmap-primary` mechanism for balancing primary PG distribution.
*   **rados/operations/stretch-mode.rst**: Configuration for clusters spanning multiple data centers.
*   **rados/operations/upmap.rst**: Instructions for using `pg-upmap` to optimize PG distribution.
*   **rados/operations/user-management.rst**: Detailed guide on CephX users, capabilities, and keyrings.
*   **rados/troubleshooting/index.rst**: Master index for all RADOS-related troubleshooting and profiling.
*   **rados/troubleshooting/community.rst**: Directory of mailing lists and community support channels.
*   **rados/troubleshooting/cpu-profiling.rst**: Steps for using `oprofile` to analyze Ceph CPU usage.
*   **rados/troubleshooting/log-and-debug.rst**: Instructions for adjusting subsystem logging levels.
*   **rados/troubleshooting/memory-profiling.rst**: Procedures for heap profiling using TCMalloc and Valgrind.
*   **rados/troubleshooting/troubleshooting-mon.rst**: Procedures for recovering Monitor quorum and fixing broken monmaps.
*   **rados/troubleshooting/troubleshooting-osd.rst**: Guide for diagnosing OSD failures, bottlenecks, and latency.
*   **rados/troubleshooting/troubleshooting-pg.rst**: Logic for resolving stuck PGs and repairing data inconsistencies.

## Code Changes That Would Require Documentation Updates
*   **CLI Command Syntax**: Adding, renaming, or changing arguments for `ceph osd`, `mon`, `pg`, `auth`, or `device` commands.
*   **Health Check Logic**: Adding new `health_check_t` identifiers or changing aggregation logic in `ceph health detail`.
*   **Configuration Defaults**: Updates to core thresholds (e.g., `pg_autoscale_mode`, `mon_clock_drift_allowed`, `osd_pool_default_size`, or BlueStore cache settings).
*   **Erasure Code Plugins**: Implementing new EC plugins or modifying existing plugin parameters.
*   **CRUSH & Balancer**: Modifications to `CrushWrapper`, new bucket types, or changes to the `balancer` module algorithms.
*   **API Signatures**: Any change to `librados.h`, the Python `rados` wrapper, `libcephsqlite`, or the Object Class (CLS) SDK.
*   **PG State Machine**: Adding or renaming states in the PG state machine for peering, recovery, or backfill.
*   **Monitor/Paxos Logic**: Changes to election strategies, quorum requirements, or the Paxos implementation.
*   **Backend & Protocol**: Modifications to BlueStore data structures, Messenger (msgr2) revisions, or deprecation of legacy logic (Filestore/Cache-tiering).
*   **mClock Scheduler**: Tuning the hard-coded ratios for QoS profiles or changing OSD shard handling.
*   **Internal Tooling**: Updates to `ceph-objectstore-tool`, `ceph-monstore-tool`, or admin socket commands.

## Key Technical Concepts
*   **Daemons & Consistency**: OSD, Monitor (Paxos), Manager (MGR), Quorum, Election Strategies, Epochs, Monmap/OSDMap.
*   **Data Placement**: CRUSH Buckets (Root, Rack, Host), Straw2, Placement Groups (PGs), Acting/Up Sets, `pg-upmap`, Balancer.
*   **Pool Types**: Replicated vs. Erasure Coded (Jerasure, ISA, LRC, SHEC, CLAY), Profiles.
*   **Storage Backends**: BlueStore (Block, DB, WAL), Filestore (legacy), RocksDB, Checksums, Compression.
*   **Authentication & Security**: CephX, Capabilities (Caps), Keyrings, Message Signatures, Blocklisting.
*   **Resource Scheduling**: mClock QoS, Profiles (balanced, high_recovery, etc.), Reservations, Weights.
*   **Operations & Health**: Scrubbing (Deep/Shallow), Peering, Backfilling, Recovery, SMART Monitoring, Fullness Ratios.
*   **Networking**: Messenger v2 (msgr2), Public vs. Cluster networks, Heartbeats, Port 3300/6789.
*   **APIs & SDKs**: `librados` (C/C++), Python `rados`, SQLite VFS, Object Classes (CLS), IoCtx, AIO.
*   **Diagnostics**: Clock Skew, Stuck PGs, Heap Profiling (TCMalloc), CPU Profiling, Log Subsystems.

## Related Components
*   **Cephadm / Rook**: Orchestrators for automated deployment and daemon management.
*   **Storage Interfaces**: RGW (Object), RBD (Block), and CephFS (File) which consume RADOS pools.
*   **BlueStore & RocksDB**: The internal storage engine and metadata database.
*   **ceph-volume**: The tool for provisioning storage devices for OSDs.
*   **Systemd**: The service manager for daemon operations.
*   **mClock**: The I/O scheduler for resource prioritization.
*   **Librados**: The foundational client-side library for all Ceph services.