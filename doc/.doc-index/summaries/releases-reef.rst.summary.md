This documentation file, `releases/reef.rst`, serves as the official release notes and lifecycle tracker for **Ceph Reef (v18.2.x)**, the 18th stable release of the Ceph distributed storage system.

### 1. Primary Purpose
The file documents the evolution of the Reef stable series. It provides a historical record of every point release (backport/hotfix), highlighting critical bug fixes, security patches (CVEs), major feature introductions, and detailed instructions for upgrading from previous versions (Pacific and Quincy).

### 2. Key Topics Covered
*   **Release Lifecycle**: Chronological tracking of point releases (v18.2.0 through v18.2.7) with release dates and "Notable Changes."
*   **Component Updates**:
    *   **RADOS**: Deprecation of FileStore, introduction of the Read Balancer, and mClock scheduler improvements.
    *   **RGW (RADOS Gateway)**: Multi-site resharding, server-side encryption (SSE) fixes, and bucket index tooling.
    *   **RBD (RADOS Block Device)**: Layered encryption and performance optimizations for diff-iterate.
    *   **CephFS**: Client eviction logic, snapshot scheduling, and metadata thresholding.
    *   **Dashboard**: UI overhaul, centralized logging, and Rook-backed cluster improvements.
*   **Infrastructure**: Upgrades to RocksDB (7.9.2), migration of container images to CentOS 9, and telemetry leaderboard features.
*   **Upgrade Procedures**: Comprehensive guides for both `cephadm` and non-cephadm (manual) cluster upgrades.

### 3. Technical Keywords
*   **APIs**: `rbd_aio_compare_and_writev`, `pool_is_in_selfmanaged_snaps_mode`, `librados`, `S3/Swift REST API`.
*   **Configuration Options**: `require-min-compat-client`, `osd_mclock_profile`, `mds_session_metadata_threshold`, `rgw_enable_ops_log`.
*   **Commands**: `ceph orch upgrade`, `ceph osd require-osd-release reef`, `radosgw-admin bucket check`, `ceph osd rm-pg-upmap-primary-all`.
*   **Storage Engines/Backends**: `BlueStore`, `RocksDB`, `FileStore` (Deprecated), `Beast` (RGW front-end).
*   **Concepts**: `pg-upmap-primary`, `mClock Scheduler`, `Read Balancer`, `Multi-site Replication`.

### 4. Target Audience
*   **Storage Administrators**: Seeking to upgrade clusters or troubleshoot specific regressions in the Reef series.
*   **DevOps/SREs**: Managing containerized Ceph deployments via `cephadm` or Rook.
*   **Developers**: Monitoring changes to librados, librbd, or RGW APIs for application compatibility.
*   **Package Maintainers**: Tracking dependencies (like RocksDB or Python versions) for Linux distributions.

### 5. Related Concepts
*   **Upstream Releases**: Directly follows **Quincy (v17)** and precedes **Squid (v19)**.
*   **Orchestration**: Deeply integrated with `cephadm` and **Rook** (Kubernetes).
*   **Data Integrity**: Relates to BlueStore scrubbing, PG (Placement Group) log management, and disaster recovery.

---

### Update Triggers for AI Systems
This file should be updated whenever any of the following code-level or project-level changes occur:

1.  **New Tag/Release**: When a new point release (e.g., v18.2.8) is tagged, a new section must be added with the date, notable PRs, and a full changelog.
2.  **Critical Regressions**: If a critical bug is discovered in a previous Reef version (e.g., the BlueStore regression in 18.2.5), the "Notable Changes" for the fixing release must explicitly warn users.
3.  **Deprecation Enforcement**: When a feature (like FileStore or Cache Tiering) moves from "deprecated" to "removed" or "unsupported" within the Reef lifecycle.
4.  **Security Patches**: Upon the resolution of a CVE, the specific vulnerability ID and the corresponding fix PR must be documented.
5.  **Infrastructure Shifts**: If the base OS for container images changes (e.g., the move to CentOS 9) or if a core dependency (RocksDB, Sphinx) is bumped.
6.  **CLI/API Contract Changes**: If a new `ceph` or `radosgw-admin` command is added to the Reef backport stream to resolve operational issues.