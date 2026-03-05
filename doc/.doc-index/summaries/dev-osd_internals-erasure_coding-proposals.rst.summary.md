This documentation file, located at `dev/osd_internals/erasure_coding/proposals.rst`, is a strategic design document outlining the evolution of the Erasure Coding (EC) backend in Ceph. It specifically addresses the transition from append-only EC to a system that supports efficient **overwrites** and **Read-Modify-Write (RMW)** operations.

### 1. Primary Purpose
The file serves as a **technical roadmap and design proposal** for enhancing the `ECBackend`. Its main goal is to define the architectural changes required to support partial stripe writes, improve performance through caching, and maintain data consistency/recovery during complex failure scenarios in erasure-coded pools.

### 2. Key Topics Covered
*   **Write Algorithms**: Comparison between standard Read-Modify-Write (RMW) and **Parity-Delta-Write** (which reduces network hops by computing deltas on data shards).
*   **Stripe Caching**: Introduction of an `ExtentCache` to buffer small sequential writes (like journals/logs) and reduce IO latency.
*   **Two-Phase Operations**: A proposal for a **Prepare/Apply** model using temporary objects and in-memory tracking to ensure atomicity and rollback capabilities.
*   **Peering and Recovery**: Modifications to how `last_update` is determined when some shards may not have participated in (or recorded) a partial write.
*   **Deep Scrubbing & Checksums**: Strategies for storing per-shard checksums in OMAP to support data integrity verification without reading the entire stripe.
*   **Version Consistency**: Addressing the "Inconsistent `object_info_t`" problem, where shards not involved in a partial write will have lagging version metadata.
*   **Implementation Roadmap**: A staged plan to merge features (checksums, moving primitives, version vectors) incrementally.

### 3. Technical Keywords
*   **APIs/Primitives**: `clonerange`, `move-range` (proposed ObjectStore primitive), `ObjectStore`, `KV store`, `PGLog`.
*   **Data Structures**: `object_info_t`, `pg_log_entry_t`, `ghobject`, `ExtentCache`, `omap`.
*   **Concepts**: Parity-delta, RMW, Write Amplification, Authoritative Log, Rollback, Shard participating/witnessing, Version Vectors, Divergent Priors.
*   **Components**: `ECBackend`, `NewStore` (BlueStore predecessor), `FileStore`, `RADOS`.

### 4. Target Audience
*   **Ceph Core Developers**: Specifically those working on the OSD (Object Storage Daemon) and `ECBackend`.
*   **Storage Architects**: Anyone designing low-level storage primitives or consistency protocols.
*   **Maintainers**: Developers responsible for peering, recovery, and scrub logic within RADOS.

### 5. Related Concepts
*   **BlueStore/FileStore**: The underlying object stores that must implement the proposed primitives.
*   **LRC/SHEC**: Specific erasure coding plugins mentioned regarding how they handle parity and shard rotation.
*   **Peering State Machine**: The core Ceph logic for reaching consensus on PG (Placement Group) history.
*   **RBD (RADOS Block Device)**: Mentioned as a primary consumer of version assertions that are complicated by EC overwrites.

---

### Update Triggers for AI Systems
This file should be updated or referenced when the following code changes occur:
1.  **Erasure Code Plugin Interface**: If the `ErasureCodeInterface` is modified to include new methods for parity-delta calculations.
2.  **PGLog Logic**: Changes to how `last_update` is calculated or how log entries are merged during peering.
3.  **ObjectStore Primitives**: Implementation of new "move" or "atomic clone" operations in BlueStore or generic ObjectStore.
4.  **Metadata Changes**: Updates to the fields within `object_info_t` or `pg_log_entry_t` (e.g., adding shard participation vectors).
5.  **Scrubbing Logic**: Modification to how OSDs perform deep scrubs on EC pools, specifically involving OMAP-based checksums.
6.  **RADOS Protocol**: Any changes to how ACKs are generated for EC writes or changes to version-assertion flags in client requests.