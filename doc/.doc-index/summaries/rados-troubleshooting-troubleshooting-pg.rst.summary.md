### Documentation Summary: `rados/troubleshooting/troubleshooting-pg.rst`

#### 1. Primary Purpose
This document serves as a comprehensive troubleshooting guide for **Placement Groups (PGs)** in a Ceph cluster. It provides diagnostic steps and remediation strategies for PGs that fail to reach an `active+clean` state, become stuck, experience peering failures, or contain inconsistent/unfound data.

#### 2. Key Topics Covered
*   **PG States & Health**: Diagnosing PGs that remain in `degraded`, `remapped`, or `active` but not `clean`.
*   **Topology-Specific Issues**: Handling "One Node Clusters" and clusters where the number of OSDs is fewer than the replica count.
*   **Stuck PGs**: Definitions and diagnostic commands for `inactive`, `unclean`, and `stale` states.
*   **Peering Failures**: Using `pg query` to identify why PGs are blocked (e.g., down OSDs).
*   **Data Integrity Problems**:
    *   **Unfound Objects**: Scenarios where the cluster knows an object exists but cannot locate a copy.
    *   **Inconsistencies**: Analyzing scrub errors and performing repairs.
*   **Resource Distribution**: Handling "Homeless PGs" (all OSDs for a PG are down) and uneven data distribution.
*   **Erasure Coding (EC) Troubleshooting**: Addressing mapping failures specific to EC pools, CRUSH constraints, and tuning `choose_tries`.

#### 3. Technical Keywords
*   **CLI Commands**: `ceph pg dump_stuck`, `ceph health detail`, `ceph pg <id> query`, `ceph osd lost`, `ceph pg mark_unfound_lost`, `ceph pg repair`, `rados list-inconsistent-pg`, `crushtool`.
*   **Configuration Options**: `osd_crush_chooseleaf_type`, `osd_pool_default_size`, `osd_pool_default_min_size`, `osd_scrub_auto_repair`, `osd_scrub_auto_repair_num_errors`.
*   **PG States**: `active`, `clean`, `remapped`, `degraded`, `stale`, `inactive`, `unclean`, `peering`, `down`, `inconsistent`.
*   **CRUSH Parameters**: `step set_choose_tries`, `step set_chooseleaf_tries`, `choose_total_tries`, `ITEM_NONE (2147483647)`.

#### 4. Target Audience
*   **Storage Administrators**: Responsible for daily cluster health and incident response.
*   **Site Reliability Engineers (SREs)**: Automating recovery and monitoring PG states.
*   **Developers/Testers**: Setting up experimental single-node clusters or debugging data path issues.

#### 5. Related Concepts
*   **RADOS**: The underlying object store where PGs reside.
*   **CRUSH Algorithm**: The placement logic that PGs rely on for mapping to OSDs.
*   **Scrubbing**: The process that identifies the inconsistencies discussed in this file.
*   **Peering & Recovery**: The state machine logic OSDs use to agree on the state of data.

---

### Update Triggers (For AI Systems)
This file should be updated if code changes occur in the following areas:

1.  **PG State Machine**: If new PG states are introduced (e.g., beyond `stale`, `peering`, etc.) or if the definitions of "stuck" PGs change in the Monitor code.
2.  **CLI Output/Syntax**: If the JSON structure of `ceph pg query` or `rados list-inconsistent-obj` is modified, as the document contains hardcoded examples of these outputs.
3.  **Repair Logic**: If the behavior of `ceph pg repair` changes (e.g., how it selects an authoritative copy) or if new repair strategies (like BlueStore-specific repairs) are added.
4.  **CRUSH Tunables**: If default CRUSH values (like `choose_total_tries`) are altered or if new `step` types are added to the CRUSH map syntax.
5.  **Default Safety Settings**: If changes are made to the default pool sizes or the minimum required OSDs to satisfy a write (related to `osd_pool_default_min_size`).
6.  **EC Profiles**: If the way erasure code profiles interact with CRUSH rules changes.