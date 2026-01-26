### 1. Primary Purpose
This documentation serves as the authoritative guide for **Placement Groups (PGs)** in a Ceph cluster. It explains the architectural role of PGs as an abstraction layer between logical pools and physical OSDs, provides instructions for managing PG counts via manual configuration or the automated **PG Autoscaler**, and outlines operational procedures for troubleshooting, scrubbing, and prioritizing data recovery.

---

### 2. Key Topics Covered
*   **PG Architecture**: Mapping of RADOS objects to PGs and PGs to OSDs; the purpose of PGs in scaling and metadata management.
*   **PG Autoscaler**: Operational modes (`on`, `off`, `warn`), viewing recommendations, and understanding the scaling threshold.
*   **Capacity Planning**: Setting `target_size_bytes` or `target_size_ratio` to help the cluster pre-calculate ideal PG counts.
*   **Bulk Pools**: The `bulk` flag for large pools to ensure high initial PG counts for performance.
*   **Placement Tuning**: Use of `pg_num_min`, `pg_num_max`, and `mon_target_pg_per_osd`.
*   **Data Durability & Recovery**: Impact of PG counts on recovery speed and data safety; forcing/prioritizing backfill and recovery.
*   **Maintenance & Integrity**: Manual scrubbing (shallow and deep), checking for "stuck" PGs, and handling "unfound" lost objects.

---

### 3. Technical Keywords
*   **Configuration Options**: `pg_autoscale_mode`, `osd_pool_default_pg_autoscale_mode`, `mon_target_pg_per_osd`, `pg_num_min`, `pg_num_max`, `target_size_bytes`, `target_size_ratio`, `recovery_priority`.
*   **Commands**: `ceph osd pool autoscale-status`, `ceph pg dump`, `ceph pg map`, `ceph pg scrub`, `ceph pg force-recovery`, `ceph pg mark_unfound_lost`.
*   **Concepts**: `pg_num` vs `pgp_num`, CRUSH roots/subtrees, `bulk` flag, `bias`, `effective ratio`, `backfill`, `peering`.
*   **PG States**: `inactive`, `undersized`, `stale`, `unclean`, `degraded`.

---

### 4. Target Audience
*   **Storage Administrators**: Responsible for cluster health, performance tuning, and capacity planning.
*   **SREs/DevOps**: Automating cluster deployments and managing resource utilization.
*   **Support Engineers**: Troubleshooting data unavailability, stuck PGs, or performance degradation during recovery.

---

### 5. Related Concepts
*   **CRUSH Maps/Rules**: PGs rely on CRUSH to determine physical placement; issues arise if pools span multiple CRUSH roots.
*   **RADOS**: The underlying object store that PGs organize.
*   **Ceph Manager (MGR)**: The module responsible for running the PG Autoscaler.
*   **BlueStore/FileStore**: Underlying OSD storage backends that influence resource consumption per PG.
*   **Backfill/Recovery**: The internal processes triggered when `pg_num` or cluster topology changes.

---

### Maintenance Triggers: When to Update this File
This file must be updated if code changes occur in the following areas:
1.  **Autoscaler Logic**: Changes to the scaling threshold formulas (currently power-of-two based), the dynamic reduction of thresholds for scaling up, or how `BIAS` is calculated.
2.  **CLI Changes**: Addition, removal, or renaming of `ceph osd pool` or `ceph pg` subcommands.
3.  **Default Values**: Updates to `mon_target_pg_per_osd` or the default `pg_autoscale_mode`.
4.  **Health Checks**: Addition of new health codes related to PG overcommitment or target size conflicts (e.g., `POOL_TARGET_SIZE_BYTES_OVERCOMMITTED`).
5.  **New Flags**: Introduction of new pool-level flags similar to `bulk` that influence PG distribution or lifecycle.
6.  **Architecture Shifts**: Any change in how objects map to PGs or how PGs map to OSDs (e.g., shifting away from the power-of-two requirement).