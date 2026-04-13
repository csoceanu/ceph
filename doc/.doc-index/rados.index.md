# RADOS Documentation Index

## Overview
This documentation covers the core of the Ceph system: RADOS (Reliable Autonomic Distributed Object Store). It describes the architecture, operation, configuration, and APIs of the storage cluster's primary daemons—Monitors (MON), OSDs, and Managers (MGR)—along with the mechanisms for data placement, cluster health, and reliability.

## Files Summary
*   **index.rst**: The high-level entry point for the RADOS documentation, categorizing content into configuration, operations, and APIs.
*   **man/index.rst**: A directory of manual pages for Ceph-specific command-line utilities and system administration tools.
*   **operations/add-or-rm-osds.rst**: Detailed procedures for expanding or shrinking cluster capacity by managing OSD daemons manually.
*   **operations/erasure-code-shec.rst**: Technical details and profile parameters for the Shingled Erasure Code (SHEC) plugin.
*   **operations/health-checks.rst**: A comprehensive catalog of health check identifiers (e.g., `MON_DOWN`, `OSD_FULL`) with explanations and resolution steps.
*   **operations/erasure-code-lrc.rst**: Configuration for Locally Repairable erasure codes designed to minimize recovery bandwidth between racks/hosts.
*   **operations/bluestore-migration.rst**: Guide for migrating OSDs from the legacy FileStore backend to the modern BlueStore backend.
*   **operations/change-mon-elections.rst**: Instructions for configuring monitor election strategies, including `classic`, `disallow`, and `connectivity` modes.
*   **operations/crush-map-edits.rst**: Advanced guide for manually decompiling, editing, and recompiling the CRUSH map for custom data distribution.
*   **operations/pools.rst**: Documentation on creating and managing logical storage partitions, snapshots, quotas, and application associations.
*   **operations/erasure-code.rst**: General introduction to erasure-coded pools, overwrites, and performance/overhead trade-offs.
*   **operations/cache-tiering.rst**: Configuration guide for (deprecated) cache tiering logic, used to put fast storage in front of slower backing tiers.
*   **operations/crush-map.rst**: Core conceptual documentation for the CRUSH algorithm, bucket hierarchies, device classes, and weights.
*   **operations/stretch-mode.rst**: Configuration for "Stretch Clusters" distributed across geographically separated data centers with witness monitors.
*   **operations/devices.rst**: Guide to device management, health tracking, LED "blinking" for disk identification, and failure prediction.
*   **operations/placement-groups.rst**: Details on Placement Groups (PGs), including the PG Autoscaler and how objects map to storage.
*   **operations/data-placement.rst**: High-level overview of how Pools, PGs, and CRUSH work together to distribute data dynamically.
*   **operations/erasure-code-clay.rst**: Documentation for the Coupled-Layer (CLAY) erasure code plugin aimed at reducing repair network/disk IO.
*   **operations/operating.rst**: Reference for managing Ceph services via `systemd` or legacy `SysVinit` init systems.
*   **operations/monitoring-osd-pg.rst**: Instructions for tracking the lifecycle and state transitions of OSDs and Placement Groups.
*   **operations/upmap.rst**: Guide to using the `pg-upmap` mechanism for fine-grained, manual optimization of PG distribution.
*   **operations/index.rst**: Table of contents for RADOS operational tasks, including high-level monitoring and troubleshooting.
*   **operations/pg-states.rst**: Dictionary defining all possible PG states (e.g., `creating`, `peering`, `backfilling`, `stale`).
*   **operations/erasure-code-jerasure.rst**: Configuration reference for the widely-used Jerasure erasure code plugin.
*   **operations/read-balancer.rst**: Instructions for using the primary-read balancer to optimize performance in replicated pools.
*   **operations/erasure-code-profile.rst**: Guide to setting and getting erasure coding profiles and their associated CRUSH rules.
*   **operations/control.rst**: Command-line reference for subsystem-specific control (e.g., `ceph osd`, `ceph pg`, `ceph mon`).
*   **operations/user-management.rst**: Documentation for CephX authentication, user creation, and capability-based authorization.
*   **operations/erasure-code-isa.rst**: Reference for the Intel Storage Acceleration Library (ISA-L) erasure code plugin.
*   **operations/add-or-rm-mons.rst**: Procedures for adding or removing monitors and handling quorum loss in unhealthy clusters.
*   **operations/monitoring.rst**: Introduction to cluster status checking and watching real-time cluster logs.
*   **operations/balancer.rst**: Guide to the `ceph-mgr` balancer module for automated data distribution optimization.
*   **operations/pg-concepts.rst**: Glossary of fundamental PG terminology including Acting Sets, Up Sets, and Peering.
*   **configuration/common.rst**: Reference for foundational daemon settings, data paths, and configuration metavariables.
*   **configuration/bluestore-config-ref.rst**: Low-level tuning for BlueStore, including cache sizing, device sharding, and checksums.
*   **configuration/osd-config-ref.rst**: Extensive reference for OSD operational parameters, scrubbing, recovery, and mClock QoS.
*   **configuration/filestore-config-ref.rst**: Legacy configuration reference for the (deprecated) FileStore storage backend.
*   **configuration/pool-pg-config-ref.rst**: Configuration settings governing default PG numbers, log entries, and pool attributes.
*   **configuration/msgr2.rst**: Details on the Messenger v2 protocol, secure connection modes, and IPv4/IPv6 binding.
*   **configuration/auth-config-ref.rst**: Configuration for CephX authentication, keyring paths, and message signature requirements.
*   **configuration/general-config-ref.rst**: Global cluster settings like admin sockets and process ID file locations.
*   **configuration/index.rst**: Index of all RADOS configuration and optimization reference guides.
*   **configuration/mon-config-ref.rst**: Deep dive into monitor consistency (Paxos), storage capacity ratios, and quorum logic.
*   **configuration/network-config-ref.rst**: Infrastructure guide for public vs. cluster networks, firewalling, and interface bonding.
*   **configuration/journal-ref.rst**: Reference for legacy FileStore journaling mechanisms.
*   **configuration/mon-osd-interaction.rst**: Configuration of heartbeat intervals and failure reporting thresholds between daemons.
*   **configuration/ceph-conf.rst**: Manual for the `ceph.conf` file format, configuration sources, and runtime override methods.
*   **configuration/mon-lookup-dns.rst**: Details on monitor discovery using DNS SRV records.
*   **configuration/storage-devices.rst**: Overview of OSD backends (BlueStore vs. FileStore) and daemon storage requirements.
*   **configuration/mclock-config-ref.rst**: Guide to the mClock-based QoS scheduler and its built-in performance profiles.
*   **api/librados-intro.rst**: Introduction to writing applications against RADOS using C, C++, Python, Java, and PHP.
*   **api/librados.rst**: Detailed C API reference for librados, including synchronous and asynchronous I/O.
*   **api/libradospp.rst**: Placeholder/index for the C++ object-oriented interface to librados.
*   **api/libcephsqlite.rst**: Documentation for using SQLite on top of RADOS via a custom VFS.
*   **api/index.rst**: Directory of all RADOS-level APIs and developer SDKs.
*   **api/python.rst**: Guide and method reference for the `python-rados` module.
*   **api/objclass-sdk.rst**: Documentation for developing out-of-tree Ceph Object Classes (CLS).
*   **troubleshooting/troubleshooting-osd.rst**: Guide for diagnosing failed processes, slow I/O, and disk capacity issues.
*   **troubleshooting/log-and-debug.rst**: Manual for subsystem logging levels, log rotation, and debugging at runtime.
*   **troubleshooting/community.rst**: Links and instructions for Ceph community support resources (mailing lists, bug trackers).
*   **troubleshooting/troubleshooting-mon.rst**: Steps for diagnosing quorum issues, clock skew, and monitor store corruption.
*   **troubleshooting/cpu-profiling.rst**: Guide to using `oprofile` for analyzing Ceph daemon CPU performance.
*   **troubleshooting/index.rst**: Entry point for all cluster troubleshooting and profiling documentation.
*   **troubleshooting/memory-profiling.rst**: Instructions for heap profiling with TCMalloc and Massif to diagnose memory leaks.
*   **troubleshooting/troubleshooting-pg.rst**: Detailed resolutions for stuck, inconsistent, or "unfound" Placement Groups.
*   **operations/pgcalc/index.rst**: A conceptual and logical guide to the PG per Pool calculator tool.

## Code Changes That Would Require Documentation Updates
*   **Daemon Logic Changes**: Any change to the startup process, default data paths (e.g., `/var/lib/ceph`), or daemon roles (MON, MGR, OSD).
*   **CRUSH Algorithm**: Changes to the bucket hierarchy, new bucket types, or modifications to how device classes (HDD, SSD, NVMe) are handled.
*   **mClock QoS Profiles**: Adding, removing, or changing the default IOPS/bandwidth weights for `balanced`, `high_client_ops`, or `high_recovery_ops`.
*   **Messenger Protocol (msgr2)**: Introduction of new encryption algorithms, changes to port assignments (3300/6789), or modifications to v1/v2 feature negotiation.
*   **BlueStore Internals**: Changes to RocksDB sharding, allocator behavior, cache autotuning logic, or checksum types.
*   **PG Autoscaler/Balancer**: Changes to the logic for scaling `pg_num`, new balancer modes (beyond `upmap` and `read`), or threshold adjustments.
*   **Health Checks**: Addition of new health check codes or changes to the severity/message of existing ones (e.g., `OSD_DOWN`).
*   **CephX Authentication**: Changes to the capability (caps) syntax or the introduction of new authentication providers (like Kerberos).
*   **API Signatures**: Any changes to `librados.h` or its language bindings (Python, Java, etc.) that modify function parameters or return values.
*   **Erasure Coding**: Adding new EC plugins or modifying the parameters for existing ones (Jerasure, ISA-L, SHEC, CLAY).
*   **Stretch Mode Logic**: Modifications to the peering rules, tiebreaker monitor behavior, or degraded mode transitions for multi-site clusters.
*   **Deprecations**: Removal of legacy features like FileStore, Cache Tiering, or SysVinit support.

## Key Technical Concepts
*   **Daemons**: `ceph-osd`, `ceph-mon`, `ceph-mgr`, `ceph-mds`, `radosgw`.
*   **Storage Entities**: Pools (replicated/erasure-coded), Placement Groups (PGs), Objects, Namespaces.
*   **CRUSH**: Map, Hierarchy, Buckets (root, host, rack, etc.), Rules, Device Classes, Weights, `straw2`.
*   **Backend**: BlueStore (WAL, DB, `block`), FileStore (Journal, XATTRs), `rocksdb`.
*   **Distribution Tools**: Balancer module, PG Autoscaler, `upmap`, `pg-upmap-primary`.
*   **Authentication**: CephX, Keyrings, Capabilities (caps), Profiles (bootstrap-osd, etc.).
*   **Networking**: Public Network, Cluster Network, msgr1, msgr2, `crc` vs `secure` modes.
*   **QoS/Performance**: mClock, dmClock, I/O Priority, Recovery vs Backfill, Scrubbing (light/deep).
*   **Cluster States**: Quorum, Peering, Acting Set, Up Set, Authoritative History, Epoch.
*   **APIs**: `librados`, `rados_t`, `ioctx`, `AIO`, `libcephsqlite`, `cls` (Object Classes).
*   **Commands**: `ceph status`, `ceph health`, `ceph osd tree`, `ceph osd crush`, `rados df`.

## Related Components
*   **Cephadm / Orchestrators**: Used for deploying and managing the daemons described here.
*   **Kernel (libceph)**: The Linux kernel client for Ceph that must support features like `msgr2` and `upmap`.
*   **systemd**: The service manager for controlling daemon processes.
*   **RocksDB**: The embedded database used by BlueStore and Monitors for metadata and Paxos storage.
*   **TCMalloc / gperftools**: Used for memory allocation and heap profiling.
*   **NTP / Chrony**: Critical for avoiding Monitor clock skew issues.
*   **LVM**: Used by `ceph-volume` to provision storage for BlueStore and FileStore.