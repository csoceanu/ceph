This documentation file, `rados/operations/placement-groups.rst`, is the definitive guide for managing **Placement Groups (PGs)** in a Ceph cluster. It explains the architectural necessity of PGs for scaling and provides operational instructions for managing data distribution, durability, and health.

### 1. Primary Purpose
The file documents the lifecycle, management, and optimization of Placement Groups. It serves as both a conceptual overview (explaining why PGs exist) and an operational manual (detailing how to use the PG Autoscaler, manually tune PG counts, and troubleshoot stuck PGs).

### 2. Key Topics Covered
*   **The PG Autoscaler**: Detailed explanation of `on`, `warn`, and `off` modes; how it calculates recommendations based on pool usage and target sizes.
*   **Pool Parameters**: Configuration of `pg_num`, `pgp_num`, `target_size_bytes`, and `target_size_ratio`.
*   **Scaling Thresholds**: How the system decides when to trigger a split or merge of PGs.
*   **The "Bulk" Flag**: A special designation for large pools to ensure they start with an appropriate number of PGs to prevent performance bottlenecks.
*   **Data Durability & Recovery**: The mathematical relationship between OSD counts, PG counts, and the risk of data loss during hardware failure.
*   **Operational Commands**: Procedures for scrubbing PGs, prioritizing backfill/recovery, and handling "unfound" objects.
*   **PG Map/Status**: How to interpret `autoscale-status` and `pg dump` outputs.

### 3. Technical Keywords
*   **APIs/Properties**: `pg_autoscale_mode`, `pg_num`, `pgp_num`, `target_size_bytes`, `target_size_ratio`, `pg_num_min`, `pg_num_max`, `recovery_priority`, `bulk`.
*   **CLI Commands**: `ceph osd pool autoscale-status`, `ceph pg dump_stuck`, `ceph tell {pg-id} scrub`, `ceph pg force-recovery`, `ceph osd pool set {pool} bulk`.
*   **Configuration Options**: `osd_pool_default_pg_autoscale_mode`, `mon_target_pg_per_osd`, `mon_max_pg_per_osd`.
*   **Internal States**: `inactive`, `undersized`, `stale`, `degraded`, `remap`, `backfill`.

### 4. Target Audience
*   **Storage Administrators**: Who need to optimize cluster performance and balance data across OSDs.
*   **System Architects**: Designing Ceph clusters who need to calculate initial PG counts and durability risks.
*   **SREs/Troubleshooters**: Diagnosing performance issues or data availability problems (e.g., stuck PGs, unfound objects).

### 5. Related Concepts
*   **CRUSH Algorithm**: PGs are the input to CRUSH for determining physical OSD placement.
*   **RADOS Objects**: PGs are the logical containers/aggregators for individual objects.
*   **Ceph Pools**: The logical layer directly above PGs; PGs are subsets of pools.
*   **OSD (Object Storage Daemons)**: The physical layer where PG data is stored and replicated.

---

### Update Triggers (When to update this file)
This file should be updated if code changes occur in the following areas:
1.  **Autoscaler Logic**: If the mathematical formula for calculating `NEW PG_NUM` changes or if new calculation variables (like new ratios or biases) are added to the Manager module.
2.  **PG States**: If new PG states are introduced or if the definition of "stuck" PGs is modified in the Monitor.
3.  **Default Values**: If hardcoded defaults for `mon_target_pg_per_osd` or the default scaling threshold (currently 3.0) are changed.
4.  **CLI Changes**: If the output format of `autoscale-status` changes (e.g., adding a new column) or if new `ceph pg` subcommands are added.
5.  **BlueStore/OSD Backend**: If changes to the OSD backend alter the recommended number of PGs per OSD (as seen when Filestore was deprecated).
6.  **New Pool Flags**: If new pool-level flags (similar to `bulk`) are introduced that affect PG behavior or initial placement.