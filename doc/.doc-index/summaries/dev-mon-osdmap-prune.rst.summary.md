This documentation file, `dev/mon-osdmap-prune.rst`, provides a technical specification and algorithmic overview of the **Full OSDMap Version Pruning** mechanism within the Ceph Monitor.

### 1. Primary Purpose
The file documents the strategy for managing disk space and key/value store pressure by selectively deleting ("pruning") historical full OSDMap epochs. It explains how the monitor maintains a balance between serving client requests quickly (which requires full maps) and preventing unbounded storage growth when OSDMap trimming is blocked (e.g., due to unclean PGs).

### 2. Key Topics Covered
*   **The Problem**: Unbounded growth of full OSDMaps in the monitor store when the standard trimming mechanism (`OSDMonitor::get_trim_to()`) is stalled.
*   **The Pruning Strategy**: Instead of deleting all maps or rebuilding them on-demand, the monitor "pokes holes" in the history, keeping only a subset (default ~10%) of full maps.
*   **The Pruning Algorithm**: A detailed walkthrough of how the monitor decides when to prune, how it "pins" specific versions to keep, and how it executes deletions in batches (transactions).
*   **The Manifest (`osdmap_manifest_t`)**: A data structure used to track which maps are pinned/available versus which have been pruned.
*   **Integration with Trimming**: How the pruning manifest is updated or deleted when the standard monitor trim process catches up to pruned intervals.
*   **Safety & Invariants**: Strict conditions required for quorum compatibility and data integrity during crashes.

### 3. Technical Keywords
*   **Configuration Options**: 
    * `mon_min_osdmap_epochs`: Minimum epochs to keep before pruning starts (default 500).
    * `mon_osdmap_full_prune_min`: Threshold of epochs required to trigger the pruning algorithm (default 10,000).
    * `mon_osdmap_full_prune_interval`: The gap between kept (pinned) maps (default 10).
    * `mon_osdmap_full_prune_txsize`: Maximum maps deleted per transaction.
*   **APIs/Classes**: `OSDMonitor::get_trim_to()`, `osdmap_manifest_t`, `first_committed`, `last_committed`.
*   **Data Structures**: `std::set<version_t> pinned`.

### 4. Target Audience
*   **Ceph Core Developers**: Specifically those working on the `OSDMonitor` or Paxos service.
*   **Storage Engineers**: Debugging monitor store size issues or performance degradation in the KV store (RocksDB).
*   **System Architects**: Understanding the trade-offs between CPU/IO (rebuilding maps) and Disk Space (storing full maps).

### 5. Related Concepts
*   **OSDMap Trimming**: The standard process of deleting old incrementals and full maps.
*   **Paxos**: The consensus mechanism ensuring all monitors agree on the pruned state.
*   **RocksDB/Monitor Store**: The underlying storage where OSDMaps and the manifest are persisted.
*   **Placement Groups (PGs)**: The health of PGs (e.g., "unclean" state) is a primary reason why standard trimming might stop, necessitating this pruning feature.

### 6. When to Update This File
This documentation should be updated if code changes occur in the following areas:
*   **Algorithm Logic**: Changes to how `prune_to` is calculated or how the "pinned" list is populated.
*   **Config Changes**: If default values for pruning thresholds or intervals are modified.
*   **Data Structure Updates**: If `osdmap_manifest_t` is expanded to include more metadata (e.g., timestamps or checksums).
*   **Compatibility/Quorum**: If the requirements for monitors to support pruning before joining a quorum change.
*   **Trimming Logic**: If the interaction between `OSDMonitor::get_trim_to()` and the pruning manifest is refactored.