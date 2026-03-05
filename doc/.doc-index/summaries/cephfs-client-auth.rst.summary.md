This documentation file, `cephfs/client-auth.rst`, serves as the definitive guide for managing and restricting client access within the Ceph File System (CephFS). It focuses on the principle of "least privilege" by defining how to use Ceph’s authentication capabilities to limit client authority.

### 1. Primary Purpose
The file documents the **CephFS Client Capability system**, specifically how administrators can restrict client access based on file system paths, specific metadata operations (layouts, quotas, snapshots), network locations, and multi-filesystem visibility.

### 2. Key Topics Covered
*   **Path Restriction**: Limiting clients to specific subdirectories within the CephFS hierarchy.
*   **Operational Restrictions (Flags)**:
    *   `p` flag: Controlling the ability to modify file layouts and quotas (extended attributes).
    *   `s` flag: Controlling the ability to create or delete snapshots.
*   **Network Restriction**: Using CIDR notation to restrict access based on the client's source IP.
*   **Multi-FS Isolation**: Restricting clients so they can only "see" and communicate with specific file systems in a multi-filesystem Ceph cluster.
*   **Root Squash**: Protecting against accidental mass-deletion by preventing `uid=0` from performing write/delete operations.
*   **Capability Management**: How the `fs authorize` command behaves when creating, updating, or appending permissions to existing users.

### 3. Technical Keywords
*   **Commands**: `ceph fs authorize`, `ceph auth get`, `ceph-fuse`, `ceph fs ls`.
*   **Capability Flags**: `r` (read), `w` (write), `p` (layout/quota), `s` (snapshots).
*   **Configuration Options**: `client quota df`, `root_squash`.
*   **Components**: MDS (Metadata Server), MON (Monitor), OSD (Object Storage Daemon), RADOS namespaces.
*   **Syntax Elements**: `fsname`, `path=`, `network/prefix`.

### 4. Target Audience
*   **Storage Administrators**: Who need to provision access for different users or applications.
*   **Security Engineers**: Looking to implement multi-tenancy and data isolation.
*   **System Integrators**: Using `ceph-fuse` or the kernel client to mount restricted sub-directories.

### 5. Related Concepts
*   **RADOS Namespaces**: Crucial for full data-level isolation, as path restriction only affects the metadata tree.
*   **File Layouts**: Used to map specific directories to specific RADOS pools/namespaces.
*   **CephX Authentication**: The underlying framework that stores and validates these keys and capabilities.
*   **Quotas**: Directly related to the `p` flag and `df` reporting behavior.

---

### AI Update Trigger Guide
This file should be updated if any of the following code-level changes occur:
1.  **CLI Changes**: If the syntax for `ceph fs authorize` changes or new arguments are added.
2.  **New Capability Flags**: If a new permission flag (similar to `p` or `s`) is introduced to the MDS or Monitor.
3.  **Default Behavior Shifts**: If the default behavior of `fs authorize` changes regarding how it handles existing keys (e.g., the transition noted in the Reef release).
4.  **Reporting Logic**: If the way `df` or free space is calculated for sub-directory mounts is modified in the client-side code.
5.  **Security Features**: If new safety mechanisms (like a variation of `root_squash`) are implemented in the MDS.
6.  **Multi-FS Logic**: If the way Monitors filter the file system list for clients is overhauled.