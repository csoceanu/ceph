This documentation file, `releases/giant.rst`, serves as the official release notes and changelog for **Ceph Giant (v0.87.x)**, the 7th stable release of the Ceph storage system. It tracks the evolution of the Giant release from its development phase through its final point releases.

### 1. Primary Purpose
The file documents the lifecycle of the Ceph Giant release. It provides a historical record of "Notable Changes," upgrade instructions, and architectural highlights introduced between the Firefly (v0.80.x) and Giant (v0.87.x) releases. It serves as a roadmap for administrators upgrading to this version.

### 2. Key Topics Covered
*   **Release Highlights**: High-level improvements in RADOS performance, CephFS stability, and Monitor responsiveness.
*   **Feature Introductions**: New capabilities like Local Recovery Codes (LRC), the `ceph-objectstore-tool`, and the distinction between "misplaced" and "degraded" data.
*   **Upgrade Procedures**: Specific sequencing (Monitors → OSDs → MDS/RGW) and version requirements (must upgrade from Firefly).
*   **Subsystem Updates**: Detailed fix/feature lists for RADOS, Librados, RBD, CephFS, Mon, OSD, and Rados Gateway (RGW).
*   **Breaking Changes**: Removal of legacy CephFS anchor tables and the deprecation of `mkcephfs`.
*   **Point Releases**: Specific fixes in maintenance releases v0.87.1 and v0.87.2.

### 3. Technical Keywords
*   **Components/Daemons**: `mon` (Monitor), `osd` (Object Storage Daemon), `mds` (Metadata Server), `rgw` (Rados Gateway), `librados`, `librbd`.
*   **Configuration Options**: `rbd cache`, `leveldb_write_buffer_size`, `leveldb_cache_size`, `tier cache-mode`.
*   **Commands/Tools**: `ceph-objectstore-tool`, `cephfs-journal-tool`, `radosgw-admin`, `ceph-disk`, `ceph df`.
*   **Storage Concepts**: Erasure Coding (LRC), Cache Tiering (writeback/forward), Paxos (async writes), CRUSH rulesets, Scrubbing, Backfill.
*   **APIs/Frameworks**: Swift/S3 (via RGW), Civetweb, LTTNG (tracing), `pybind`.

### 4. Target Audience
*   **Storage Administrators**: For planning upgrades and understanding health report changes (degraded vs. misplaced).
*   **System Architects**: For evaluating new features like Erasure Coding profiles and Cache Tiering improvements.
*   **Ceph Developers**: For tracking bug fixes, regression details, and the migration of internal libraries (e.g., LevelDB configuration changes).
*   **Package Maintainers**: For noting changes in systemd integration and RPM/Debian packaging structures.

### 5. Related Concepts
*   **Release Cycle**: Follows **Firefly** (previous stable) and precedes **Hammer** (referenced as the next focus for stability).
*   **Storage Backends**: References to FileStore, KeyValueStore, and the experimental RocksDB/Kinetic backends.
*   **Kernel Integration**: Relationships with `ceph-fuse` and the Linux kernel versioning (specifically kernel 3.18 limits).
*   **Data Safety**: The implementation of `rbd cache writethrough until flush` to protect data in VM environments.

---

### AI Update Trigger Guide
*   **New Point Release**: This file must be updated whenever a new `v0.87.x` tag is created, adding a new section at the top.
*   **Backported Fixes**: If a critical bug fix is backported to the Giant branch, the "Notable Changes" section for the relevant point release should be updated.
*   **Configuration Changes**: If a default value for a configuration option is changed in the Giant source code, the "Upgrading" or "Notable Changes" section must reflect this.
*   **Tooling Updates**: Changes to offline repair tools (like `ceph-objectstore-tool`) must be logged here to alert administrators.