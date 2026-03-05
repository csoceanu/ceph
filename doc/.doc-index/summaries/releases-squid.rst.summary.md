This documentation file, `releases/squid.rst`, serves as the **official release notes and changelog for the Ceph "Squid" (v19.x) release series**. It tracks the evolution of the 19th stable release of the Ceph storage system, including major feature highlights, critical hotfixes, and detailed backport history.

### 1. Primary Purpose
The file documents the lifecycle of the **Ceph Squid release**. It provides a chronological record of versions (starting from the initial v19.2.0 release), high-level feature summaries, upgrade instructions, and a comprehensive list of Pull Requests (PRs) and bug fixes included in each minor/point release.

### 2. Key Topics Covered
*   **Version Highlights**: Major architectural changes and new features in RADOS, RGW, RBD, and CephFS.
*   **Notable Changes & Hotfixes**: Critical updates, such as the RGW data loss bug fix in v19.2.2.
*   **Release Dates**: Official timestamps for each point release.
*   **Component-Specific Updates**:
    *   **RGW**: S3 Object Lock, User Accounts feature, and IAM API compatibility.
    *   **RADOS**: BlueStore optimizations, mClock scheduler tuning, and balancer improvements.
    *   **CephFS**: Subvolume quiesce commands, metadata threshold management, and snapshot/clone support.
    *   **Crimson**: Technical preview status for the next-generation OSD.
*   **Upgrade Procedures**: Step-by-step guides for both `cephadm` and manual (non-cephadm) deployments.
*   **Changelog**: A granular list of every PR merged into the release branch.

### 3. Technical Keywords
*   **APIs**: `PutObjectLockConfiguration`, `S3 Object Lock`, `STS` (Security Token Service), `IAM APIs`, `librbd`.
*   **Commands**: `ceph osd rm-pg-upmap-primary-all`, `rbd trash mv`, `fs subvolume create`, `ceph auth rotate`, `ceph orch upgrade`.
*   **Configuration Options**: `osd_op_num_shards_hdd`, `mds_session_metadata_threshold`, `rgw_realm`, `notification_v2`.
*   **Components**: `RGW` (RADOS Gateway), `RBD` (RADOS Block Device), `MDS` (Metadata Server), `MGR` (Manager), `BlueStore`, `RocksDB`, `Crimson/Seastore`.
*   **Security**: `CVE-2023-43040`, `CVE-2024-48916`, `ARN-based conditionals`.

### 4. Target Audience
*   **System Administrators**: Responsible for upgrading and maintaining Ceph clusters.
*   **Storage Engineers**: Designing storage solutions using Squid's new features (e.g., Read Balancing or RGW Accounts).
*   **Software Developers**: Integrating applications with Ceph's S3/RBD/CephFS interfaces.
*   **DevOps/SREs**: Managing automated deployments via `cephadm` or monitoring via Grafana/Prometheus.

### 5. Related Concepts
*   **Release Orchestration**: Relates to `cephadm` and the Ceph backport process.
*   **Data Integrity**: Relates to BlueStore scrubbing, RGW garbage collection, and snapshot consistency.
*   **AWS Compatibility**: Relates to RGW’s ongoing effort to mirror AWS S3 and IAM behaviors.
*   **Next-Gen Performance**: Relates to the "Crimson" project (high-performance OSD for NVMe).

---

### AI Update Trigger Analysis
This file must be updated whenever specific events occur in the Ceph codebase or release lifecycle. An AI system should flag this file for modification when:

1.  **A New Version is Tagged**: When a new point release (e.g., v19.2.4) is cut, a new section must be added to the top of the file.
2.  **Backports are Merged**: Whenever a Pull Request is merged into the `squid` branch, the PR title, number, and author should be appended to the current version's **Changelog** section.
3.  **Critical Bugs/Hotfixes**: If a "Stop Ship" or critical data-loss bug is identified (like the CopyObject regression), a `.. warning::` block and a **Notable Changes** entry must be added.
4.  **Documentation Refinement**: When technical writers update specific component docs (RADOS, RGW, etc.) that affect Squid-specific behavior, the corresponding summaries in this file should be synced.
5.  **Security Patches**: The inclusion of a fix for a CVE must be explicitly noted in the Changelog and Notable Changes.