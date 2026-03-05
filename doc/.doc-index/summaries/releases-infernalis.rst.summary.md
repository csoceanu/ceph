This documentation file, `releases/infernalis.rst`, serves as the official release notes and upgrade guide for **Infernalis**, the 9th stable release of the Ceph distributed storage system (version 9.2.x).

### 1. Primary Purpose
The file provides a historical record of the Infernalis release cycle. It documents major architectural shifts, version-specific bug fixes (point releases), supported operating systems, and critical upgrade procedures for users moving from previous major releases (Firefly and Hammer) to Infernalis.

### 2. Key Topics Covered
*   **Release Lifecycle**: Specific notes for v9.2.1 (point release), v9.2.0 (stable), and earlier development releases (v9.0.x and v9.1.0).
*   **Architectural Shifts**: Transition from `sysvinit/upstart` to `systemd` and changing the daemon user from `root` to `ceph`.
*   **Component Improvements**:
    *   **RADOS**: Write-proxying for cache tiers, SHEC erasure coding, and the unified IO queue.
    *   **RBD**: Introduction of the `object-map`, `exclusive-lock`, and `deep-flatten` features.
    *   **RGW**: Swift API compatibility, object expiration, and orphan detection tools.
    *   **CephFS**: Snapshot renaming and metadata repair tools.
*   **Upgrade Instructions**: Detailed manual steps for handling data ownership (chown) and cluster fencing during upgrades.
*   **Deprecations**: Changes in command-line syntax (e.g., `ceph scrub` moving to `ceph mon scrub`).

### 3. Technical Keywords
*   **Core APIs**: `librados`, `librbd`, `libcephfs`, `ObjectStore`, `C++11`.
*   **Daemons/Tools**: `ceph-mon`, `ceph-osd`, `ceph-mds`, `radosgw`, `ceph-fuse`, `ceph-objectstore-tool`, `ceph-disk`.
*   **Features/Config**: `object-map`, `exclusive-lock`, `SHEC` (Erasure Coding), `systemd`, `setuser match path`, `OPERATION_FULL_TRY`, `NewStore` (prototype).
*   **Platforms**: CentOS 7, Debian Jessie, Ubuntu Trusty, Fedora 22.

### 4. Target Audience
*   **Storage Administrators**: Responsible for upgrading clusters and managing daemon permissions.
*   **Software Developers**: Using `librados` or `librbd` who need to be aware of breaking ABI changes or new API flags.
*   **DevOps/SREs**: Building automation for Ceph deployment via `systemd` or `ceph-deploy`.

### 5. Related Concepts
*   **Cluster Orchestration**: Relates to how Linux distributions manage services (transitioning to `systemd`).
*   **Security & Permissions**: Relates to Linux User/Group management and SELinux policies.
*   **Data Integrity**: Relates to CRUSH maps, scrubbing, and object repair.
*   **Backward Compatibility**: Specifically the transition path from **Firefly (v0.80)** $\rightarrow$ **Hammer (v0.94)** $\rightarrow$ **Infernalis (v9.2)**.

---

### Integration Guide for AI Systems
An AI should monitor or update this file when the following code-level changes occur:
1.  **Version Tags**: When a new point release (e.g., v9.2.2) is tagged.
2.  **CLI Changes**: If a command is deprecated or a subcommand is renamed (e.g., `osdmaptool` arguments).
3.  **Dependency Shifts**: If the required compiler version changes (e.g., requiring C++14) or a new Linux distro is added to the build gate.
4.  **Breaking API Changes**: If a return value in `librados` or `librbd` changes its behavior (as seen with `rbd_aio_read`).
5.  **Default Feature Toggling**: When a performance feature (like `objectmap`) is changed from disabled to enabled-by-default.