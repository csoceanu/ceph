# RADOS Documentation Index

## Overview
This documentation covers the Ceph Storage Cluster, specifically the **Reliable Autonomic Distributed Object Store (RADOS)**. It provides comprehensive guidance on cluster architecture, day-to-day operations, data placement strategies (CRUSH, PGs, Pools), hardware management, and troubleshooting procedures for the core storage daemons (OSDs, Monitors, and Managers).

## Files Summary
*   **rados/index.rst**: The landing page for the Ceph Storage Cluster, providing an architectural overview of OSDs, MONs, and MGRs.
*   **rados/man/index.rst**: A directory of manpages for core object store utilities like `ceph-volume`, `crushtool`, and `rados`.
*   **rados/operations/add-or-rm-osds.rst**: Instructions for manually adding, removing, and replacing OSDs, including hardware preparation and CRUSH map integration.
*   **rados/operations/erasure-code-shec.rst**: Technical details for the Shingled Erasure Code (SHEC) plugin, focusing on recovery efficiency.
*   **rados/operations/health-checks.rst**: A comprehensive reference of health check IDs (e.g., `MON_DISK_LOW`, `OSD_FULL`) and their remediation steps.
*   **rados/operations/erasure-code-lrc.rst**: Documentation for the Locally Repairable erasure code (LRC) plugin and its locality-based recovery.
*   **rados/operations/bluestore-migration.rst**: Strategies for migrating OSDs from the legacy Filestore back end to BlueStore.
*   **rados/operations/change-mon-elections.rst**: Explains monitor election strategies (Classic, Disallow, Connectivity) and connectivity scoring.
*   **rados/operations/crush-map-edits.rst**: Detailed guide on decompiling, editing, and recompiling the CRUSH map manually.
*   **rados/operations/pools.rst**: Core management of RADOS pools, covering creation, quotas, snapshots, and configuration values.
*   **rados/operations/erasure-code.rst**: General overview of erasure coding in Ceph, profiles, overhead calculations, and overwrites.
*   **rados/operations/cache-tiering.rst**: Documentation for the (deprecated) cache tiering feature, modes (writeback/readonly), and sizing.
*   **rados/operations/crush-map.rst**: Explains the CRUSH algorithm, bucket hierarchies, failure domains, device classes, and tunables.
*   **rados/operations/stretch-mode.rst**: Configuration for stretch clusters across geographically separated sites with tiebreaker monitors.
*   **rados/operations/devices.rst**: Management of physical storage devices, including SMART monitoring, failure prediction, and LED control.
*   **rados/operations/placement-groups.rst**: Detailed explanation of PGs, the PG Autoscaler, scaling thresholds, and durability factors.
*   **rados/operations/data-placement.rst**: High-level overview of how Pools, PGs, and CRUSH work together to place data.
*   **rados/operations/erasure-code-clay.rst**: Documentation for the Coupled-Layer (CLAY) erasure code plugin and its sub-chunking mechanism.
*   **rados/operations/operating.rst**: Practical guide for managing Ceph daemons using `systemd` or legacy `SysVinit`.
*   **rados/operations/monitoring-osd-pg.rst**: Techniques for monitoring OSD health and PG sets (Up Set vs. Acting Set).
*   **rados/operations/upmap.rst**: Explains the `pg-upmap` mechanism for manual and automated PG distribution fine-tuning.
*   **rados/operations/index.rst**: Table of contents for all cluster operational documentation.
*   **rados/operations/pg-states.rst**: A glossary of all possible Placement Group states (e.g., *peering*, *backfilling*, *laggy*).
*   **rados/operations/erasure-code-jerasure.rst**: Details for the Jerasure plugin, supporting various Reed-Solomon and Cauchy techniques.

## Code Changes That Would Require Documentation Updates
*   **New Health Checks**: Adding a new `health_check_t` in the Monitor or Manager code requires updating `health-checks.rst`.
*   **EC Plugin Development**: Changes to erasure code logic, new plugins (like a new `ErasureCodePlugin`), or new techniques (like a variation of Reed-Solomon) require updates to `erasure-code-*.rst`.
*   **CRUSH Algorithm Modifications**: Any change to the `CrushWrapper`, new bucket types, or new `step` types in the CRUSH map syntax require updates to `crush-map.rst` and `crush-map-edits.rst`.
*   **PG Autoscaler Logic**: Adjustments to the scaling algorithm, threshold calculations, or new `pg_autoscale_mode` options require updates to `placement-groups.rst`.
*   **BlueStore/Store Changes**: Modifications to BlueStore metadata (e.g., per-pool omap tracking) or deprecation of backends (like Filestore) must be reflected in `bluestore-migration.rst` and `pools.rst`.
*   **CLI/Command Changes**: Adding subcommands to `ceph osd`, `ceph pg`, or `ceph monitor` necessitates updates to the Command Reference and related operation files.
*   **Monitor Election Logic**: Any change to `ElectionStrategy.cc` or the introduction of new scoring metrics requires updates to `change-mon-elections.rst`.
*   **Device Management**: New metrics collected by `ceph-mgr` health modules or changes to `smartctl` scraping logic require updates to `devices.rst`.
*   **Stretch Mode Enhancements**: Support for more than two sites or EC pools in stretch mode would require significant updates to `stretch-mode.rst`.

## Key Technical Concepts
*   **Daemons**: OSD (Object Storage Daemon), MON (Monitor), MGR (Manager).
*   **Data Structures**: Pool, Placement Group (PG), CRUSH Map, OSDMap.
*   **PG States**: `active+clean`, `peering`, `degraded`, `remapped`, `backfilling`, `stale`, `incomplete`.
*   **Placement Logic**: Acting Set, Up Set, Primary OSD, Primary Affinity, `pg-upmap`, Balancer Module.
*   **Erasure Coding Parameters**: K (Data Chunks), M (Coding Chunks), d (Helper Chunks), c (Durability Estimator), Stripe Unit.
*   **CRUSH Elements**: Buckets (Host, Rack, Root), Device Classes (HDD, SSD, NVMe), Failure Domains, Tunables.
*   **Operational Modes**: Writeback vs. Readproxy (Cache Tiering), Degraded vs. Recovery (Stretch Mode), Connectivity Mode (MON Elections).
*   **Hardware Backends**: BlueStore, Filestore, RocksDB (for metadata).

## Related Components
*   **ceph-volume**: The tool used to provision and activate OSD devices.
*   **ceph-mgr modules**: Specifically the `balancer`, `pg_autoscaler`, `devicehealth`, and `telemetry` modules.
*   **systemd**: The service manager used to control Ceph daemon lifecycles.
*   **smartmontools**: External dependency (`smartctl`) used for device health scraping.
*   **LVM**: Used by `ceph-volume` to manage the logical volumes underlying BlueStore.
*   **RADOS Gateway (RGW) / CephFS / RBD**: These high-level storage services consume the RADOS pools described in this index.

---

# RADOS Documentation Index

## Overview
This documentation covers the core Reliable Autonomic Distributed Object Store (RADOS) layer of Ceph. it provides comprehensive guidance on cluster operations (balancing, monitoring, and scaling), configuration of core daemons (OSDs, Monitors, and Managers), security/authentication (Cephx), networking (Msgr2), and the low-level `librados` API for multi-language development.

## Files Summary

### Operations & Management
*   **rados/operations/read-balancer.rst**: Instructions for operating the read/primary balancer to optimize pool performance using `pg-upmap-primary`.
*   **rados/operations/erasure-code-profile.rst**: Details on managing erasure coding profiles, including plugin selection and stripe unit configuration.
*   **rados/operations/control.rst**: Comprehensive list of CLI commands for interacting with Monitor, OSD, MDS, and PG subsystems.
*   **rados/operations/user-management.rst**: Guide to Cephx authentication, managing user capabilities (caps), and handling keyrings/namespaces.
*   **rados/operations/erasure-code-isa.rst**: Specific configuration parameters for the Intel ISA-L erasure coding plugin.
*   **rados/operations/add-or-rm-mons.rst**: Procedures for maintaining monitor quorum, including manual deployment, removal, and changing network addresses.
*   **rados/operations/monitoring.rst**: Protocols for checking cluster health, data usage statistics, network performance, and daemon status.
*   **rados/operations/balancer.rst**: Documentation for the Manager `balancer` module, covering automatic optimization modes like `upmap` and `crush-compat`.
*   **rados/operations/pg-concepts.rst**: Definitional reference for Placement Group states and lifecycle terms (Peering, Recovery, Epochs, etc.).

### Configuration
*   **rados/configuration/ceph-conf.rst**: Core reference for configuration sources, section syntax, metavariables, and the Monitor configuration database.
*   **rados/configuration/common.rst**: Overview of basic settings for networks, authentication, and daemon data paths.
*   **rados/configuration/bluestore-config-ref.rst**: Detailed reference for BlueStore back-end settings, including cache autotuning, compression, and device management (WAL/DB).
*   **rados/configuration/osd-config-ref.rst**: Reference for OSD daemon settings, scrubbing intervals, and mClock-based Quality of Service (QoS).
*   **rados/configuration/mon-config-ref.rst**: Monitor architecture reference covering Paxos consensus, quorum requirements, and storage capacity thresholds.
*   **rados/configuration/network-config-ref.rst**: Detailed guide on public/cluster network separation, IPTables configuration, and binding settings.
*   **rados/configuration/mclock-config-ref.rst**: Configuration for mClock profiles to balance client I/O against background recovery/scrub operations.
*   **rados/configuration/msgr2.rst**: Reference for the Messenger v2 protocol, including secure/encrypted connection modes and address formats.
*   **rados/configuration/auth-config-ref.rst**: Technical details on Cephx protocol enablement, message signing, and daemon-specific keyrings.
*   **rados/configuration/storage-devices.rst**: High-level comparison of OSD storage back-ends (BlueStore vs. legacy FileStore).
*   **rados/configuration/mon-lookup-dns.rst**: Configuration for using DNS SRV records to discover cluster monitors.
*   **rados/configuration/mon-osd-interaction.rst**: Tuning heartbeats, failure reporting intervals, and subtree-level monitoring.
*   **rados/configuration/filestore-config-ref.rst**: Legacy reference for the deprecated FileStore storage back-end.
*   **rados/configuration/pool-pg-config-ref.rst**: Configuration for default pool sizes, PG counts, and CRUSH rules.
*   **rados/configuration/journal-ref.rst**: Legacy reference for FileStore journal speed and consistency settings.
*   **rados/configuration/general-config-ref.rst**: Basic process settings for admin sockets and PID files.

### Developer APIs
*   **rados/api/librados-intro.rst**: Introduction to `librados` for C, C++, Python, Java, and PHP, covering cluster handles and I/O contexts.
*   **rados/api/librados.rst**: Low-level C API reference for RADOS access, including synchronous and asynchronous I/O patterns.
*   **rados/api/libradospp.rst**: Placeholder for the Librados C++ API documentation.

## Code Changes That Would Require Documentation Updates

### 1. CLI & Command Infrastructure
*   Adding new subcommands to `ceph`, `rados`, `osdmaptool`, or `monmaptool`.
*   Changes to CLI output formats (especially JSON) for monitoring tools.
*   Deprecated commands (e.g., changes to `ceph osd reweight-by-utilization` vs. `balancer`).

### 2. Configuration Subsystem
*   Introduction of new `confval` entries in `common/options.cc`.
*   Modifying default values for heartbeats, timeouts, or capacity thresholds (e.g., `mon_osd_full_ratio`).
*   Changes to the "Central Config" database logic or precedence rules.
*   Adding new configuration metavariables.

### 3. Storage Back-ends (BlueStore/FileStore)
*   Changes to BlueStore's allocation logic (e.g., `min_alloc_size`).
*   New compression algorithms or checksum types.
*   Changes to RocksDB column family sharding or metadata management.
*   New offloading technologies (e.g., DSA/SPDK updates).

### 4. mClock & QoS
*   Adding new mClock profiles or modifying the reservation/limit/weight ratios of existing ones.
*   Changes to OSD capacity benchmarking logic (automated vs. manual).
*   Modifying OSD shard or thread defaults for specific media types (HDD vs. SSD).

### 5. Networking & Protocol (Msgr2)
*   Revisions to the wire protocol (Msgr3+).
*   Changes to encryption/compression modes in the messenger.
*   Modifying default port assignments or address format types.

### 6. Librados API
*   Adding new exported symbols/functions to `librados.h` or `librados.hpp`.
*   Changes to I/O completion logic or asynchronous callback structures.
*   New bindings for additional programming languages.

### 7. Core Subsystems (Mon/OSD/Balancer)
*   Changes to Paxos election strategies or synchronization logic.
*   Updates to the balancer module (new modes, logic changes to `upmap` or `read-balancer`).
*   Changes to PG state transitions (Peering, Recovery, Backfill).

## Key Technical Concepts
*   **Daemons**: `ceph-mon`, `ceph-osd`, `ceph-mgr`, `ceph-mds`, `radosgw`.
*   **Balancing**: `upmap`, `pg-upmap-primary`, `read_balance_score`, `crush-compat`.
*   **Storage Back-ends**: BlueStore (WAL, DB, Main), FileStore (deprecated).
*   **Authentication**: Cephx, Keyrings, Capabilities (Caps), Signatures.
*   **Networking**: Public Network, Cluster Network, Msgr2 (v1/v2 addresses), CRC vs. Secure modes.
*   **Placement Groups (PGs)**: Peering, Acting Set, Up Set, Recovery, Backfill, Scrubbing.
*   **mClock**: Reservations, Limits, Weights, Balanced/High-Client/High-Recovery profiles.
*   **API Concepts**: Cluster Handle, I/O Context, AIO Completions, Bufferlists.
*   **Redundancy**: Replication, Erasure Coding (K/M chunks, ISA/Jerasure plugins).

## Related Components
*   **Ceph Manager (MGR)**: Hosts the balancer module and mClock autotuning.
*   **Monitor (MON)**: Maintains the cluster map, Paxos database, and auth subsystem.
*   **OSD**: Handles data storage, heartbeats, and peer-to-peer recovery.
*   **CRUSH**: The underlying algorithm for data placement and hierarchy management.
*   **RocksDB**: Key/value store used for BlueStore and Monitor metadata.
*   **Librados**: The foundational library for RBD, CephFS, and RGW.

---

# RADOS Documentation Index

## Overview
This documentation area covers the core interfaces and operational maintenance of the Reliable Autonomic Distributed Object Store (RADOS), the foundation of the Ceph storage cluster. It provides technical guidance for developers using `librados` (specifically Python) and the Object Class SDK, as well as comprehensive troubleshooting procedures for OSDs, Monitors, and Placement Groups (PGs).

## Files Summary
*   **rados/api/libcephsqlite.rst**: Details the SQLite VFS implementation for RADOS, allowing decentralized SQLite databases to be backed by Ceph objects, including locking and performance tuning.
*   **rados/api/index.rst**: The primary entry point for all Ceph Storage Cluster APIs, including C, C++, Python, and Object Classes.
*   **rados/api/python.rst**: A comprehensive guide and API reference for the `rados` Python module, covering cluster handles, I/O contexts, and object operations.
*   **rados/api/objclass-sdk.rst**: Describes the Software Development Kit for creating Ceph Object Classes, enabling custom server-side logic outside the core Ceph source tree.
*   **rados/troubleshooting/troubleshooting-osd.rst**: Guidance for resolving OSD failures, performance bottlenecks, "flapping" states, and disk space exhaustion.
*   **rados/troubleshooting/log-and-debug.rst**: Explains how to manipulate subsystem logging levels at runtime and boot time to diagnose cluster issues.
*   **rados/troubleshooting/community.rst**: Lists official communication channels (mailing lists, Slack, IRC) for obtaining community support.
*   **rados/troubleshooting/troubleshooting-mon.rst**: Detailed procedures for fixing Monitor quorum issues, clock skews, and corrupted monitor stores.
*   **rados/troubleshooting/cpu-profiling.rst**: Instructions for using `oprofile` to analyze Ceph's CPU utilization.
*   **rados/troubleshooting/index.rst**: The central directory for all RADOS-related troubleshooting and profiling documentation.
*   **rados/troubleshooting/memory-profiling.rst**: Covers heap profiling using `TCMalloc` and `Valgrind/Massif` to diagnose memory leaks or high consumption.
*   **rados/troubleshooting/troubleshooting-pg.rst**: Resolving complex Placement Group states including "stuck," "unfound," "stale," and "inconsistent" PGs.
*   **rados/operations/pgcalc/index.rst**: Logic and calculator for determining the optimal number of Placement Groups based on cluster size and use case.

## Code Changes That Would Require Documentation Updates
*   **Librados/Python API Changes**: Adding, removing, or changing signatures of methods in the `rados` Python module or the underlying C/C++ libraries.
*   **New Subsystems**: Adding new internal Ceph modules that require their own `debug_{subsystem}` logging configuration.
*   **Default Configuration Shifts**: Changing default values for "full" ratios (`mon_osd_full_ratio`), mClock scheduler settings, or monitor timeout intervals.
*   **CLI Command Updates**: Modifying the syntax or output format of `ceph tell`, `ceph daemon`, `ceph-monstore-tool`, or `ceph-objectstore-tool`.
*   **SQLite VFS Modifications**: Changing the RADOS URI format for SQLite (`file:///pool:ns/db?vfs=ceph`) or updating lock renewal logic.
*   **Object Class SDK**: Updates to `objclass.h` or changes in how shared objects are loaded/built out-of-tree.
*   **Health Check Logic**: Introducing new PG states or modifying the criteria for "stale," "inactive," or "unclean" warnings.
*   **Scheduler Logic (mClock)**: Changes to how OSD shards or threads are allocated for HDD vs. SSD workloads.
*   **Blocklisting Changes**: Updates to the `osd blocklist` command or how "dead lockers" are handled in `libcephsqlite`.

## Key Technical Concepts
*   **librados**: The fundamental library for interacting with the Ceph object store.
*   **Ioctx (I/O Context)**: The operational handle for performing I/O on specific pools and namespaces.
*   **SQLite VFS**: An implementation of the SQLite virtual file system that stripes database files across RADOS objects.
*   **Quorum**: The majority consensus required for Ceph Monitors to operate.
*   **Peering & Scrubbing**: The processes by which PGs agree on data state and verify data integrity.
*   **TCMalloc & Heap Profiling**: Memory management and diagnostic tools for identifying memory usage patterns.
*   **CRUSH Map & Tunables**: The algorithm and settings governing data placement and failure domains.
*   **mClock Scheduler**: A quality-of-service oriented OSD scheduler for balancing client I/O and background operations.
*   **Object Classes (cls)**: Dynamic plugins that allow custom operations to be executed on the OSD.
*   **Flapping OSDs**: A state where OSDs repeatedly join and leave the cluster in rapid succession.

## Related Components
*   **Ceph OSD (Object Storage Daemon)**: Manages data replication, erasure coding, and recovery.
*   **Ceph Monitor (MON)**: Maintains the "map" of the cluster state and ensures quorum.
*   **Ceph Manager (MGR)**: Handles cluster orchestration and performance monitoring.
*   **BlueStore/FileStore**: The underlying OSD storage backends.
*   **RADOS Striper**: Used by `libcephsqlite` and the CLI to spread files across multiple objects.
*   **Linux Kernel**: Impacted via `nf_conntrack`, `SyncFS`, and `PID_max` settings mentioned in troubleshooting.