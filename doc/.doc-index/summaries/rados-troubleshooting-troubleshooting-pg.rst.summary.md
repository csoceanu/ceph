This documentation file, `rados/troubleshooting/troubleshooting-pg.rst`, serves as the primary diagnostic and remediation guide for **Placement Group (PG)** health within a Ceph cluster. It addresses scenarios where data distribution, redundancy, or recovery processes fail.

### 1. Primary Purpose
The file provides actionable troubleshooting steps for resolving PG-related health warnings and errors. It explains why PGs may become stuck in non-optimal states (e.g., degraded, peering, stale, or inconsistent) and provides the specific CLI commands needed to identify and fix the underlying infrastructure or configuration issues.

### 2. Key Topics Covered
*   **Non-Clean PGs**: Causes for PGs failing to reach `active+clean`, including configuration issues in small clusters (one-node or low OSD counts).
*   **Stuck PG Definitions**: Specific explanations for `inactive`, `unclean`, and `stale` states.
*   **Peering Failures**: How to diagnose PGs stuck in `down+peering` using `pg query` and how to handle lost OSDs.
*   **Unfound Objects**: Scenarios where the cluster knows data exists but cannot locate any live replicas, and how to use `mark_unfound_lost`.
*   **Data Distribution Issues**: Troubleshooting why only a few OSDs receive data or why writes are blocked (min_size issues).
*   **Inconsistency and Repair**: Identifying scrub errors, analyzing JSON output from `list-inconsistent-obj`, and using the `pg repair` command.
*   **Erasure Coding (EC) Specifics**: Troubleshooting EC PGs that fail to map due to CRUSH constraints or insufficient OSDs.
*   **Advanced CRUSH Tuning**: Using `crushtool` to simulate mappings and adjusting `set_choose_tries`.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph pg dump_stuck`, `ceph pg query`, `ceph osd lost`, `ceph pg list_unfound`, `ceph pg mark_unfound_lost`, `ceph pg repair`, `rados list-inconsistent-pg`, `crushtool`.
*   **Configuration Options**: `osd_crush_chooseleaf_type`, `osd_pool_default_size`, `osd_pool_default_min_size`, `osd_scrub_auto_repair`, `set_choose_tries`.
*   **PG States**: `active+clean`, `active+degraded`, `active+remapped`, `peering`, `stale`, `inconsistent`, `unfound`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for maintaining cluster health and resolving `HEALTH_WARN` or `HEALTH_ERR` states.
*   **DevOps/SREs**: Implementing automation or monitoring for Ceph and needing to understand failure modes.
*   **Developers**: Testing Ceph in unconventional environments (like single-node setups).

### 5. Related Concepts
*   **CRUSH Algorithm**: The underlying logic that determines data placement.
*   **Scrubbing**: The background process that checks data integrity.
*   **Peering**: The process by which OSDs agree on the state of data in a PG.
*   **Erasure Coding**: An alternative to replication for data redundancy.

---

### Update Trigger Analysis for AI
This file should be updated if code changes occur in the following areas:

*   **PG State Machine Changes**: If new PG states are introduced or if the definitions of "stuck" (stale, inactive, unclean) are modified in the Monitor/OSD code.
*   **CLI Command Output/Syntax**: If the JSON structure of `ceph pg query` or `rados list-inconsistent-obj` changes, or if new arguments are added to `pg repair`.
*   **BlueStore/Filestore Evolution**: As the documentation notes differences in how these backends handle checksums and repairs, any shift in the "authoritative copy" selection logic requires an update.
*   **CRUSH Default Changes**: If the default `chooseleaf` type or default pool sizes/min_sizes are altered in the source code.
*   **New Repair Capabilities**: If `pg repair` is enhanced to handle cases it currently cannot (e.g., automated EC repair logic changes).