This documentation file serves as the definitive operational guide for managing **Pools** within a Ceph RADOS cluster. Pools are the primary logical partitions for object storage, governing data resilience, placement, and performance characteristics.

### 1. Primary Purpose
The file documents the lifecycle management, configuration, and monitoring of Ceph pools. It explains how to create, modify, secure, and delete pools, while defining the underlying mechanisms (CRUSH, PGs, Replication/Erasure Coding) that pools use to protect data.

### 2. Key Topics Covered
*   **Data Protection Strategies**: Detailed comparison between Replicated pools (mirrored copies) and Erasure-Coded pools (parity-based).
*   **Placement Groups (PGs)**: Role of the PG autoscaler, manual PG/PGP tuning, and the importance of target PG counts per OSD.
*   **Pool Operations**: Step-by-step CLI instructions for creating, listing, renaming, and deleting pools.
*   **Application Association**: Requirement for tagging pools with their intended consumer (CephFS, RBD, RGW).
*   **Quotas and Statistics**: Setting limits on object counts or byte sizes and monitoring utilization via `rados df`.
*   **Configuration Attributes**: In-depth reference for pool-specific settings including compression (BlueStore), scrubbing intervals, and recovery priorities.
*   **Stretch Pools**: Specialized configuration for high availability across distinct geographic locations or data centers.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph osd pool create`, `ceph osd lspools`, `ceph osd pool set`, `rados df`, `ceph osd pool mksnap`, `ceph osd pool application enable`.
*   **Configuration Options**: `osd_pool_default_pg_num`, `mon_allow_pool_delete`, `osd_pool_default_crush_rule`.
*   **Attributes/Keys**: `pg_num`, `pgp_num`, `size` (replicas), `min_size`, `crush_rule`, `compression_algorithm`, `allow_ec_overwrites`, `autoscale-mode`.
*   **Flags**: `hashpspool`, `nodelete`, `nopgchange`, `nosizechange`, `bulk`.

### 4. Target Audience
*   **Storage Administrators**: For day-to-day cluster maintenance and resource allocation.
*   **System Architects**: For designing resilience strategies and capacity planning.
*   **DevOps Engineers**: For automating storage provisioning and integrating with RBD/RGW.

### 5. Related Concepts
*   **CRUSH Maps**: Pools rely on CRUSH rules to define physical data placement.
*   **BlueStore**: Pool-level compression settings directly interface with the BlueStore backend.
*   **PG Autoscaler**: A critical subsystem that automates pool scaling.
*   **RADOS Gateway (RGW) / RBD / CephFS**: These higher-level storage interfaces utilize pools as their underlying storage layer.

---

### Update Triggers for AI Maintenance
This file should be updated if code changes occur in the following areas:
1.  **CLI Syntax Changes**: If `ceph-mgr` or `ceph-mon` modifies the arguments for `osd pool` commands.
2.  **New BlueStore Features**: If new compression algorithms or storage hint flags are added to the backend.
3.  **Default Value Shifts**: If the hardcoded defaults for `size`, `min_size`, or `pg_num` change in the source code.
4.  **Application Logic**: If new official application types are added beyond `rbd`, `rgw`, and `cephfs`.
5.  **Scaling Logic**: Changes to the PG Autoscaler behavior or the relationship between `pg_num` and `pgp_num`.
6.  **Safety Mechanisms**: Modifications to pool deletion protections or mandatory flags.