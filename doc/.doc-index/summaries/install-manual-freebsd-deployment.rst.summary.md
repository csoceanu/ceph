This documentation provides a technical guide for the manual installation and configuration of a Ceph Storage Cluster on the **FreeBSD** operating system. It serves as a platform-specific adaptation of the standard manual deployment process, emphasizing FreeBSD’s unique filesystem hierarchy and its reliance on **ZFS** and **rc.d** (bsdrc) init scripts.

### 1. Primary Purpose
The file documents the step-by-step procedure to manually bootstrap a Ceph cluster on FreeBSD. It focuses on the divergence from Linux-based deployments, specifically regarding disk preparation with ZFS, configuration file paths, and service management.

### 2. Key Topics Covered
*   **Disk Layout & ZFS Integration**: Using `gpart` and `zpool` to create OSD storage, including the use of SSDs for ZFS L2ARC (cache) and ZIL (log).
*   **FreeBSD-Specific Pathing**: Adjusting for `/usr/local` defaults and establishing symlinks for configuration consistency.
*   **Monitor (MON) Bootstrapping**: Generating the cluster FSID, monitor maps, and keyrings (Admin and Monitor).
*   **OSD Deployment**: The "Long Form" process of creating OSDs, initializing data directories, and manually updating the CRUSH map.
*   **MDS Setup**: Adding Metadata Servers for CephFS support.
*   **Service Management**: Enabling and starting Ceph daemons using FreeBSD `rc.conf` and `service` commands.

### 3. Technical Keywords
*   **Storage/Filesystem**: `ZFS`, `UFS2`, `L2ARC`, `ZIL`, `xattribs`, `gpart`, `zpool`.
*   **Ceph Daemons**: `ceph-mon`, `ceph-osd`, `ceph-mds`, `ceph-mgr`.
*   **CLI Tools**: `ceph-authtool`, `monmaptool`, `uuidgen`, `ceph osd crush`.
*   **Configuration Files**: `ceph.conf`, `/usr/local/etc/ceph/`, `/etc/rc.conf`.
*   **FreeBSD Init**: `bsdrc`, `rc.d`, `service ceph start`.
*   **Ceph Architecture**: `fsid`, `monmap`, `keyring`, `CRUSH map`, `placement groups (PGs)`.

### 4. Target Audience
*   **System Administrators**: Specifically those operating in FreeBSD environments.
*   **DevOps Engineers**: Building automation scripts for Ceph where standard tools (like `cephadm` or `rook`) might not be applicable or supported for FreeBSD.
*   **Ceph Contributors**: Looking to understand the nuances of non-Linux porting and manual deployment logic.

### 5. Related Concepts
*   **RADOS**: The underlying autonomous storage layer.
*   **CephFS**: The distributed file system (related via MDS setup).
*   **CRUSH Algorithm**: The mechanism used for data placement and cluster topology.
*   **Manual Deployment (Linux)**: The parent documentation this file mimics.

---

### Update Triggers for AI Systems
This file should be updated if any of the following code or environmental changes occur:
1.  **Pathing Changes**: Changes in the FreeBSD ports tree regarding where Ceph binaries or sample configs are installed (e.g., changes to `PREFIX`).
2.  **Command Deprecations**: If `ceph-osd` or `ceph-mon` flags (like `--mkfs` or `--mkkey`) are modified or replaced by newer orchestration-agnostic tools.
3.  **Init System Migration**: If FreeBSD shifts away from `rc.d` or if the `bsdrc` scripts within the Ceph repository are renamed/refactored.
4.  **ZFS Integration**: If Ceph introduces native ZFS features or if the recommendation to use ZFS as the underlying OSD storage changes (e.g., introduction of BlueStore-specific requirements for FreeBSD).
5.  **Cluster Naming**: If "vanity names" (deprecated in this doc) are officially removed from the codebase.
6.  **Authentication/Caps**: Changes to the `cephx` capability syntax or required permissions for `client.admin`.