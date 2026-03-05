This documentation outlines the design and architecture of **SeaStore**, a next-generation storage backend for the Ceph "Crimson" project. It is specifically engineered to exploit the performance characteristics of modern NVMe devices while utilizing the Seastar framework's high-performance asynchronous programming model.

### 1. Primary Purpose
The file documents the architectural blueprint and implementation status of **SeaStore**, an ObjectStore implementation designed to replace or complement BlueStore. It focuses on solving the mismatch between traditional file systems and the internal garbage collection (GC) and segment-based nature of Flash/NVMe storage.

### 2. Key Topics Covered
*   **Segment-Based Storage:** Designing an on-disk layout that mirrors NVMe physical segments to minimize write amplification and internal SSD garbage collection overhead.
*   **The Seastar Model:** Integration with the `seastar::future` programming model, utilizing a "run-to-completion" and sharded memory/CPU architecture.
*   **Logical Block Addressing (LBA):** A mapping system that decouples internal data references from physical on-disk locations to allow for seamless data relocation during GC.
*   **Journaling and Atomicity:** A sequential log-structured approach where updates are written as "deltas" and "blocks" to open segments.
*   **Garbage Collection (GC):** An inline cleaning mechanism intended to move live data and reclaim segments without background latency spikes.
*   **ObjectStore Integration:** Considerations for Ceph-specific requirements like PG (Placement Group) splits and merges.

### 3. Technical Keywords
*   **Frameworks/APIs:** `Seastar` (futures/sharding), `SPDK` (user-space IO), `DPDK` (zero-copy networking).
*   **Core Components:** `SegmentManager`, `TransactionManager`, `LBAManager` (and `BtreeLBAManager`), `Journal`, `Block Cache`.
*   **Data Structures:** `onode_t` (object metadata), `lba_tree`, `ghobject_t`, `CachedExtent`, `paddr_t` (physical address), `laddr_t` (logical address).
*   **Key Operations:** `apply_delta`, `try_construct_record`, `trim_journal`, `relocate_live_blocks`.

### 4. Target Audience
*   **Ceph Storage Developers:** Specifically those working on the "Crimson" OSD project.
*   **Storage Systems Engineers:** Interested in log-structured file systems or high-performance NVMe optimization.
*   **Architects:** Designing systems for zero-copy data paths and sharded processing models.

### 5. Related Concepts
*   **Log-Structured File Systems:** Heavily influenced by `f2fs` and `btrfs`.
*   **Crimson OSD:** The high-performance, Seastar-based rewrite of the Ceph OSD.
*   **BlueStore:** The predecessor to SeaStore; SeaStore aims to address BlueStore's limitations regarding Flash management and synchronous threading.
*   **ZNS (Zoned Namespaces):** Mentions of Zone devices suggest SeaStore's segment model is designed for compatibility with zoned storage.

---

### When to Update This File
This documentation is a living design spec. It should be updated when:
1.  **Component API Changes:** If the interfaces in `segment_manager.h` or `lba_manager.h` are significantly refactored.
2.  **Implementation Progress:** Several sections are marked "TODO" (e.g., GC implementation, HDD support). As these features move from design to code, the "Next Steps" and "Goals" sections should be updated.
3.  **On-Disk Format Changes:** If the structure of a `record` (header/delta/block) or the `segment` layout is modified.
4.  **Addressing "TODO" items:** Specifically regarding write amplification estimates and the heuristic functions for GC.
5.  **Sharding Logic Changes:** If the strategy for PG splits/merges across shards evolves beyond the "ObjectStore considerations" section.