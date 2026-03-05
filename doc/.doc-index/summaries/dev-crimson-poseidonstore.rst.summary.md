This documentation defines **PoseidonStore**, an emerging storage backend for **Crimson** (the next-generation, high-performance Ceph OSD). Its primary objective is to maximize the performance of high-end NVMe SSDs by minimizing host CPU consumption, primarily by replacing heavy "black-box" components like RocksDB with a custom, sharded, and hybrid update strategy.

---

### 1. Primary Purpose
The file serves as the **architectural specification and design roadmap** for PoseidonStore. It documents the motivation behind the store, its internal data structures, I/O path logic, and the planned implementation phases within the Crimson project.

### 2. Key Topics Covered
*   **Hybrid Update Strategy**: 
    *   *Small I/O (≤ Threshold):* Uses **In-place updates** via a Write-Ahead Log (WAL) to minimize CPU-heavy host-side Garbage Collection (GC), relying on the SSD's internal GC instead.
    *   *Large I/O (> Threshold):* Uses **Out-of-place (Log-structured) updates** to avoid the "double write" penalty of WAL for large payloads.
*   **CPU Optimization Techniques**: Eliminating RocksDB and file abstraction layers; using `io_uring` for interrupt-driven I/O to save CPU cycles compared to constant polling (SPDK/DPDK).
*   **Data Structures**: 
    *   **Radix Tree**: Used for efficient Onode (object metadata) lookup based on OID prefixes.
    *   **B+Tree**: Used for managing free space (extent-based) and Object Maps (omaps).
*   **Sharding Model**: The store is divided into **Sharded Partitions (SP)**, each mapped to specific CPU cores/reactor threads to ensure lockless execution and vertical scalability.
*   **Crash Consistency**: Leveraging NVMe atomic write features (AWUPF) to ensure transaction integrity without heavy fsync overhead.

### 3. Technical Keywords
*   **APIs/Interfaces**: `io_uring`, `NVMe Atomic Write`, `queue_transaction()`, `shared_blob`.
*   **Components**: `Onode`, `WAL (Write-Ahead Log)`, `Radix Tree`, `Extent Tree`, `Superblock`, `Freelist`.
*   **Configuration**: `Threshold` (size determining update strategy), `Sharded Partition (SP)`.
*   **Storage Concepts**: WAF (Write Amplification Factor), Host-side GC vs. Device-level GC, CoW (Copy-on-Write).

### 4. Target Audience
*   **Ceph/Crimson Developers**: To understand how to implement or interface with the PoseidonStore backend.
*   **Storage Architects**: To evaluate the performance trade-offs between CPU efficiency and SSD endurance.
*   **Maintainers**: To verify if code changes in Crimson's core (like the `ObjectStore` interface) require corresponding updates to this design doc.

### 5. Related Concepts
*   **BlueStore**: The predecessor/current standard Ceph store; PoseidonStore inherits the hybrid write idea but removes the RocksDB/BlueFS layers.
*   **SeaStore**: Another Crimson backend; PoseidonStore differs by targeting NVMe SSDs specifically and utilizing in-place updates for small I/O (SeaStore is purely log-structured).
*   **NetApp’s Continuous Segment Cleaning**: Referenced as the inspiration for handling external fragmentation.

---

### Triggering Updates: When to Edit This File
This file should be updated if any of the following code changes occur:
1.  **I/O Path Changes**: If the logic for when data goes to WAL vs. Data Partition (the `Threshold` logic) changes.
2.  **Metadata Schema**: If the `onode` structure or the Radix tree lookup logic is modified.
3.  **Threading/Concurrency**: If the mapping of Sharded Partitions (SPs) to reactor threads or the handling of cross-SP transactions is altered.
4.  **Hardware Requirements**: If the store adds support for ZNS (Zoned Namespaces) or moves away from `io_uring`.
5.  **New Features**: Implementation of CoW/Clones via `shared_blob` or changes to the Omap B+Tree implementation.