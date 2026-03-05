This documentation file, `releases/firefly.rst`, provides a chronological and technical history of the **Firefly** release cycle (v0.80.x), which was the 6th stable release of Ceph and its first designated Long-Term Support (LTS) version.

### 1. Primary Purpose
The file serves as the **official release notes and upgrade guide** for the Ceph Firefly series. It documents the transition from previous versions (Dumpling/Emperor), outlines major architectural features introduced in the v0.80 branch, and provides a granular log of bug fixes and "Notable Changes" for every point release from v0.80.1 through v0.80.11.

### 2. Key Topics Covered
*   **Major Features**: Detailed introduction of Erasure Coding, Cache Tiering, Primary Affinity, and experimental Key/Value OSD backends.
*   **Release Lifecycle**: Documentation of the v0.80 stable release and its subsequent eleven bugfix point releases.
*   **Upgrade Procedures**: Specific sequencing for upgrading daemons (Monitors → OSDs → MDS/RGW) and version-specific transition notes.
*   **CRUSH Tunables**: Instructions on adjusting CRUSH maps (e.g., `straw_calc_version`) and managing data rebalancing.
*   **Component-Specific Updates**: Significant changes to the RADOS Gateway (RGW), CephFS (MDS), RBD (Block Device), and the RADOS Object Store.

### 3. Technical Keywords
*   **APIs & Interfaces**: `librados`, `librbd`, `libcephfs`, `ObjectWriteOperation`, `TMAP2OMAP`.
*   **Configuration Options**: `mon warn on legacy crush tunables`, `osd pool default crush replicated ruleset`, `hashpspool`, `leveldb cache size`.
*   **Commands**: `ceph osd crush set-tunable`, `ceph osd crush reweight-all`, `ceph mds set allow_new_snaps`, `crushtool`.
*   **Technical Components**: Civetweb (RGW frontend), Jerasure (Erasure coding library), HitSets (Tiering tracking), Bloom Filters.
*   **Storage Concepts**: Erasure Coding (EC), Cache Pools, Writeback/ReadOnly modes, Primary Affinity, OMAP vs. TMAP.

### 4. Target Audience
*   **Storage Administrators**: Managing Ceph clusters running or upgrading to/from the Firefly release.
*   **Software Engineers/SREs**: Needing to understand breaking changes in APIs or internal data encodings.
*   **Package Maintainers**: Tracking changes in RPM/Debian packaging structures (e.g., the split of `ceph-common`).

### 5. Related Concepts
*   **LTS (Long Term Support)**: Firefly was the baseline for extended maintenance.
*   **Backward Compatibility**: The file discusses interaction with older clients (Pre-Bobtail/Dumpling) and kernel version requirements (e.g., Linux 3.2, 3.9).
*   **Data Integrity**: Coverage of Deep Scrubbing, snapshot trimming, and XFS-specific allocation hint bugs (issue #8830).

---

### AI Update Trigger Analysis
An AI system should update or reference this file when the following code-level changes occur:
1.  **Version Tagging**: When a new point release (e.g., v0.80.12) is tagged in the Firefly branch.
2.  **Backports**: When a pull request is merged with a `backport: firefly` label.
3.  **Upgrade Logic Changes**: If the `ceph-deploy` or internal mon/osd upgrade sequencing logic is modified.
4.  **Feature Deprecation**: If any feature mentioned as "experimental" in this file (like the standalone RGW or KeyValueStore) is promoted or deprecated in later versions.
5.  **Critical Bug Fixes**: When a tracker issue (linked in the file via `#issue-number`) is resolved and requires documentation of the fix for stable users.