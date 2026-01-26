This documentation file, `rados/operations/crush-map.rst`, is the definitive guide to Ceph’s **CRUSH (Controlled Replication Under Scalable Hashing)** algorithm and its operational configuration. It explains how Ceph determines data placement without a centralized metadata server, focusing on scalability and fault tolerance.

### 1. Primary Purpose
The file documents the **CRUSH Map**, the internal data structure used to map data to physical devices. It provides instructions for managing the cluster hierarchy, defining data placement rules, and tuning the algorithm to ensure high availability and performance across various failure domains.

### 2. Key Topics Covered
*   **CRUSH Location & Hierarchy**: How OSDs are organized into logical buckets (hosts, racks, rows, roots) to mirror physical topology.
*   **Device Classes**: Categorizing storage by hardware type (HDD, SSD, NVMe) to target specific media via placement rules.
*   **CRUSH Rules**: Defining policies for data replication (replicated pools) and data striping (erasure-coded pools).
*   **Weight Sets**: Managing data balance through `compat` and `per-pool` weight optimizations to correct distribution skews.
*   **Operational Management**: Commands for adding, moving, reweighting, and removing OSDs or buckets within the map.
*   **Tunables**: Historical and version-specific algorithm adjustments (e.g., `straw2`, `chooseleaf_stable`) to improve mapping stability.
*   **Primary Affinity**: Influencing which OSD in a set acts as the "Primary" for read/write operations to optimize performance.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph osd crush set`, `ceph osd tree`, `ceph osd crush rule create-replicated`, `ceph osd primary-affinity`, `ceph osd crush weight-set`.
*   **Configuration Options**: `crush_location`, `crush_location_hook`, `osd_crush_update_on_start`, `mon_warn_on_legacy_crush_tunables`.
*   **Data Structures**: `Buckets`, `Roots`, `Straw2` (algorithm), `MSR Rules` (Multi-Step Retry).
*   **Concepts**: Failure Domains, Shadow Hierarchies, Device Classes, Weights, Tunables, Primary Affinity.

### 4. Target Audience
*   **Storage Administrators**: Responsible for designing cluster topology and ensuring data redundancy.
*   **System Architects**: Designing failure domains (e.g., rack-aware or DC-aware clusters).
*   **Site Reliability Engineers (SREs)**: Troubleshooting data imbalance or performance bottlenecks related to OSD utilization.

### 5. Related Concepts
*   **Peering and Recovery**: CRUSH determines where data *should* be; peering ensures it *is* there.
*   **The Balancer Module (`ceph-mgr`)**: Automatically manages the weight sets discussed in this file.
*   **Erasure Coding**: Profiles for EC pools rely heavily on the CRUSH rules generated here.
*   **RADOS Pools**: The high-level logical storage units that apply the CRUSH rules documented here.

---

### Triggering Updates: Guidelines for Developers
This file should be updated if code changes involve:
*   **New CRUSH Algorithms**: Adding a new bucket type (like the evolution from `straw` to `straw2`).
*   **New Feature Bits**: Introduction of new tunables or feature flags (e.g., the `CRUSH_MSR` bit mentioned for the Squid release).
*   **CLI Changes**: Any modifications to `ceph osd crush ...` command syntax or new subcommands.
*   **Default Behavior Changes**: If the default failure domain, default bucket types, or the logic for `crush_location` auto-detection changes.
*   **Hardware Classification**: Updates to how OSDs auto-detect device classes (HDD/SSD/NVMe).
*   **Scalability Limits**: If new hierarchy depths or types are supported or modified.