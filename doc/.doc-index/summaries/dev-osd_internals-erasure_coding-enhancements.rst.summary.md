This documentation file, `enhancements.rst`, serves as the **high-level architectural roadmap and design specification for improving Erasure Coding (EC) performance in Ceph**, specifically targeting small random I/O workloads.

### 1. Primary Purpose
The file documents the proposed and ongoing technical enhancements to Ceph’s Erasure Coding implementation. Its goal is to transform EC from a "cold storage" (sequential write) solution into a viable option for block (`RBD`) and file (`CephFS`) storage by reducing I/O amplification, network bandwidth, and latency.

### 2. Key Topics Covered
*   **Performance Bottlenecks**: Analysis of why current EC reads/writes (which round up to full stripes) are inefficient for small random I/O.
*   **I/O Path Optimizations**:
    *   **Partial Reads/Writes**: Reading/writing only necessary shards or sub-chunks rather than full stripes.
    *   **Parity-Delta-Writes**: Using XOR/Galois Field arithmetic to update parities by calculating differences (deltas) rather than full-stripe recalculation.
    *   **Direct I/O**: Allowing clients to bypass the Primary OSD and communicate directly with Shard OSDs for reads and certain writes.
*   **Metadata Evolution**: Proposals to reduce the overhead of updating `object_info_t`, PG logs, and stats on every shard, moving toward an $M+1$ update model.
*   **Structural Improvements**: Implementing variable/automatic chunk sizes to balance storage efficiency for small objects with performance for large ones.
*   **Integrity & Maintenance**: Updates to Deep Scrubbing (longitudinal XOR checks) and removing redundant CRC calculations for overwritten objects.
*   **Implementation Roadmap**: A detailed "Stories" section breaking the work into incremental phases (Test tools, Plugin updates, Primary selection logic, etc.).

### 3. Technical Keywords
*   **APIs/Libraries**: `ISA-L`, `JErasure`, `librados`, `OSDC`, `CLAY`, `LRC`.
*   **Data Structures**: `object_info_t`, `pg_log_t`, `hinfo`, `MOSDOp`, `MOSDEcSubOpDelta`, `MOSDEcSubOpSequence`.
*   **Concepts**: Parity-delta-write, I/O amplification, Stripe Cache, Sub-chunking, Acting Primary, Backfill Vector, Galois Field (GF) arithmetic.
*   **Configuration**: `k+m` configurations, `NONATOMIC` flags, `noout` maintenance mode.

### 4. Target Audience
*   **Ceph Core Developers**: Specifically those working on the OSD (Object Storage Daemon) and EC backend.
*   **Storage Architects**: Designing high-performance clusters who need to understand the trade-offs of EC vs. Replication.
*   **QA/Test Engineers**: The document contains extensive sections on required test coverage and new "thrashing" strategies for EC.

### 5. Related Concepts
*   **BlueStore**: Much of the data integrity (CRC) responsibility is shifted here.
*   **Peering & Recovery**: The proposed metadata changes directly impact how OSDs reach consensus on object versions.
*   **RBD/CephFS**: The primary beneficiaries of these enhancements due to their reliance on small-block random I/O.
*   **RAID 5/6**: The "Parity-delta-write" is a classic optimization borrowed from hardware RAID controllers.

---

### When to update this file
This file should be updated if any of the following occur:
1.  **Protocol Changes**: If new `MOSD*` messages are added or existing EC-sub-op flows are modified.
2.  **Plugin Evolution**: If new Erasure Coding plugins (beyond ISA-L/JErasure) are added that require different delta-handling.
3.  **Metadata Format Shifts**: If the structure of `object_info_t` or the PG Log is altered to accommodate new version tracking (e.g., the "vector of version numbers").
4.  **Feature Completion**: As the listed "Stories" move from proposal to merged code, their status and implementation details should be updated to reflect reality.
5.  **Direct I/O Constraints**: If the requirements for client atomicity or the `NONATOMIC` flag logic change.