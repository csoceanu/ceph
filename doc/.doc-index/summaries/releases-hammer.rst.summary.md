This documentation file, `releases/hammer.rst`, serves as the official release notes and changelog history for **Ceph Hammer**, the 8th stable release of the Ceph storage system. It documents the evolution of the Hammer series from its initial major release (v0.94) through various point releases (up to v0.94.10).

### 1. Primary Purpose
The file provides a centralized record of new features, bug fixes, performance improvements, and critical upgrade instructions for the Ceph Hammer release cycle. It acts as a bridge between the raw code changes (commits/PRs) and the end-user/administrator, highlighting significant changes that impact cluster stability and management.

### 2. Key Topics Covered
*   **Major Release Highlights**: High-level features introduced in the Hammer series, such as RADOS performance gains, RGW object versioning, and RBD object maps.
*   **Point Release Changelogs**: Granular lists of fixes for versions v0.94.1 through v0.94.10.
*   **New Tools & Features**:
    *   **Monitor Recovery**: Tools to rebuild the monitor database from OSDs (`ceph-monstore-tool`).
    *   **RGW Resharding**: Introduction of offline bucket index resharding.
    *   **CRUSH Algorithms**: The introduction of the `straw2` bucket type.
*   **Component-Specific Updates**: Specific sections for RADOS (OSD), RADOS Gateway (RGW), RBD (Block Device), and CephFS.
*   **Upgrade Procedures**: Critical warnings and step-by-step instructions for moving from older releases (Firefly/Giant) to Hammer.

### 3. Technical Keywords
*   **Core Components**: `OSD`, `MON` (Monitor), `MDS` (Metadata Server), `RGW` (RADOS Gateway), `Librados`, `Librbd`.
*   **Management Tools**: `radosgw-admin`, `ceph-objectstore-tool`, `ceph-disk`, `ceph-deploy`, `crushtool`.
*   **Configuration & APIs**: `straw2`, `Civetweb`, `S3/Swift API`, `Object Versioning`, `Quotas`, `backfill`, `scrub`, `omap`.
*   **Commands**:
    *   `radosgw-admin bucket reshard`
    *   `ceph osd reweight-by-utilization`
    *   `ceph osd df`
    *   `pg ls-by-primary`

### 4. Target Audience
*   **Storage Administrators**: Who need to plan upgrades, troubleshoot issues, or implement new features like bucket versioning or resharding.
*   **Software Engineers/Developers**: Integrating with Ceph via `librados` or `librbd` who need to understand API changes and deprecated fields.
*   **Site Reliability Engineers (SREs)**: Managing large-scale Ceph clusters who monitor release stability and bug fixes.

### 5. Related Concepts
*   **Data Integrity**: Relates to `scrub`, `deep-scrub`, and `checksum` mechanisms discussed in the notes.
*   **Erasure Coding**: Specifically `SHEC` (Shingled Erasure Coding) and `LRC` (Local Recovery Codes).
*   **Cache Tiering**: Improvements to promotion/demotion logic and hit-set management.
*   **Legacy Support**: References to "Firefly" (v0.80) and "Giant" (v0.87) as preceding versions.

---

### AI Update Triggers
An AI system should update or reference this file when:
1.  **New Point Release**: A new version in the Hammer series (e.g., v0.94.11) is tagged, requiring the addition of a new header and changelog.
2.  **Tooling Changes**: Modifications are made to CLI tools (like `radosgw-admin`) that add or change flags documented here.
3.  **Critical Bug Discovered**: If a regression is found in a Hammer version, this file must be updated with "Upgrading" warnings or recommendations to move to a newer point release.
4.  **Issue/PR Reference**: When a Pull Request or Tracker Issue is merged that is specifically backported to the Hammer branch, it should be appended to the "Notable Changes" section of the relevant point release.