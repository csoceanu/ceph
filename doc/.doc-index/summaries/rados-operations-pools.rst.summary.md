This documentation file, `rados/operations/pools.rst`, serves as the definitive operational guide for managing **Pools** within a Ceph RADOS cluster. Pools are the primary logical partitions for data storage, and this file details their configuration, lifecycle management, and performance tuning.

### 1. Primary Purpose
The file documents the **management and configuration of Ceph Pools**. It provides administrators with the conceptual framework (resilience, placement groups, CRUSH rules) and the specific CLI commands required to create, modify, monitor, and delete pools.

### 2. Key Topics Covered
*   **Pool Fundamental Concepts**: Resilience (Replication vs. Erasure Coding), Placement Groups (PGs), and CRUSH placement rules.
*   **Lifecycle Management**: Creating, renaming, and deleting pools (including safety flags like `mon_allow_pool_delete`).
*   **Application Association**: Linking pools to specific Ceph services (RGW, RBD, CephFS).
*   **Data Protection & Scaling**: Configuring replica counts (`size`), minimum replica requirements (`min_size`), and PG autoscaling modes.
*   **Performance & Storage Tuning**: Inline compression settings for BlueStore, pool quotas, and I/O priorities.
*   **Advanced Features**: Snapshots, Stretch Pools (cross-datacenter peering), and Erasure Coding optimizations.
*   **Monitoring**: Command-line tools for checking pool statistics and utilization.

### 3. Technical Keywords
*   **Core Commands**: `ceph osd pool create`, `ceph osd lspools`, `ceph osd pool set/get`, `rados df`, `ceph osd pool mksnap`.
*   **Configuration Keys**: `pg_num`, `pgp_num`, `size`, `min_size`, `crush_rule`, `compression_algorithm`, `compression_mode`, `allow_ec_overwrites`.
*   **Flags**: `hashpspool`, `nodelete`, `nopgchange`, `nosizechange`, `bulk`, `noscrub`.
*   **Modes**: `replicated`, `erasure`, `autoscale-mode (on/off/warn)`.
*   **Internal IDs**: Pool IDs, Pool Names (reserved `.` prefix).

### 4. Target Audience
*   **Storage Administrators**: Responsible for cluster health, data durability, and resource allocation.
*   **System Architects**: Designing failure domains and performance profiles.
*   **DevOps Engineers**: Automating storage provisioning via CLI scripts.
*   **Support Engineers**: Troubleshooting data availability and PG distribution issues.

### 5. Related Concepts
*   **Placement Groups (PGs)**: The underlying distribution mechanism for pools.
*   **CRUSH Maps**: The algorithm used by pools to decide physical data placement on OSDs.
*   **BlueStore**: The backend storage engine that handles the pool-level compression settings.
*   **Erasure Coding (EC)**: A data protection method documented here as a pool type.
*   **Ceph Services (RBD/RGW/CephFS)**: The "applications" that consume RADOS pools.

### When to update this file (Trigger for AI)
This file should be updated if code changes occur in the following areas:
1.  **CLI Changes**: Any modifications to the `ceph osd pool` command subtree or new arguments added to pool creation.
2.  **New Pool Attributes**: Addition of new keys that can be tuned via `ceph osd pool set`.
3.  **BlueStore Innovations**: New compression algorithms or logic changes in how data is stored at the OSD level.
4.  **Scaling Logic**: Changes to the PG Autoscaler behavior or default PG calculations.
5.  **Safety Protocols**: Changes to how pools are deleted or protected (e.g., changes to `mon_allow_pool_delete` logic).
6.  **Feature Enhancements**: New data protection strategies beyond standard Replication and Erasure Coding.