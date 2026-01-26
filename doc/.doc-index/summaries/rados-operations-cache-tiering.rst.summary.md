This documentation provides a technical overview and administrative guide for **Cache Tiering** within a Ceph cluster.

### 1. Primary Purpose
The file documents the implementation, configuration, and management of **Cache Tiering** in RADOS. It explains how to overlay a "hot" pool (fast storage like SSDs) over a "cold" backing pool (slower/cheaper storage like HDDs or Erasure Coded pools) to improve I/O performance transparently for clients.

**Crucial Status Note**: As of the **Reef release**, cache tiering is **deprecated**. The documentation strongly advises against new deployments due to a lack of maintainers and potential performance degradation for most modern workloads.

### 2. Key Topics Covered
*   **Operational Theory**: How the Ceph Objecter and tiering agent handle data promotion (cold to hot) and flushing/eviction (hot to cold).
*   **Cache Modes**:
    *   `writeback`: High-performance mode for mutable data.
    *   `readproxy`: Transitionary mode used for draining caches.
    *   `readonly`: Experimental mode for read-only workloads.
    *   `none`: Disables tiering.
*   **Workload Suitability**: Analysis of "Good" (RGW time-skewed) vs. "Bad" (RBD with Erasure Coding) use cases.
*   **Setup & Configuration**: Step-by-step commands to create pools, add tiers, and set overlays.
*   **Tiering Agent Management**: Detailed tuning of Bloom Filters (HitSets), sizing limits (absolute and relative), and object age-based flushing.
*   **Removal Procedures**: Safety protocols for removing read-only vs. writeback tiers without data loss.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph osd tier add`, `ceph osd tier set-overlay`, `ceph osd tier cache-mode`, `rados -p {pool} cache-flush-evict-all`.
*   **Configuration Keys**: `target_max_bytes`, `target_max_objects`, `cache_target_dirty_ratio`, `cache_target_full_ratio`, `cache_min_flush_age`, `hit_set_type`, `min_read_recency_for_promote`.
*   **Components**: Ceph Objecter, Tiering Agent, Bloom Filter, HitSets, CRUSH rules.

### 4. Target Audience
*   **Storage Administrators**: Responsible for cluster performance tuning and hardware lifecycle management.
*   **System Architects**: Evaluating whether cache tiering is appropriate for specific application workloads.
*   **Maintenance Engineers**: Handling the decommissioning or migration of legacy Ceph clusters.

### 5. Related Concepts
*   **CRUSH Maps & Device Classes**: Required to ensure the cache pool resides on high-performance hardware.
*   **Erasure Coding (EC)**: Often used as the "base tier" in a cache configuration to save space.
*   **Librados**: Specifically mentioned regarding object enumeration limitations in tiered environments.
*   **Pool Management**: General pool creation and attribute setting.

---

### AI Update Triggers (When to update this file)
An AI system should flag this documentation for updates if code changes occur in the following areas:
1.  **Deprecation Status**: If the feature moves from "deprecated" to "removed" in a specific release.
2.  **Objecter Logic**: Changes to how `librados` or the Objecter handles transparent routing between pools.
3.  **New Storage Modes**: If a new `cache-mode` is added or the experimental status of `readonly` changes.
4.  **Default Parameter Changes**: If default values for `hit_set` or `dirty_ratio` are modified in the OSD source code.
5.  **Erasure Coding Updates**: If EC pools gain native support for small writes, the "Known Bad Workloads" section may become obsolete.
6.  **Internal Namespace Changes**: If the `.ceph-internal` naming convention for HitSet archives changes (relevant to the Troubleshooting section).