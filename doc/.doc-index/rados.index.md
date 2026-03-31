# RADOS Documentation Index

## Overview
This documentation area covers RADOS (Reliable Autonomic Distributed Object Store), the core foundation of the Ceph Storage Cluster. It provides comprehensive guides on architectural daemons (OSDs, Monitors, Managers), cluster configuration (networking, authentication, storage backends), operational management (CRUSH maps, pools, placement groups), and the `librados` API for application development.

## Files Summary

### Core Configuration
*   **rados/index.rst**: High-level entry point for the Ceph Storage Cluster documentation.
*   **rados/configuration/ceph-conf.rst**: Detailed explanation of the `ceph.conf` structure, configuration sources (central database, file, env), and metavariable expansion.
*   **rados/configuration/common.rst**: Standard settings for node names, data paths, and cluster naming conventions.
*   **rados/configuration/general-config-ref.rst**: Reference for admin sockets, PID files, and file descriptor limits.

### Networking & Protocol
*   **rados/configuration/network-config-ref.rst**: Guide to public and cluster network separation, port ranges, and firewall (IP Tables) requirements.
*   **rados/configuration/msgr2.rst**: Documentation for the Messenger v2 protocol, including encryption, compression, and port 3300 requirements.
*   **rados/configuration/mon-lookup-dns.rst**: Instructions for using DNS SRV records for monitor discovery.

### Storage Backends & Devices
*   **rados/configuration/storage-devices.rst**: Overview of OSD storage devices and the transition from FileStore to BlueStore.
*   **rados/configuration/bluestore-config-ref.rst**: Configuration for BlueStore devices (WAL, DB), cache autotuning, compression, and checksums.
*   **rados/configuration/bluestore-migration.rst**: Strategies for migrating legacy FileStore OSDs to BlueStore.
*   **rados/configuration/filestore-config-ref.rst**: Legacy configuration for FileStore (deprecated).
*   **rados/configuration/journal-ref.rst**: Reference for FileStore journaling settings (deprecated).
*   **rados/operations/devices.rst**: Management of physical devices, LED control, and failure prediction (SMART).

### Authentication & Authorization
*   **rados/configuration/auth-config-ref.rst**: Reference for CephX authentication, keyrings, and message signing.
*   **rados/operations/user-management.rst**: Guide to managing users, capabilities (caps), and RADOS namespaces.

### Cluster Topology & Placement
*   **rados/operations/crush-map.rst**: Core documentation for the CRUSH algorithm, hierarchy (buckets), and rules.
*   **rados/operations/crush-map-edits.rst**: Advanced guide for manual CRUSH map decompilation and editing.
*   **rados/operations/data-placement.rst**: High-level overview of how pools, PGs, and CRUSH interact.
*   **rados/operations/stretch-mode.rst**: Configuration for clusters spanning multiple data centers with tiebreaker monitors.
*   **rados/operations/upmap.rst**: Using `pg-upmap` for manual data distribution fine-tuning.
*   **rados/operations/balancer.rst**: Management of the `ceph-mgr` balancer module for PG distribution.
*   **rados/operations/read-balancer.rst**: Operating the primary affinity balancer to optimize read performance.

### Pool & Placement Group (PG) Management
*   **rados/operations/pools.rst**: Comprehensive guide to creating pools, setting replicas, quotas, and application associations.
*   **rados/operations/placement-groups.rst**: Detailed management of PGs, including the PG autoscaler (`pg_autoscale_mode`).
*   **rados/operations/pg-concepts.rst**: Glossary and concepts including Peering, Acting Sets, and Up Sets.
*   **rados/operations/pg-states.rst**: Exhaustive list of PG states (clean, degraded, peering, etc.).
*   **rados/operations/pgcalc/index.rst**: Logic and formulas used for calculating optimal PG counts.

### Erasure Coding (EC)
*   **rados/operations/erasure-code.rst**: Basics of EC pools, overwrites, and optimizations.
*   **rados/operations/erasure-code-profile.rst**: How to manage and set EC profiles.
*   **rados/operations/erasure-code-[jerasure, isa, lrc, shec, clay].rst**: Specific plugin documentation for different EC techniques.

### Cluster Operations & Monitoring
*   **rados/configuration/mon-config-ref.rst**: Monitor-specific configuration, Paxos consensus, and quorum requirements.
*   **rados/configuration/osd-config-ref.rst**: OSD-specific settings, including scrubbing and DMClock QoS.
*   **rados/configuration/mclock-config-ref.rst**: Detailed reference for mClock-based QoS profiles (Balanced, High Client Ops, etc.).
*   **rados/configuration/mon-osd-interaction.rst**: Internal heartbeat and failure reporting mechanisms between OSDs and Monitors.
*   **rados/operations/operating.rst**: Basics of starting/stopping daemons via `systemd`.
*   **rados/operations/control.rst**: CLI command reference for managing cluster subsystems.
*   **rados/operations/monitoring.rst**: High-level tools for checking cluster health and usage.
*   **rados/operations/monitoring-osd-pg.rst**: Techniques for monitoring OSD and PG specific health.
*   **rados/operations/health-checks.rst**: List and explanation of all cluster health check codes (e.g., OSD_DOWN).
*   **rados/operations/cache-tiering.rst**: Configuration for cache tiers (deprecated).
*   **rados/operations/change-mon-elections.rst**: Configuring monitor election strategies (Classic, Disallow, Connectivity).

### Development & APIs
*   **rados/api/index.rst**: Entry point for RADOS developers.
*   **rados/api/librados-intro.rst**: Introduction to the `librados` library for C, C++, Python, Java, and PHP.
*   **rados/api/librados.rst**: C language specific API bindings and examples.
*   **rados/api/python.rst**: Python language specific API bindings and usage.
*   **rados/api/libcephsqlite.rst**: Documentation for the SQLite VFS implementation on top of RADOS.
*   **rados/api/objclass-sdk.rst**: SDK for creating custom Object Classes (CLS) to extend Ceph.

### Troubleshooting
*   **rados/troubleshooting/index.rst**: Troubleshooting entry point.
*   **rados/troubleshooting/log-and-debug.rst**: Adjusting log levels and interpreting debug output.
*   **rados/troubleshooting/troubleshooting-mon.rst**: Resolving quorum, clock skew, and monmap issues.
*   **rados/troubleshooting/troubleshooting-osd.rst**: Resolving OSD startup failures, full OSDs, and performance bottlenecks.
*   **rados/troubleshooting/troubleshooting-pg.rst**: Resolving peering failures and unfound objects.
*   **rados/troubleshooting/memory-profiling.rst**: Instructions for heap profiling with TCMalloc.
*   **rados/troubleshooting/cpu-profiling.rst**: Instructions for CPU profiling with OProfile.

## Code Changes That Would Require Documentation Updates

*   **Config System:** Adding, renaming, or deprecating global or daemon-specific configuration options in `Common.cc` or `config_opts.h`.
*   **CLI Changes:** Modifying the output format of `ceph status`, `ceph osd df`, or `ceph pg dump` (affects monitoring/troubleshooting docs).
*   **New Daemons:** Introduction of new daemon types beyond MON, OSD, MGR, and MDS.
*   **Protocol Versions:** Updates to the Messenger protocol (msgr2) or additions of new connection/encryption modes.
*   **Storage Engines:** Significant changes to BlueStore (e.g., new device types, sharding logic) or the introduction of a new backend like SeaStore.
*   **mClock/QoS:** Changes to the default mClock profiles or the introduction of new QoS scheduling buckets.
*   **CRUSH Algorithm:** Adding new bucket types, straw2 algorithm changes, or new CRUSH rule steps.
*   **Erasure Coding:** New EC plugins or changes to existing ones (e.g., CLAY, SHEC).
*   **Health Checks:** Adding new health check identifiers (identifiers in `HealthMonitor.cc`) or changing the thresholds for existing ones.
*   **API Changes:** Modifications to the `librados` public headers (`librados.h`, `librados.hpp`) or the Python/Java/PHP wrappers.
*   **Stretch Mode:** Changes to the election strategies or the logic for degraded stretch mode peering.
*   **Manager Modules:** Updates to core modules like `balancer`, `pg_autoscaler`, or `devicehealth`.

## Key Technical Concepts

*   **Daemons:** OSD (Object Storage Daemon), MON (Monitor), MGR (Manager), MDS (Metadata Server).
*   **Data Structures:** Cluster Map, Monmap, OSDMap, PGMap, CRUSH Map.
*   **Placement Logic:** CRUSH (Controlled Replication Under Scalable Hashing), Placement Group (PG), Pool, Namespace, Device Class, Primary Affinity.
*   **OSD Backend:** BlueStore, WAL (Write-Ahead Log), DB device, RocksDB sharding, FileStore (Legacy).
*   **Network:** Public Network, Cluster Network, msgr2 protocol, port 3300, port 6789.
*   **State Machine:** Peering, Recovery, Backfill, Scrubbing (Light/Deep), Quorum, Epoch.
*   **QoS:** DMClock, mClock profiles (balanced, high_client_ops).
*   **Security:** CephX, Keyring, Capabilities (Caps).
*   **Operations:** `pg_autoscale_mode`, `upmap`, `pg-upmap-primary`, `stretch-mode`.
*   **Health Codes:** `OSD_DOWN`, `MON_DOWN`, `PG_DEGRADED`, `OSD_FULL`, `MON_CLOCK_SKEW`.

## Related Components

*   **Cephadm/Orchestrator:** Used for deployment (referenced in bootstrap and device management).
*   **CephFS:** Uses RADOS for data and metadata storage.
*   **RBD (Ceph Block Device):** Built on top of `librados`.
*   **RGW (RADOS Gateway):** Uses RADOS for object storage and index/metadata (namespaces, omap).
*   **RocksDB:** Embedded within BlueStore and Monitors for metadata storage.
*   **Systemd:** Used for daemon management.
*   **Smartmontools:** Used for device health scraping.