This documentation file, `rados/configuration/bluestore-config-ref.rst`, serves as the definitive technical reference for **BlueStore**, the default storage engine for Ceph OSDs. It details how BlueStore interacts directly with raw block devices, manages internal metadata via RocksDB, handles memory allocation, and applies data optimization techniques like compression and checksumming.

### 1. Primary Purpose
The file provides architectural guidance and configuration specifications for deploying and tuning BlueStore OSDs. It explains how to distribute BlueStore's components (Data, DB, and WAL) across different storage media to optimize performance and storage efficiency.

### 2. Key Topics Covered
*   **Device Management**: Defines the three logical device types: **Primary (block)**, **WAL (Write-Ahead Log)**, and **DB (Metadata)**.
*   **Provisioning Strategies**: Instructions for "colocated" (single device) vs. "distributed" (mixed HDD/SSD) setups using `ceph-volume`.
*   **Sizing Guidance**: Mathematical recommendations for DB device sizing (1% to 4% of data size) based on workloads (RGW vs. RBD).
*   **Cache Management**: Explains both **Automatic Cache Sizing** (priority-based autotuning using TCMalloc) and **Manual Cache Sizing**.
*   **Data Integrity & Efficiency**: Detailed sections on **Checksums** (CRC32C, xxhash) and **Inline Compression** (Snappy, Zlib, LZ4, Zstandard).
*   **Advanced Features**: Covers RocksDB Sharding (introduced in Pacific), SPDK for NVMe bypass, and DSA (Data Streaming Accelerator) for hardware offloading.
*   **Storage Overhead**: Analysis of **Minimum Allocation Size** and its impact on "space amplification" in RGW/EC workloads.

### 3. Technical Keywords
*   **Tools/Commands**: `ceph-volume`, `ceph-bluestore-tool`, `accel-config`, `vgcreate`, `lvcreate`.
*   **Configuration Options**: `bluestore_min_alloc_size`, `osd_memory_target`, `bluestore_cache_autotune`, `bluestore_compression_mode`, `bluestore_rocksdb_cf`.
*   **APIs/Libraries**: RocksDB, SPDK, TCMalloc, DML (Data Mover Library), RADOS.
*   **Storage Concepts**: OMAP keys, Write-Ahead Log (WAL), Column Families (sharding), CRC32C, Space Amplification.

### 4. Target Audience
*   **Storage Administrators**: Planning hardware layouts and OSD provisioning.
*   **Performance Engineers**: Tuning cache ratios and compression settings to meet specific SLAs.
*   **System Architects**: Designing Ceph clusters with mixed media (NVMe/SSD/HDD).

### 5. Related Concepts
*   **FileStore**: The legacy storage engine BlueStore replaced.
*   **RocksDB**: The embedded KV store used by BlueStore for all metadata.
*   **LVM (Logical Volume Manager)**: The underlying volume management layer used by `ceph-volume`.
*   **RGW (RADOS Gateway)**: Specifically mentioned regarding metadata-heavy workloads and small-file storage overhead.

---

### When to Update This File
This file is a "living document" and should be updated in the following scenarios:
1.  **Default Value Changes**: If the default `min_alloc_size` or `cache_meta_ratio` is modified in the source code (e.g., `common/options.cc`).
2.  **New Hardware Support**: If support for new accelerators (like updated Intel DSA features) or new storage technologies (like ZNS or new NVMe specs) is added.
3.  **RocksDB Integration**: If the way Ceph interacts with RocksDB changes (e.g., new sharding strategies or new Column Families).
4.  **Tooling Updates**: If `ceph-volume` or `ceph-bluestore-tool` introduce new flags or deprecate old workflows for provisioning.
5.  **New Compression/Checksum Algorithms**: If a new library (e.g., a new compression codec) is integrated into BlueStore.
6.  **Performance Logic Changes**: If the cache autotuning algorithm or the way "spillover" (DB to Primary) is handled is modified.