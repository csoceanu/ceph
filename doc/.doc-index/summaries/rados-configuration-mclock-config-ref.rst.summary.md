### 1. Primary Purpose
This documentation serves as the definitive reference for configuring **mClock-based Quality of Service (QoS)** in Ceph OSDs. It explains how Ceph uses the dmClock algorithm to prioritize different types of I/O traffic (client vs. background) and provides instructions on using **mClock Profiles** to simplify complex scheduling parameters.

### 2. Key Topics Covered
*   **mClock Client Classification**: Categorizes traffic into *Client* (external), *Background Recovery*, and *Background Best-effort* (backfill, scrub, etc.).
*   **Built-in Profiles**: Detailed breakdown of the three standard profiles:
    *   `balanced` (Default): Equal priority for clients and recovery.
    *   `high_client_ops`: Prioritizes user I/O over background tasks.
    *   `high_recovery_ops`: Prioritizes data restoration/rebalancing.
*   **Custom Profiles**: Guidance for advanced users to manually define QoS parameters.
*   **OSD Shard Configuration**: Specific performance tuning for HDD-based clusters (single shard/multiple threads optimization).
*   **Automated Capacity Determination**: How Ceph benchmarks OSD IOPS capacity at initialization.
*   **Configuration Locking**: Identification of parameters that are "locked" or overridden when using built-in profiles to ensure QoS predictability.

### 3. Technical Keywords
*   **Core Concepts**: `dmClock algorithm`, `QoS (Quality of Service)`, `Reservation`, `Weight`, `Limit`.
*   **Configuration Options**: 
    *   `osd_mclock_profile`
    *   `osd_mclock_max_capacity_iops_[hdd|ssd]`
    *   `osd_mclock_override_recovery_settings`
    *   `osd_op_num_shards_hdd` / `osd_op_num_threads_per_shard_hdd`
*   **Commands**: 
    *   `ceph config set`
    *   `ceph tell osd.N bench`
    *   `ceph daemon osd.N config set`
    *   `ceph tell osd.N injectargs`
*   **Client Types**: `background_recovery`, `background_best_effort`, `client`.

### 4. Target Audience
*   **Storage Administrators**: Seeking to tune cluster performance or resolve "slow request" issues during recoveries.
*   **Performance Engineers**: Benchmarking OSD hardware and optimizing throughput.
*   **Developers**: Understanding the interaction between the mClock scheduler and the BlueStore/OSD sharding layers.

### 5. Related Concepts
*   **dmClock/mClock QoS**: The underlying mathematical algorithm for distributed resource scheduling.
*   **BlueStore**: The storage backend whose throttling parameters (`bluestore_throttle_bytes`) directly impact mClock efficiency.
*   **OSD Benchmarking**: The internal process of measuring hardware capabilities to feed the scheduler.
*   **Recovery and Backfill**: The specific Ceph data movement processes regulated by these QoS settings.

### 6. Maintenance Trigger: When to update this file
This file must be updated if any of the following code-level changes occur:
*   **Default Logic Changes**: If the default values for `balanced`, `high_client_ops`, or `high_recovery_ops` (reservation/weight/limit ratios) are modified in the source.
*   **New Profiles**: If a new built-in profile is added to the mClock code.
*   **Shard Logic**: If the threading or sharding model for OSDs (especially HDDs) is refactored.
*   **Command Line Changes**: If the `ceph bench` syntax or `config` subsystem interfaces are altered.
*   **Parameter Exposure**: If new `osd_mclock_*` configuration options are introduced or existing ones are deprecated/renamed.
*   **Threshold Adjustments**: If the automated capacity detection logic or its safety thresholds (e.g., `osd_mclock_iops_capacity_threshold_ssd`) are tuned.