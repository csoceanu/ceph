# RADOS Documentation Index

## Overview
This documentation covers the Reliable Autonomic Distributed Object Store (RADOS), the foundation of the Ceph Storage Cluster. It provides a comprehensive guide to managing core daemons (MON, OSD, MGR), hardware lifecycle, data placement strategies (CRUSH, PGs), storage backends (BlueStore), developer APIs (librados, Python, SDKs), and advanced operations including troubleshooting and failure recovery.

## Files Summary
*   **rados/index.rst**: High-level entry point and foundation for RADOS architecture and deployment.
*   **rados/operating.rst**: Instructions for managing Ceph daemons using `systemd` and legacy `SysVinit`.
*   **rados/monitoring-osd-pg.rst**: Guide to monitoring OSD and Placement Group (PG) states and status.
*   **rados/health-checks.rst**: Comprehensive reference for cluster health codes, their meanings, and resolution steps.
*   **rados/devices.rst**: Management of physical hardware, LED blinking, and failure prediction (SMART metrics).
*   **rados/pools.rst**: Management of logical partitions, including quotas, snapshots, and pool-level configurations.
*   **rados/placement-groups.rst**: Detailed explanation of PGs, including the PG Autoscaler, sizing, and durability.
*   **rados/pg-states.rst**: A glossary and reference for the various states a PG can occupy (e.g., peering, degraded).
*   **rados/data-placement.rst**: Conceptual overview of how data is mapped from objects to OSDs.
*   **rados/crush-map.rst**: Detailed guide to the CRUSH algorithm, bucket types, device classes, and location hooks.
*   **rados/crush-map-edits.rst**: Procedures for manual CRUSH map manipulation (decompiling/recompiling).
*   **rados/upmap.rst**: Usage of `pg-upmap` exceptions for manual or automated data distribution fine-tuning.
*   **rados/read-balancer.rst**: Instructions for the primary PG balancer to optimize read performance.
*   **rados/erasure-code.rst**: Core concepts of Erasure Coding (EC), profiles, and overhead.
*   **rados/erasure-code-jerasure.rst**: Configuration for the Jerasure EC plugin.
*   **rados/erasure-code-isa.rst**: Configuration for the ISA-L EC plugin.
*   **rados/erasure-code-lrc.rst**: Configuration for the Locally Repairable (LRC) EC plugin.
*   **rados/erasure-code-shec.rst**: Configuration for the Shingled EC (SHEC) plugin.
*   **rados/erasure-code-clay.rst**: Configuration for the CLAY (coupled-layer) EC plugin.
*   **rados/bluestore-migration.rst**: Strategies for migrating OSDs from legacy Filestore to BlueStore.
*   **rados/cache-tiering.rst**: Documentation for the (now deprecated) cache tiering feature.
*   **rados/stretch-mode.rst**: Setup for multi-site clusters with tiebreaker monitors and site-aware peering.
*   **rados/change-mon-elections.rst**: Configuration for Monitor election strategies (Classic, Disallow, Connectivity).
*   **rados/man/index.rst**: Navigation index for RADOS-related manual pages.
*   **rados/operations/erasure-code-profile.rst**: Instructions for creating, removing, and listing erasure code profiles and their associated plugins.
*   **rados/operations/control.rst**: A reference for administrative CLI commands used to manage and query monitors, OSDs, PGs, and authentication.
*   **rados/operations/user-management.rst**: Detailed guide on Cephx user creation, capability (caps) syntax, and keyring management.
*   **rados/operations/erasure-code-isa.rst**: Configuration parameters for the Intel Storage Acceleration (ISA) library erasure coding plugin.
*   **rados/operations/add-or-rm-mons.rst**: Procedures for monitor lifecycle management, quorum logic, and handling monitor IP address changes.
*   **rados/operations/monitoring.rst**: Guide to checking cluster health, interpreting `ceph status`, and using the admin socket for real-time daemon queries.
*   **rados/operations/balancer.rst**: Instructions for the OSD balancer module, including `upmap`, `read`, and `crush-compat` modes.
*   **rados/operations/pg-concepts.rst**: A technical glossary defining the states and lifecycle stages of Placement Groups.
*   **rados/operations/pgcalc/index.rst**: Documentation of the logic and interface for the interactive PG count calculator.
*   **rados/configuration/common.rst**: Essential settings for initial cluster setup, including monitor hosts and metavariables.
*   **rados/configuration/bluestore-config-ref.rst**: In-depth configuration for BlueStore, covering cache autotuning, compression, RocksDB sharding, and SPDK.
*   **rados/configuration/osd-config-ref.rst**: Comprehensive OSD settings including scrubbing intervals, mClock QoS scheduling, and backfilling limits.
*   **rados/configuration/filestore-config-ref.rst**: Reference for legacy Filestore-specific settings and its deprecation status.
*   **rados/configuration/pool-pg-config-ref.rst**: Configuration for pool defaults, PG counts, and CRUSH placement rules.
*   **rados/configuration/msgr2.rst**: Details on the Messenger v2 protocol, including encryption, compression, and port 3300.
*   **rados/configuration/auth-config-ref.rst**: Advanced Cephx authentication settings, message signing, and ticket time-to-live.
*   **rados/configuration/general-config-ref.rst**: Miscellaneous settings for admin sockets, PID files, and file descriptor limits.
*   **rados/configuration/index.rst**: Main entry point for RADOS configuration documentation.
*   **rados/configuration/mon-config-ref.rst**: Deep dive into Monitor internals, Paxos consensus, storage ratios, and clock drift.
*   **rados/configuration/network-config-ref.rst**: Architecture of Public and Cluster networks, firewalls, and bind settings.
*   **rados/configuration/journal-ref.rst**: Reference for the OSD journal used in legacy Filestore deployments.
*   **rados/configuration/mon-osd-interaction.rst**: Tuning the heartbeat mechanisms and failure reporting logic.
*   **rados/configuration/ceph-conf.rst**: Guide to the `ceph.conf` syntax, source precedence, and metavariables.
*   **rados/configuration/mon-lookup-dns.rst**: Instructions for configuring Monitor discovery using DNS SRV records.
*   **rados/configuration/storage-devices.rst**: High-level comparison and selection guide between BlueStore and Filestore backends.
*   **rados/configuration/mclock-config-ref.rst**: Extensive guide to mClock scheduler profiles, IOPS capacity benchmarks, and QoS tuning.
*   **rados/api/index.rst**: Landing page for Ceph APIs, including `librados` and Object Classes.
*   **rados/api/librados-intro.rst**: A beginner's guide to the `librados` API across multiple languages.
*   **rados/api/librados.rst**: Reference for the C language bindings of `librados`.
*   **rados/api/libradospp.rst**: Introduction for the C++ language bindings of `librados`.
*   **rados/api/python.rst**: API reference for the `rados` Python module, covering cluster handles and IO contexts.
*   **rados/api/objclass-sdk.rst**: Documentation for building Ceph Object Classes outside of the main source tree.
*   **rados/api/libcephsqlite.rst**: Documentation for the SQLite VFS implementation on top of RADOS.
*   **rados/troubleshooting/index.rst**: Entry point for logs, daemon issues, and profiling.
*   **rados/troubleshooting/log-and-debug.rst**: Adjusting debug levels and managing log rotation.
*   **rados/troubleshooting/troubleshooting-mon.rst**: Diagnosing quorum issues, clock skews, and store recovery.
*   **rados/troubleshooting/troubleshooting-osd.rst**: Covers OSD startup failures, "full" ratios, slow requests, and performance.
*   **rados/troubleshooting/troubleshooting-pg.rst**: Addressing stuck PGs, unfound objects, and manual repair.
*   **rados/troubleshooting/memory-profiling.rst**: Tracking leaks using TCMalloc and Valgrind.
*   **rados/troubleshooting/cpu-profiling.rst**: Analyzing CPU utilization and call graphs using `oprofile`.
*   **rados/troubleshooting/community.rst**: Directory of mailing lists and community resources.

## Code Changes That Would Require Documentation Updates
*   **CLI Command Syntax:** Adding or modifying subcommands for `ceph osd`, `ceph mon`, `ceph pg`, `ceph auth`, or `ceph device`.
*   **Health Check Identifiers:** Adding new health warnings or errors to `MonMonitor.cc` or `Mgr.cc`.
*   **CRUSH Algorithm Modifications:** Changes to `straw2`, bucket types, or `device-class` handling in `CrushWrapper.cc`.
*   **Configuration Defaults:** Changing default values for timing, intervals, mClock tunables, or fullness ratios (e.g., `mon_osd_full_ratio`).
*   **PG Logic:** Changes to the PG Autoscaler (power-of-two logic, `bulk` flag) or the PG State Machine (transitions in `PG.h`).
*   **Storage Backends:** Modifications to BlueStore (block devices, WAL/DB partitioning, caching) or removal of Filestore.
*   **Monitor Internals:** Changes to Monitor election strategies, Paxos consensus, or heartbeat/failure detection logic.
*   **Erasure Code Plugins:** Adding plugins or changing parameters (k, m, d, c).
*   **Messenger Protocol:** Introduction of new encryption algorithms or connection modes in msgr2.
*   **APIs & SDKs:** Extensions to `librados` (C, C++, Python), breaking changes to the Object Class SDK (`objclass.h`), or SQLite VFS locking.
*   **Tooling Output:** Changes to the output format of `ceph-objectstore-tool`, `crushtool`, or admin socket (`asok`) commands.

## Key Technical Concepts
*   **Core Daemons:** OSD (Object Storage), MON (Monitor), MGR (Manager), MDS (Metadata Server).
*   **Data Structures:** OSDMap, CRUSH Map, PG Map, MonMap, fsid, epoch.
*   **Placement Logic:** CRUSH algorithm, Up Sets vs. Acting Sets, Peering, Backfill, Recovery.
*   **Storage Backends:** BlueStore (RocksDB, WAL, DB device), Filestore (deprecated).
*   **Erasure Coding (EC):** Data Chunks (K), Coding Chunks (M), Local Parity (L), SHEC, CLAY.
*   **Balancing Tools:** Balancer module (`upmap`, `read`, `crush-compat`), `pg-upmap-primary`.
*   **PG States:** `active+clean`, `degraded`, `remapped`, `stale`, `incomplete`, `peering`.
*   **Protocols & Auth:** Cephx, Messenger v2 (msgr2), Paxos, Heartbeats.
*   **QoS & Performance:** mClock, IOPS capacity, scrubbing, TCMalloc profiling.
*   **APIs:** `librados` (`IoCtx`, `AioCompletion`), `libcephsqlite`, Object Classes.
*   **Advanced Modes:** Stretch Mode, Connectivity Election, Arbiter/Tiebreaker.

## Related Components
*   **Monitor (MON):** Authority for cluster maps, quorum, and authentication.
*   **OSD:** Unit of physical storage, replication, and heartbeating.
*   **Manager (MGR):** Host for the balancer, device health, and PG autoscaler modules.
*   **LVM/ceph-volume:** Used for OSD lifecycle and BlueStore migration.
*   **Orchestrator (cephadm/Rook):** Management for hardware and LED controls.
*   **CRUSH:** The algorithm for decentralized data placement.
*   **librados:** The base library for RBD, CephFS, and RGW.
*   **RocksDB:** Internal metadata storage for BlueStore and Monitors.
*   **Messenger Layer:** Inter-daemon and client-to-daemon networking.