This documentation provides a technical overview and administrative guide for **Cache Tiering** within a Ceph cluster. It outlines the mechanism for using high-performance storage pools (hot tiers) to transparently buffer I/O for slower, more economical backing pools (cold tiers).

### 1. Primary Purpose
The file serves as the definitive guide for implementing, configuring, and decommissioning cache tiers. Crucially, it also acts as a **deprecation notice**, advising against new deployments and warning that the feature lacks a maintainer as of the **Reef release**.

### 2. Key Topics Covered
*   **Architectural Overview**: Explanation of the "Objecter" and "Tiering Agent" and how they maintain transparency for Ceph clients.
*   **Cache Modes**: 
    *   `writeback`: High-performance mode for mutable data.
    *   `readproxy`: Used for draining caches or transitioning modes.
    *   `readonly`: Experimental mode for immutable workloads.
*   **Operational Suitability**: Critical analysis of "Good Workloads" (RGW time-skewed) vs. "Bad Workloads" (RBD with Erasure Coding).
*   **Pool Setup**: Instructions on creating backing pools (replicated or erasure-coded) and high-speed cache pools using CRUSH rules.
*   **Management Lifecycle**: Commands for adding tiers, setting overlays, and the specific multi-step safety procedures for removing writeback caches.
*   **Tuning and Sizing**: Detailed logic for flushing (dirty to cold) and evicting (clean to out) based on HitSets, target bytes, and ratios.
*   **Troubleshooting**: Handling "unfound objects" related to HitSet archives during upgrades.

### 3. Technical Keywords
*   **APIs/Commands**: `ceph osd tier add/remove`, `set-overlay`, `cache-mode`, `rados cache-flush-evict-all`.
*   **Configuration Options**: `target_max_bytes`, `target_max_objects`, `cache_target_dirty_ratio`, `cache_target_full_ratio`, `cache_min_flush_age`, `hit_set_type` (Bloom Filter).
*   **Internal Components**: Objecter, Tiering Agent, HitSet, CRUSH Device Class.
*   **Logic Gates**: `min_read_recency_for_promote`, `min_write_recency_for_promote`.

### 4. Target Audience
*   **Storage Administrators**: Those managing legacy Ceph clusters or planning migrations away from cache tiering.
*   **System Architects**: Evaluating whether a specific workload (e.g., RGW vs. RBD) justifies the complexity and performance overhead of tiering.
*   **Site Reliability Engineers (SREs)**: Troubleshooting performance degradation or data consistency issues (unfound objects) in tiered environments.

### 5. Related Concepts
*   **CRUSH Maps**: Essential for ensuring cache pools reside on physically faster hardware (SSDs/NVMe).
*   **Erasure Coding (EC)**: Often used as the "cold" tier; the documentation highlights specific performance friction between EC and replicated cache tiers.
*   **Librados**: Specifically mentions limitations regarding object enumeration when tiering is active.
*   **Pool Management**: General pool creation and property setting (`ceph osd pool set`).

---

### AI Update Triggers
This file should be updated if code changes occur in the following areas:
*   **Deprecation Status**: If the feature is officially removed in a release following Reef.
*   **Objecter Logic**: Any change to how the Objecter handles primary/overlay pool routing.
*   **OSD/Tiering Agent**: Changes to the flushing/eviction algorithms or the handling of `hit_set` archives (especially regarding the "unfound objects" bug).
*   **Default Values**: Changes to default ratios or recency requirements for promotion.
*   **CLI Syntax**: Updates to `ceph osd tier` or `rados` command arguments.