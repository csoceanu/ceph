This documentation file, `mclock_wpq_cmp_study.rst`, serves as a technical report and benchmark analysis comparing two I/O schedulers in Ceph: the legacy **Weighted Priority Queue (WPQ)** and the refined **mClock (dmClock)** scheduler.

### 1. Primary Purpose
The file documents a performance study conducted to validate the effectiveness of the mClock scheduler refinements (introduced around the Ceph *Quincy* release). It aims to prove that mClock can successfully provide Quality of Service (QoS) guarantees—specifically prioritizing client I/O during background operations like recovery—where the WPQ scheduler fails under high concurrency.

### 2. Key Topics Covered
*   **mClock Algorithm Fundamentals**: Explanation of *Reservation*, *Weight*, and *Limit* controls.
*   **Scheduler Comparison**: WPQ vs. mClock under synthetic recovery stress tests.
*   **mClock Profiles**: Detailed breakdown of built-in profiles: `high_client_ops`, `balanced`, and `high_recovery_ops`.
*   **Baseline Benchmarking**: Methodology for establishing OSD capacity on NVMe SSDs and HDDs.
*   **Performance Metrics**: Analysis of Average IOPS, Percentile Latency (95th, 99th, 99.5th), and Recovery Throughput (objects/sec).
*   **Hardware Impact**: Comparison of results across different storage media (NVMe vs. HDD) and BlueStore configurations (with/without WAL/DB).

### 3. Technical Keywords
*   **Schedulers**: `mclock_scheduler`, `wpq` (Weighted Priority Queue).
*   **APIs/Algorithms**: `dmClock`, `cbt` (Ceph Benchmarking Tool).
*   **Configuration Options**:
    *   `osd_op_queue`
    *   `osd_mclock_profile`
    *   `osd_mclock_max_capacity_iops_hdd` / `_ssd`
    *   `osd_max_backfills`, `osd_recovery_max_active`
    *   `bluestore_throttle_bytes`
*   **Service Types**: `client`, `background recovery`, `background best effort`.
*   **QoS Parameters**: `reservation`, `weight`, `limit`.

### 4. Target Audience
*   **Ceph Developers**: Looking for historical context on why mClock was refined and how it was validated.
*   **Storage Administrators**: Seeking to understand which mClock profile to choose for specific cluster workloads.
*   **Performance Engineers**: Interested in the methodology for benchmarking Ceph I/O scheduling and recovery impact.

### 5. Related Concepts
*   **RADOS/OSD Internals**: Specifically how OSDs handle internal vs. external operation queues.
*   **BlueStore**: The underlying storage engine and its throttling mechanisms.
*   **Data Recovery & Backfill**: The specific background processes that compete with client I/O.
*   **dmClock Library**: The external C++ implementation of the mClock algorithm.

---

### Update Triggers for AI Maintenance
This file should be updated if:
1.  **Profile Defaults Change**: If the internal percentages for `high_client_ops`, `balanced`, or `high_recovery_ops` are modified in the Ceph source code.
2.  **New Profiles Added**: If a new built-in profile (e.g., `high_scrub_ops`) is introduced.
3.  **Default Scheduler Change**: If mClock replaces WPQ as the absolute default for all OSD types in a new release.
4.  **Algorithm Refinements**: If the `dmClock` implementation is updated to include new parameters beyond Reservation/Weight/Limit.
5.  **Benchmark Regression**: If new hardware architectures (like ZNS SSDs or Persistent Memory) render the existing HDD/NVMe study obsolete.