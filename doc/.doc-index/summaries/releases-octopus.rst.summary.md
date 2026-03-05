This documentation file, `releases/octopus.rst`, serves as the official release history and technical changelog for the **Ceph Octopus (v15.x)** stable release series.

### 1. Primary Purpose
The file documents the lifecycle of the Ceph Octopus release, ranging from its initial major version (v15.2.0) to its final backport (v15.2.17). It serves as a central repository for **release notes, notable technical changes, security fixes (CVEs), and upgrade instructions** required by administrators and developers to maintain a Ceph cluster.

### 2. Key Topics Covered
*   **Release Lifecycle**: Announcements for each minor version (v15.2.x), identifying them as bugfix, hotfix, or security releases.
*   **Security (CVEs)**: Critical fixes for vulnerabilities such as CVE-2022-0670 (CephFS subvolume access), CVE-2021-20288 (Auth global_id reclaim), and CVE-2020-10753 (RGW newline sanitization).
*   **Feature Introductions**:
    *   **cephadm**: The then-new container-based deployment and management tool.
    *   **BlueStore**: Performance updates and the "omap" data accounting conversion.
    *   **RBD Snapshot Mirroring**: A new mirroring mode that doesn't require journaling.
    *   **Dashboard Updates**: Enhanced UI for OSD management, RGW MFA, and CephFS quotas.
*   **Upgrade Procedures**: Detailed, step-by-step instructions for moving from Nautilus/Mimic to Octopus, including specific commands for monitors, managers, and OSDs.
*   **Technical Changelogs**: Comprehensive lists of Pull Requests (PRs) linked to specific components like OSD, MDS, RGW, and MGR.

### 3. Technical Keywords
*   **Tools/Deployment**: `cephadm`, `ceph-volume`, `ceph orch`, `vstart.sh`, `teuthology`.
*   **Core Components**: `BlueStore`, `RocksDB`, `MDS` (Metadata Server), `RGW` (RADOS Gateway), `RBD` (RADOS Block Device).
*   **Configuration/APIs**: `SnapMapper`, `SnapMirror`, `msgr2` (Messenger v2 protocol), `require-osd-release`, `pg_autoscale_mode`.
*   **Compliance/Security**: `CVE`, `FIPS`, `CephX`, `KMS` (Key Management Service), `OpenStack Manila`.
*   **Monitoring**: `Prometheus`, `Grafana`, `Zabbix`, `Telemetry`.

### 4. Target Audience
*   **System Administrators**: Responsible for upgrading, patching, and maintaining Ceph clusters.
*   **Storage Engineers**: Designing and optimizing Ceph deployments using Octopus features.
*   **Ceph Developers/Backporters**: Tracking bug fixes and feature parity across releases.
*   **Security Auditors**: Reviewing patches for known vulnerabilities.

### 5. Related Concepts
*   **Containerization**: High dependency on `Podman` and `Docker` via the `cephadm` orchestrator.
*   **Backporting**: The process of moving fixes from later major versions (like Pacific or Quincy) back into Octopus.
*   **Orchestration**: Integration with Rook and OpenStack.
*   **Data Migration**: Specifically BlueStore on-disk format conversions and RBD live-migration.

---

### AI Update Trigger Summary
An AI system should update or reference this file when any of the following code changes occur:
1.  **New Minor Release**: A new version (e.g., v15.2.18) is tagged in the `octopus` branch.
2.  **Backport Merges**: Pull requests are merged into the `octopus` branch; these must be added to the "Changelog" section.
3.  **Security Patches**: Any fix addressing a CVE that affects the v15 series must be highlighted in "Notable Changes."
4.  **Breaking Configuration Changes**: Changes to default values (e.g., `osd_client_message_cap`) or deprecated options (e.g., `mds_cache_size`) must be documented for upgrade awareness.
5.  **Subvolume/API Format Changes**: Structural changes to internal keys (like the `SnapMapper` key format fix in v15.2.17) require explicit notification to prevent data corruption during upgrades.