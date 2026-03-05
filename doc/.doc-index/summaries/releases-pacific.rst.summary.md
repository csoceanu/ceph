This documentation file, `releases/pacific.rst`, is the primary release notes and changelog repository for the Ceph **Pacific (v16.2.x)** stable series. It serves as a historical record of all backported fixes, security updates, and major feature introductions for this specific release cycle.

### 1. Primary Purpose
The file documents the lifecycle of the Ceph Pacific release. It tracks:
*   **Version History**: Sequential point releases (from v16.2.0 to v16.2.15).
*   **Stability & Security**: Notable changes and critical security fixes (CVEs).
*   **Upgrade Paths**: Specific instructions for moving from Octopus or Nautilus to Pacific.

### 2. Key Topics Covered
*   **Point Release Summaries**: Bulleted "Notable Changes" for each version (e.g., v16.2.15 is the expected last release).
*   **Component Updates**: Detailed changes across Ceph components:
    *   **RADOS/BlueStore**: Sharding, RocksDB optimizations, and mclock scheduler.
    *   **CephFS**: MDS stability, snapshot scheduling, and mirroring.
    *   **RGW (Object Gateway)**: S3 Select support, bucket notifications, and multisite synchronization.
    *   **RBD (Block Device)**: Live migration, client-side encryption, and persistent write-back cache.
    *   **Cephadm/Dashboard**: Orchestration automation and new UI management wizards.
*   **Upgrade Procedures**: Mandatory steps for transitioning clusters, including handling MDS sanity checks and OSD format conversions.
*   **Changelogs**: Comprehensive lists of pull requests and issues resolved in each version.

### 3. Technical Keywords
*   **Orchestration**: `cephadm`, `orch upgrade`, `ingress service`, `maintenance mode`.
*   **Storage Engines**: `BlueStore`, `RocksDB sharding`, `omap`, `mclock_scheduler`.
*   **Protocols/APIs**: `S3 Select`, `LUKS` (encryption), `msgr v2`, `libcephsqlite`, `Dokan`.
*   **Configuration Options**: `osd_op_queue`, `require-osd-release`, `bluestore_rocksdb_options_annex`, `mon_mds_skip_sanity`.
*   **CLI Commands**: `ceph orch upgrade`, `ceph osd blocklist`, `ceph fs set`, `rbd-wnbd`.
*   **Security**: `CVE-2021-20288` (global_id reclaim), `CVE-2022-0670` (Manila vulnerability).

### 4. Target Audience
*   **System Administrators**: Managing Ceph clusters and planning upgrades.
*   **DevOps Engineers**: Automating deployments via `cephadm`.
*   **Storage Architects**: Evaluating Pacific-specific features like mclock or RBD encryption.
*   **Security Teams**: Auditing clusters for known vulnerabilities addressed in specific point releases.

### 5. Related Concepts
*   **Versioning Lifecycle**: This relates to the previous release (**Octopus**) and the subsequent release (**Quincy**).
*   **Infrastructure Management**: Closely tied to **Containerization** (Podman/Docker) and **Orchestration** (Rook/Cephadm).
*   **Operating Systems**: Mentions support for CentOS 8, Ubuntu 20.04/18.04, and Debian Buster/Bullseye.

---

### Update Trigger Summary for AI Systems
This file should be updated whenever:
1.  **A new point release (v16.2.x) is tagged**: A new section must be added with the version header, a summary of notable changes, and the full git changelog.
2.  **A security vulnerability is patched**: Security-related "Notable Changes" must be highlighted, often including specific CVE IDs.
3.  **Upgrade logic changes**: If a bug is discovered in the upgrade path (e.g., the OMAP corruption bug in v16.2.6), critical "DANGER" or "Warning" blocks must be inserted.
4.  **Feature Backports**: When a feature from a newer release (like Quincy) is backported to Pacific, it must be documented in the corresponding point release section.
5.  **Deprecations**: When specific commands or config options are renamed or removed (e.g., the transition from `blacklist` to `blocklist`).