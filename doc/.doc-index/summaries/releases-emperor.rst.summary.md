This documentation file, `releases/emperor.rst`, serves as the official release notes and upgrade guide for **Emperor (v0.72.x)**, the 5th major stable release of the Ceph distributed storage system.

### 1. Primary Purpose
The file tracks the lifecycle of the Emperor release, documenting new features, security fixes, bug fixes, and critical upgrade procedures. It serves as a historical and technical record for administrators moving from previous versions (like Dumpling or Cuttlefish) to Emperor.

### 2. Key Topics Covered
*   **Release Versions**: Detailed logs for v0.72 (major release) through v0.72.3 (point releases).
*   **Upgrade Procedures**: Critical sequencing for upgrading Monitors (MONs), OSDs, and Metadata Servers (MDS).
*   **Breaking Changes**: Discontinuation of TMAP to OMAP auto-conversion and changes to default snapshot behaviors in CephFS.
*   **Component Updates**:
    *   **RADOS/OSD**: Introduction of erasure coding (EC) infrastructure, tiering support, and performance optimizations (crc32c).
    *   **RGW (RADOS Gateway)**: Multi-datacenter replication, bucket quotas, and Keystone integration for S3 tokens.
    *   **MDS/CephFS**: Stability improvements and the introduction of `allow_snaps` toggles.
    *   **Monitors**: Improved health reporting and per-pool statistics.
*   **Tooling**: Introduction of `ceph_filestore_tool` (for OSD repair) and `ceph-post-file` (for log sharing).

### 3. Technical Keywords
*   **APIs/Methods**: `librados::Rados::pool_create_async`, `get_version64()`, `COPY_FROM`, `COPY_GET`, `tmap`, `omap`.
*   **Configuration Options**: `osd pool set`, `hashpspool`, `osd heartbeat min healthy ratio`, `allow_snaps`.
*   **Commands**: `ceph auth export`, `ceph osd crush add/set`, `ceph-fuse`, `ceph_filestore_tool`, `radosgw-agent`.
*   **Security/Permissions**: `rx` caps, `mon` capability bits, Keystone S3 tokens.
*   **Technical Concepts**: Erasure Coding (EC), Cache Tiering, Bloom Filters, CRUSH maps, Paxos, Quorum.

### 4. Target Audience
*   **Storage Administrators**: Responsible for maintaining, patching, and upgrading Ceph clusters.
*   **Software Engineers/Developers**: Specifically those using `librados` or writing OSD classes.
*   **DevOps/SREs**: Managing automated deployments via `sysvinit`, `upstart`, or package management (RPM/DEB).

### 5. Related Concepts
*   **Upstream Releases**: Directly follows **Dumpling (v0.67)** and precedes **Firefly**.
*   **External Integrations**: OpenStack Keystone (identity), ZFS on Linux (experimental backend), and Wireshark (packet analysis).
*   **Data Integrity**: CRC32C optimizations and opportunistic CRC checking.

---

### AI Update Trigger Guide
*Update this file if any of the following code changes occur:*
*   **CLI Syntax Changes**: If a command like `ceph osd pool set` changes its argument types (e.g., string to integer).
*   **Librados API Deprecations**: If a method like `get_version()` is deprecated or its return bit-width changes.
*   **New Security Requirements**: If monitor capability requirements (caps) for existing commands are tightened.
*   **Breaking Upgrade Logic**: If a new version requires a specific daemon restart order or manual data repair (e.g., via `ceph_filestore_tool`).
*   **Component Defaults**: If default paths for admin sockets or logging behavior change.