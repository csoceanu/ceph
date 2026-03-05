This documentation defines the architectural design for **Erasure Coding (EC) Direct Reads** (and associated split reads for replicated pools) within the Ceph storage system. Its primary goal is to optimize read performance by allowing clients to bypass the Primary OSD and fetch data shards directly from multiple OSDs.

### 1. Primary Purpose
The file documents the shift from a **Primary-mediated read model** (where the Primary OSD aggregates data for the client) to a **Client-direct model**. This design aims to reduce network hops, distribute CPU load for de-striping to the client, and improve latency/bandwidth utilization for EC and replicated pools.

### 2. Key Topics Covered
*   **Read Classifications**: Defines "Small" (sub-chunk), "Medium" (multi-shard, single stripe), and "Large" (multi-stripe) reads and their respective data flows.
*   **Client-Side Orchestration**: Detailed logic for the `Objecter` and the new `SplitRead` object.
*   **Fallback Mechanisms**: Procedures for falling back to the Primary OSD during OSD failures, stale maps, or read errors.
*   **Consistency & Ordering**: Handling "Torn Writes" via version checking and "Read Instability" via log analysis.
*   **Implementation Constraints**: Changes to chunk sizes (up to 32k for SSD/256k for HDD), OSD backend enhancements, and plugin limitations (Jerasure/ISA-L).
*   **Kernel Integration**: The requirement to port this logic from `librados` to the Linux kernel (`krbd`).
*   **Performance & Throttling**: The impact of increasing IOPS count while decreasing internal cluster traffic.

### 3. Technical Keywords
*   **Components**: `Objecter`, `SplitRead`, `EC Backend`, `librados`, `krbd`.
*   **Flags/Opcodes**: `BALANCED_READ`, `RWORDERING`, `Sparse read`, `objver` (User Version).
*   **Data Structures**: `OI` (Object Info) attribute, `K+M` shards, `Data Stripes`, `Data Shards`.
*   **Logic/States**: `De-striping`, `Concatenation`, `Torn Writes`, `Stale OSD Map`, `Epoch`.
*   **Plugins**: `Jerasure`, `ISA-L`, `LRC` (Large Role Code).

### 4. Target Audience
*   **Ceph Core Developers**: Those working on the OSD (Object Storage Daemon) or RADOS layer.
*   **Client Library Maintainers**: Developers of `librados` and the Ceph Kernel team.
*   **Performance Engineers**: Individuals tuning cluster throughput and latency for EC pools.

### 5. Related Concepts
*   **Erasure Coding (EC)**: The underlying data durability scheme.
*   **Replicated Pools**: This design borrows "Balanced Read" concepts from replicated storage and applies them to EC.
*   **RADOS Protocol**: The low-level communication protocol between clients and OSDs.
*   **Object Versioning**: The mechanism used to ensure data consistency during parallel reads.

---

### When to Update This Documentation
An AI or developer should update this file if code changes occur in the following areas:

1.  **Objecter Logic**: If the state machine for `SplitRead` or the criteria for falling back to the Primary OSD changes.
2.  **OSD Error Codes**: If new error codes are introduced to handle recovery-driven read failures (as mentioned in the "Missing Objects" section).
3.  **Default Configuration**: If the recommended default chunk sizes (currently suggested at 16k-256k) are adjusted based on performance testing.
4.  **Plugin Expansion**: If support is added for the `LRC` plugin or other EC plugins beyond Jerasure and ISA-L.
5.  **Kernel Implementation**: When the "separate investigation" for the kernel-mode client is completed and a specific implementation path is chosen.
6.  **Throttling Framework**: Once the "open problem" regarding how to throttle split I/O operations as a single unit is resolved.
7.  **New Op Support**: If the system expands to support multiple concurrent reads, attribute reads, or encrypted sparse reads.