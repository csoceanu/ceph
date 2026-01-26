This documentation file, `bluestore-config-ref.rst`, serves as the technical reference manual for **BlueStore**, the primary storage engine for Ceph OSDs (Object Storage Daemons). It details how BlueStore interacts directly with raw block devices, manages internal metadata, and optimizes performance through caching, compression, and hardware acceleration.

### 1. Primary Purpose
The file provides comprehensive guidance on configuring and tuning BlueStore. It explains the architectural relationship between the primary data storage, the metadata database (RocksDB), and the Write-Ahead Log (WAL), while providing instructions for deployment via `ceph-volume`.

### 2. Key Topics Covered
*   **Device Management**: Explanation of the three-device architecture: **Primary** (data), **DB** (RocksDB metadata), and **WAL** (internal journal).
*   **Provisioning Strategies**: Scenarios for "colocated" (single device) vs. "mixed" (HDD for data + SSD/NVMe for DB/WAL) deployments.
*   **Cache Management**: Detailed mechanics of both **Automatic Cache Sizing** (autotuning via TCMalloc) and **Manual Cache Sizing**.
*   **Data Integrity & Efficiency**: Configuration of **Checksums** (CRC32c, xxhash) and **Inline Compression** (Snappy, Zlib, LZ4, Zstd).
*   **Advanced Internal Tuning**: RocksDB sharding (column families) and throttling settings.
*   **Hardware Acceleration**: Support for high-performance interfaces like **SPDK** (for NVMe) and **Intel DSA** (Data Streaming Accelerator).
*   **Storage Economics**: Deep dive into **Minimum Allocation Size** and how it impacts "space amplification" across different drive types (HDD/SSD/QLC).

### 3. Technical Keywords
*   **Tools**: `ceph-volume`, `ceph-bluestore-tool`, `accel-config`, `lspci`.
*   **Devices**: `block`, `block.db`, `block.wal`, `logical volume (LV)`, `spdk`.
*   **APIs/Config Options**:
    *   *Memory*: `osd_memory_target`, `bluestore_cache_autotune`, `bluestore_cache_meta_ratio`.
    *   *Storage*: `bluestore_min_alloc_size`, `bluestore_compression_mode`, `bluestore_csum_type`.
*   **Internal Components**: `RocksDB`, `TCMalloc`, `Column Families`, `CRC32C`.

### 4. Target Audience
*   **Storage Administrators**: Planning OSD layouts and hardware procurement.
*   **Performance Engineers**: Tuning cache ratios and compression settings to optimize throughput/latency.
*   **DevOps/SREs**: Troubleshooting "space amplification" issues or implementing automation scripts for OSD deployment.

### 5. Related Concepts
*   **Filestore**: The legacy storage engine BlueStore replaced.
*   **RADOS**: The underlying object store layer that BlueStore serves.
*   **RGW & RBD**: Workloads that significantly impact BlueStore sizing (RGW requires larger DB devices due to OMAP usage).
*   **LVM**: The logical volume manager used by `ceph-volume` to prep devices.

---

### Maintenance Triggers for AI Systems
This file should be updated whenever code changes occur in the following areas:

1.  **Default Value Changes**: If the default `bluestore_min_alloc_size` or cache ratios are modified in the source code (e.g., changes in `common/options.cc`).
2.  **New Hardware Support**: If support for new accelerators (similar to SPDK or Intel DSA) or new memory allocators is added.
3.  **RocksDB Integration**: If the internal sharding logic, column family naming, or prefix handling changes.
4.  **Compression/Hashing Algorithms**: If new libraries (e.g., a new compression codec) are integrated into BlueStore.
5.  **Provisioning Workflow**: If `ceph-volume` changes the way it handles symlinks (`block.db`, `block.wal`) or how it interacts with `tmpfs` data directories.
6.  **Upgrade Paths**: When a new Ceph release introduces breaking changes or "experimental" features becoming "stable" (e.g., Pacific’s dynamic-level support).