# RADOS Documentation Index

## Overview
This documentation area covers **RADOS (Reliable Autonomic Distributed Object Store)**, the core storage engine and foundation of the Ceph Storage Cluster. It provides comprehensive guidance on the management of OSDs, Monitors, and Managers, data placement strategies (CRUSH, Pools, Placement Groups), cluster configuration, performance tuning via mClock and the Balancer, and the `librados` API for multi-language application development.

## Files Summary
*   **rados/index.rst**: The high-level entry point for the Ceph Storage Cluster, defining the core daemons (OSD, MON, MGR).
*   **rados/man/index.rst**: A central index of manual pages for RADOS-related command-line tools.
*   **rados/operations/add-or-rm-osds.rst**: Procedures for manual OSD lifecycle management, including deployment, replacement, and data migration observation.
*   **rados/operations/erasure-code-shec.rst**: Configuration details for the SHEC (Shingled Erasure Coding) plugin, focusing on space efficiency and durability.
*   **rados/operations/health-checks.rst**: A detailed reference of health check identifiers raised by Monitors and Managers for cluster troubleshooting.
*   **rados/operations/erasure-code-lrc.rst**: Documentation for the Locally Repairable erasure code plugin, aimed at reducing recovery bandwidth.
*   **rados/operations/bluestore-migration.rst**: Migration strategies for transitioning legacy Filestore OSDs to the modern BlueStore backend.
*   **rados/operations/change-mon-elections.rst**: Instructions for configuring Monitor election strategies (Classic, Disallow, and Connectivity modes).
*   **rados/operations/crush-map-edits.rst**: Advanced guide for manually decompiling, editing, and recompiling CRUSH maps.
*   **rados/operations/pools.rst**: Comprehensive guide to managing storage pools, including creation, snapshots, quotas, and application tagging.
*   **rados/operations/erasure-code.rst**: Core concepts of Erasure Coding in Ceph, covering profiles, overwrites, and storage overhead.
*   **rados/operations/cache-tiering.rst**: (Deprecated) Configuration guide for transparently caching data between fast and slow storage tiers.
*   **rados/operations/crush-map.rst**: Detailed explanation of the CRUSH algorithm, bucket hierarchies, rules, and device classes.
*   **rados/operations/stretch-mode.rst**: Configuration for stretch clusters distributed across multiple data centers with tiebreaker monitors.
*   **rados/operations/devices.rst**: Management of physical storage devices, including health monitoring, SMART metrics, and failure prediction.
*   **rados/operations/placement-groups.rst**: Detailed guide on PGs and the PG Autoscaler for automated data distribution tuning.
*   **rados/operations/data-placement.rst**: High-level overview of how Ceph maps data to physical hardware using Pools, PGs, and CRUSH.
*   **rados/operations/erasure-code-clay.rst**: Documentation for the CLAY (coupled-layer) erasure code plugin for high-efficiency repair.
*   **rados/operations/operating.rst**: Basics of cluster management using `systemd` and legacy init scripts.
*   **rados/operations/monitoring-osd-pg.rst**: Guide to monitoring OSD states (Up/Down, In/Out) and PG peering/mapping status.
*   **rados/operations/upmap.rst**: Instructions for using `pg-upmap` to fine-tune PG distribution across OSDs.
*   **rados/operations/index.rst**: Table of contents for all high-level and low-level cluster operations.
*   **rados/operations/pg-states.rst**: A complete glossary of all possible Placement Group states (e.g., *active+clean*, *degraded*, *peering*).
*   **rados/operations/erasure-code-jerasure.rst**: Configuration for the Jerasure plugin, the most common library for erasure coding in Ceph.
*   **rados/operations/read-balancer.rst**: Guide to operating the Primary/Read balancer to optimize read performance on replicated pools.
*   **rados/operations/erasure-code-profile.rst**: Commands for managing erasure code profiles (`set`, `rm`, `get`, `ls`).
*   **rados/operations/control.rst**: Reference for core monitor, OSD, and PG subsystem control commands.
*   **rados/operations/user-management.rst**: Guide to CephX authentication, user capabilities (caps), and keyring management.
*   **rados/operations/erasure-code-isa.rst**: Configuration for the Intel Storage Acceleration Library (ISA-L) erasure code plugin.
*   **rados/operations/add-or-rm-mons.rst**: Procedures for adding, removing, or changing IP addresses for Monitor daemons.
*   **rados/operations/monitoring.rst**: Basics of monitoring health, usage statistics (`df`), and muting health warnings.
*   **rados/operations/balancer.rst**: Detailed guide to the `balancer` module and its modes (upmap, read, crush-compat).
*   **rados/operations/pg-concepts.rst**: Deep dive into technical PG concepts like Acting Sets, Up Sets, and Authoritative History.
*   **rados/configuration/common.rst**: Reference for common settings like hostnames, metavariables ($id, $type), and network paths.
*   **rados/configuration/bluestore-config-ref.rst**: In-depth configuration reference for BlueStore (caching, checksums, compression, rocksdb sharding).
*   **rados/configuration/osd-config-ref.rst**: General OSD daemon settings, including scrubbing intervals and mClock QoS parameters.
*   **rados/configuration/filestore-config-ref.rst**: (Deprecated) Legacy configuration reference for the Filestore storage backend.
*   **rados/configuration/pool-pg-config-ref.rst**: Monitor-side configuration for default pool sizes and PG limits.
*   **rados/configuration/msgr2.rst**: Reference for the Messenger v2 on-wire protocol, including encryption and compression modes.
*   **rados/configuration/auth-config-ref.rst**: Detailed CephX configuration, including signature requirements and daemon keyring locations.
*   **rados/configuration/general-config-ref.rst**: Minimal reference for daemon administrative sockets and PID files.
*   **rados/configuration/index.rst**: TOC for storage device, network, and daemon configuration.
*   **rados/configuration/mon-config-ref.rst**: Monitor-specific settings for Paxos, quorum behavior, and storage capacity thresholds.
*   **rados/configuration/network-config-ref.rst**: Detailed networking reference covering Public/Cluster networks, bonding, and firewall rules.
*   **rados/configuration/journal-ref.rst**: (Deprecated) Reference for Filestore journaling.
*   **rados/configuration/mon-osd-interaction.rst**: Configuration for heartbeats, failure reporting intervals, and grace periods.
*   **rados/configuration/ceph-conf.rst**: Guide to configuration source precedence and metavariable expansion.
*   **rados/configuration/mon-lookup-dns.rst**: Instructions for enabling Monitor discovery via DNS SRV records.
*   **rados/configuration/storage-devices.rst**: Overview of OSD backends (BlueStore vs. Filestore).
*   **rados/configuration/mclock-config-ref.rst**: Guide to mClock QoS profiles (balanced, high_client_ops, etc.) for I/O scheduling.
*   **rados/api/librados-intro.rst**: Introduction to the `librados` API and its language bindings (C, Python, Java, PHP).
*   **rados/api/librados.rst**: Core C-language API reference for `librados`, including asynchronous I/O and completions.
*   **rados/api/libradospp.rst**: Placeholder for the C++ `librados` API.
*   **rados/api/libcephsqlite.rst**: Reference for the Ceph SQLite VFS, allowing SQLite databases to be stored on RADOS.
*   **rados/api/index.rst**: TOC for the `librados` developer documentation.
*   **rados/api/python.rst**: Reference for the `rados` Python module and object interface.
*   **rados/api/objclass-sdk.rst**: Documentation for the Ceph Object Class (CLS) SDK for building server-side logic.
*   **rados/troubleshooting/troubleshooting-osd.rst**: Guide to resolving OSD failures, full disks, and performance bottlenecks.
*   **rados/troubleshooting/log-and-debug.rst**: Reference for subsystem debug levels and log rotation.
*   **rados/troubleshooting/community.rst**: Links to Ceph mailing lists and community support resources.
*   **rados/troubleshooting/troubleshooting-mon.rst**: Guide for resolving Monitor quorum issues and broken monmaps.
*   **rados/troubleshooting/cpu-profiling.rst**: Instructions for using `oprofile` to analyze Ceph CPU usage.
*   **rados/troubleshooting/index.rst**: TOC for all troubleshooting and profiling documentation.
*   **rados/troubleshooting/memory-profiling.rst**: Guide to using `google-perftools` (TCMalloc) and `Massif` for heap profiling.
*   **rados/troubleshooting/troubleshooting-pg.rst**: Solutions for stuck PGs, inconsistent objects, and peering failures.
*   **rados/operations/pgcalc/index.rst**: Embedded logic and instructions for the Ceph PG per Pool Calculator.

## Code Changes That Would Require Documentation Updates
*   **Configuration Logic**: Adding new global or daemon-specific configuration options in `common/options.cc`.
*   **Messenger Protocol**: Changes to the Messenger v2 protocol, address formats, or encryption/compression modes (`msg/messenger.h`, `msg/async/v2/`).
*   **Backend Storage**: Major updates to BlueStore, RocksDB sharding logic, or the introduction of a new OSD backend (e.g., SeaStore).
*   **Health Checks**: Adding or removing health check strings/codes in the MON or MGR (`mon/HealthMonitor.cc`, `mgr/Mgr.cc`).
*   **CRUSH Algorithm**: Changes to bucket types, placement rules, or device class logic (`crush/`).
*   **Erasure Coding**: Adding new plugins or modifying parameters (k, m, d, c) for existing plugins like Jerasure, CLAY, or SHEC.
*   **PG Management**: Changes to the PG Autoscaler logic, `pg-upmap` exceptions, or Balancer module modes.
*   **QoS/mClock**: Updates to the dmClock scheduler, mClock profiles, or OSD sharding defaults (`osd/scheduler/mClockScheduler.h`).
*   **Monitor Core**: Changes to Paxos consensus, election strategies, or monmap structure.
*   **API/Bindings**: Modifying `librados` exported functions, `rados.py` bindings, or the Object Class (CLS) SDK.
*   **CLI Tools**: Changes to subcommands for `ceph`, `rados`, `ceph-volume`, or `osdmaptool`.
*   **Stretch Mode**: Modifications to stretch cluster peering rules or tiebreaker monitor logic.

## Key Technical Concepts
*   **Daemons**: OSD (Object Storage Daemon), MON (Monitor), MGR (Manager), MDS (Metadata Server).
*   **Data Placement**: CRUSH Map, Buckets (host, rack, root), Failure Domains, Device Classes (hdd, ssd, nvme), Placement Groups (PGs).
*   **Pools**: Replicated vs. Erasure Coded, Pool Quotas, Snapshots, Application Tags, Namespaces.
*   **PG States**: Peering, Active+Clean, Degraded, Undersized, Remapped, Stale, Inconsistent.
*   **Balancing**: Balancer Module, `upmap`, `upmap-read`, `pg-upmap-primary`, Primary Balancer.
*   **QoS**: mClock, dmClock, I/O Reservation/Limitation/Weight.
*   **Auth**: CephX, Capabilities (caps), Keyrings, `client.admin`.
*   **Storage Backends**: BlueStore (Block, WAL, DB), Filestore (Legacy), RocksDB, Sharding.
*   **Commands**: `ceph status`, `ceph health`, `ceph osd df`, `ceph pg map`, `rados df`, `ceph-volume lvm create`.
*   **APIs**: `librados`, `Ioctx`, `Rados` handle, Asynchronous I/O, SQLite VFS.
*   **Clustering**: Paxos, Quorum, Monmap, Election Strategy, Stretch Mode, Tiebreaker.

## Related Components
*   **RBD (RADOS Block Device)**: Leverages RADOS for block storage.
*   **CephFS (Ceph File System)**: Uses RADOS for data and metadata pools.
*   **RGW (RADOS Gateway)**: Object storage interface (S3/Swift) built on `librados`.
*   **RocksDB**: Embedded in BlueStore for metadata management.
*   **TCMalloc / gperftools**: Used for OSD/MON memory management and profiling.
*   **cephadm / Orchestrators**: Tools for deploying the daemons configured in these docs.
*   **systemd**: Primary service manager for Ceph daemons.