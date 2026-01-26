### Documentation Summary: `filestore-config-ref.rst`

#### 1. Primary Purpose
This file serves as the definitive configuration reference for **Filestore**, the legacy storage back end for Ceph Object Storage Daemons (OSDs). It details the tunable parameters, performance thresholds, and operational behaviors of how Ceph interacts with underlying POSIX file systems (XFS, Btrfs, ext4) to store object data.

#### 2. Key Topics Covered
*   **Deprecation Status**: Explicit notice that BlueStore replaced Filestore as the default in Luminous, and Filestore support ends with the Reef release.
*   **Extended Attributes (XATTRs)**: Logic for handling file system XATTR limits and the transition to using an external key/value database when limits are exceeded.
*   **Synchronization & Flushing**: Management of "commit points," synchronization intervals (min/max), and the "flusher" mechanism for data persistence and journal clearing.
*   **Queue & Thread Management**: Configuration of operation concurrency, byte limits in the queue, and thread timeouts (including "suicide" timeouts for hung operations).
*   **File System Specifics**: Specialized settings for Btrfs (snapshots/cloning) and journaling modes (parallel vs. write-ahead).
*   **Directory Management**: Logic for splitting and merging subdirectories (thresholds and randomization) to manage large object counts.

#### 3. Technical Keywords
*   **Back End Engines**: `Filestore`, `BlueStore`.
*   **File Systems**: `XFS`, `Btrfs`, `ext4`.
*   **Core Configuration Options**:
    *   `filestore_max_inline_xattr_size` (and file-system-specific variants)
    *   `filestore_max_sync_interval` / `filestore_min_sync_interval`
    *   `filestore_queue_max_ops` / `filestore_queue_max_bytes`
    *   `filestore_op_threads`
    *   `filestore_merge_threshold` / `filestore_split_multiple`
*   **Operations**: `sync_file_range`, `omap`, `parallel journaling`, `writeahead journaling`.
*   **Tools**: `ceph-objectstore-tool`.

#### 4. Target Audience
*   **System Administrators**: Managing legacy Ceph clusters (pre-Quincy) that still utilize Filestore.
*   **Performance Engineers**: Tuning OSD latency and throughput by adjusting synchronization and queue depths.
*   **Developers/Maintainers**: Understanding the legacy storage architecture or troubleshooting OSD crashes related to EIO or thread timeouts.

#### 5. Related Concepts
*   **RADOS (Reliable Autonomic Distributed Object Store)**: The underlying layer Filestore supports.
*   **BlueStore**: The modern successor; this documentation is often consulted during **BlueStore Migration** planning.
*   **Journaling**: The mechanism used by Filestore to ensure atomicity, which is distinct from BlueStore’s implementation.
*   **OSD Lifecycle**: Specifically how OSDs handle data persistence and consistency points.

---

### Update Triggers for AI Systems
An AI should flag this file for updates if any of the following code-level changes occur:
1.  **Deprecation/Removal**: If the Ceph codebase officially removes Filestore support (e.g., in Reef or later releases), this file should be archived or deleted.
2.  **Default Value Changes**: Changes to default values in the C++ source code for any `filestore_*` variables (typically found in `common/options.cc` or similar config headers).
3.  **New Tunables**: Introduction of new parameters in the `FileStore` class for handling XATTRs or directory sharding.
4.  **Filesystem Compatibility**: Changes in how Ceph interacts with XFS or Btrfs (e.g., deprecating Btrfs-specific features like `filestore_btrfs_snap`).
5.  **Refactoring**: Changes to the OSD threading model or internal queueing logic that render settings like `filestore_op_threads` obsolete.