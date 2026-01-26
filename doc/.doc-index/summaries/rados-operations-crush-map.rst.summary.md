This documentation file, `rados/operations/crush-map.rst`, serves as the definitive guide to Ceph's **CRUSH (Controlled Replication Under Scalable Hashing)** algorithm and its operational configuration. It explains how Ceph determines data placement without a centralized lookup table, ensuring scalability and fault tolerance.

### 1. Primary Purpose
The file documents the **CRUSH Map**, the internal data structure used to describe the cluster's physical topology and data placement policies. It provides instructions on how to define failure domains, manage the storage hierarchy, and tune the algorithm to ensure data durability and cluster performance.

### 2. Key Topics Covered
*   **CRUSH Hierarchy & Buckets**: Definition of nodes (roots, regions, datacenters, racks, hosts) and leaves (OSDs).
*   **Data Placement Rules**: How to create and manage rules for replicated and erasure-coded pools.
*   **CRUSH Location Management**: How OSDs identify their place in the hierarchy (including custom location hooks).
*   **Device Classes**: Segmenting storage by hardware type (HDD, SSD, NVMe) using shadow hierarchies.
*   **Weights and Balancing**: Differences between standard CRUSH weights, *compat* weight sets, and *per-pool* weight sets.
*   **Tunables**: Historical profiles (Argonaut to Jewel/Optimal) that adjust the algorithm's mathematical behavior to fix known mapping bugs.
*   **Primary OSD Selection**: Techniques like "Primary Affinity" and custom rules to force specific hardware (e.g., SSDs) to handle primary read/write traffic.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph osd tree`, `ceph osd crush set`, `ceph osd crush rule create-replicated`, `ceph osd crush weight-set`, `ceph osd primary-affinity`.
*   **Configuration Options**: `crush_location`, `crush_location_hook`, `osd_crush_update_on_start`, `mon_warn_on_legacy_crush_tunables`.
*   **Data Structures**: `Buckets`, `Straw/Straw2` (bucket algorithms), `MSR Rules` (Multi-Step Retry).
*   **Tunables**: `chooseleaf_stable`, `choose_total_tries`, `chooseleaf_vary_r`, `straw_calc_version`.

### 4. Target Audience
*   **Cloud Architects**: Designing failure domains (rack-aware vs. host-aware) for high availability.
*   **Storage Administrators**: Performing day-to-day operations like adding/removing OSDs or rebalancing data.
*   **Performance Engineers**: Tuning primary affinity or device classes to optimize IOPS and throughput.
*   **SREs/DevOps**: Automating OSD deployment using custom location hooks and configuration files.

### 5. Related Concepts
*   **Peering & Backfilling**: Changing CRUSH rules or weights triggers data movement (backfill).
*   **Erasure Coding (EC)**: CRUSH rules for EC are typically managed via **Erasure Code Profiles**.
*   **Ceph Manager (mgr) Balancer**: Automated weight adjustment often supersedes manual CRUSH weight-set management.
*   **RADOS Pools**: Every pool is mapped to exactly one CRUSH rule.

### When to update this file (for AI/Maintainers)
This documentation should be updated if code changes occur in the following areas:
1.  **New CRUSH Features**: If a new release introduces a feature bit (like `CRUSH_MSR` in the Squid release) or a new bucket algorithm.
2.  **Topology Defaults**: If the default hierarchy types (e.g., adding `cloud-region` or `power-zone`) are modified in the source code.
3.  **CLI Syntax**: If the `ceph osd crush` command subtree adds new arguments or changes existing parameter requirements.
4.  **Tunable Profiles**: If a new "Optimal" profile is defined for a major Ceph release.
5.  **Default Behaviors**: If the way OSDs auto-detect their `crush_location` or `device_class` changes.