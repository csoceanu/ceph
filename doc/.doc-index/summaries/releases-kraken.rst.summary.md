This documentation file, `releases/kraken.rst`, serves as the official release notes and technical summary for **Kraken**, the 11th stable release of the Ceph storage system.

### 1. Primary Purpose
The file documents the lifecycle of the Kraken release series (v11.2.0 through v11.2.1). It provides a high-level overview of new features, breaking changes, upgrade procedures from previous versions (Jewel), and a detailed log of bug fixes and pull requests included in each minor release.

### 2. Key Topics Covered
*   **Release Lifecycle**: Status of the release (Stable, EOL) and naming conventions.
*   **Upgrade Procedures**: Specific requirements for moving from Jewel (v10.2.z) to Kraken, including mandatory flags like `sortbitwise`.
*   **BlueStore (Experimental)**: Introduction of the new storage backend, its disk format, and configuration.
*   **Component Updates**:
    *   **RADOS**: Introduction of `ceph-mgr` (manager daemon), AsyncMessenger as default, and erasure-coded overwrite support.
    *   **RGW (Object Gateway)**: Bucket resharding, compression, S3 lifecycle API (expiration), and NFS v3 support.
    *   **RBD (Block Device)**: Support for EC pools, asynchronous Python API, and snapshot mirroring.
    *   **CephFS (File System)**: Improved scrub/repair capabilities and UID/GID control updates.
*   **Notable Technical Changes**: Changes in OSD error reporting (ENXIO), snap trimmer throttling, and monitor safety (preventing accidental pool deletion).

### 3. Technical Keywords
*   **Daemons**: `ceph-mgr`, `ceph-osd`, `ceph-mon`, `ceph-mds`, `radosgw`, `rbd-mirror`.
*   **Storage Backends**: `BlueStore`, `FileStore`, `RocksDB`, `LevelDB`.
*   **Configuration Options**: `ms type = async`, `osd_enxio_on_misdirected_op`, `mon_allow_pool_delete`, `osd max trimming pgs`, `osd scrub during recovery`.
*   **Commands**: `ceph osd crush rm`, `ceph osd perf`, `ceph-disk --bluestore`, `cephfs-data-scan`.
*   **Protocols/APIs**: `S3`, `Swift`, `NFS v3`, `AsyncMessenger`, `librados`, `librbd`.
*   **Concepts**: `Erasure Coding (EC)`, `CRUSH map`, `Snap Trimmer`, `Quiesce`, `Multisite`.

### 4. Target Audience
*   **Storage Administrators**: For planning upgrades, understanding new default behaviors (like AsyncMessenger), and implementing safety configurations.
*   **DevOps Engineers**: For managing package dependencies (Debian/RPM) and systemd unit changes.
*   **Developers**: To track API changes in `librados`, `librbd`, and `libcephfs`.
*   **QA/Testing Teams**: For identifying fixed issues and changes in test suites (`teuthology`).

### 5. Related Concepts
*   **Jewel/Luminous**: This release acts as the bridge between Jewel (previous stable) and Luminous (next stable/long-term release).
*   **Storage Backend Evolution**: The transition from FileStore (XFS-based) to BlueStore (raw block device).
*   **Architecture Shift**: The move toward centralized management via the new `ceph-mgr` daemon.

---

### AI Update Trigger Analysis
An AI system should update or reference this file when code changes involve:
1.  **Version Bumping**: When a new bugfix release (v11.2.x) is tagged.
2.  **Configuration Defaults**: If a default value mentioned here (e.g., `ms type`) is changed in the source code.
3.  **Deprecations**: When a feature introduced as "experimental" in this file (like BlueStore or EC overwrites) is graduated to stable or removed.
4.  **CLI/API Contracts**: If the behavior of a documented command (e.g., `osd out` preserving weight) is modified.
5.  **Upgrade Path Logic**: If the requirements for upgrading (e.g., mandatory `sortbitwise` flag) are altered in the monitor's safety checks.