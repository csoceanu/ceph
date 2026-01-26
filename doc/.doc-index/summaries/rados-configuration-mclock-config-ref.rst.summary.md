This documentation file serves as the definitive reference for **mClock-based Quality of Service (QoS)** within the Ceph RADOS layer. It explains how Ceph uses the dmClock algorithm to balance resource contention between client operations and internal background tasks.

### 1. Primary Purpose
The file documents the configuration and operational management of the **mClock scheduler**. Its main goal is to guide administrators on using **mClock Profiles**—abstracted configuration sets that simplify the complex tuning of IOPS, bandwidth, and priority across different OSD workloads.

### 2. Key Topics Covered
*   **mClock Client Classification**: Categorizes traffic into three buckets: *Client* (external I/O), *Background Recovery*, and *Background Best-Effort* (scrub, snap trim, etc.).
*   **Built-in Profiles**: Detailed breakdown of the three standard profiles:
    *   `balanced` (Default): Equal priority for clients and recovery.
    *   `high_client_ops`: Prioritizes application performance.
    *   `high_recovery_ops`: Prioritizes data restoration/rebalancing.
*   **Custom Profiles**: Guidance for advanced users to manually define QoS parameters.
*   **Hardware-Specific Tuning**: Instructions for HDD vs. SSD shard configurations and how they affect scheduling accuracy.
*   **Automated Capacity Determination**: How Ceph benchmarks OSDs at initialization to set IOPS limits.
*   **Configuration Locking**: Explanation of why certain Ceph parameters (like sleep settings) are disabled or "locked" when mClock is active to ensure predictable scheduling.

### 3. Technical Keywords
*   **Algorithms/APIs**: `dmClock`, `mClock`, `OSD Bench`.
*   **Profile Types**: `balanced`, `high_client_ops`, `high_recovery_ops`, `custom`.
*   **Core Parameters**: `reservation`, `weight`, `limit` (the "Three Pillars" of dmClock).
*   **Configuration Options**:
    *   `osd_mclock_profile`
    *   `osd_mclock_max_capacity_iops_[hdd|ssd]`
    *   `osd_op_num_shards_hdd` / `osd_op_num_threads_per_shard_hdd`
    *   `osd_mclock_override_recovery_settings`
*   **Commands**: `ceph config set`, `ceph tell osd.N bench`, `ceph daemon osd.N config set`, `injectargs`.

### 4. Target Audience
*   **Storage Administrators**: Seeking to optimize cluster performance or resolve contention between recovery and client I/O.
*   **System Architects**: Designing Ceph clusters with specific QoS requirements.
*   **Support Engineers**: Troubleshooting "slow requests" or inconsistent throughput in heterogeneous hardware environments.

### 5. Related Concepts
*   **dmClock Algorithm**: The underlying distributed queuing research.
*   **BlueStore**: The storage backend whose throttling parameters (`bluestore_throttle_bytes`) interact directly with mClock effectiveness.
*   **OSD Sharding**: The internal architecture of how OSDs process operations.
*   **Data Scrubbing & Recovery**: The specific background processes that mClock is designed to throttle.

---

### Triggering Documentation Updates (Code Change Scenarios)
This file should be updated if code changes occur in the following areas:
1.  **Algorithm Changes**: If the underlying dmClock implementation is modified or replaced.
2.  **Profile Rebalancing**: If the default `Reservation`, `Weight`, or `Limit` percentages for built-in profiles are adjusted in the source code.
3.  **New Client Types**: If a new class of OSD operation is added (e.g., a new background service) that requires its own mClock bucket.
4.  **Default Value Shifts**: If hardware thresholds (like `iops_capacity_threshold_ssd`) or default shard counts are updated based on new performance testing.
5.  **New Config Options**: Addition of new `osd_mclock_*` configuration keys or changes to the "locked" status of existing OSD recovery/sleep settings.