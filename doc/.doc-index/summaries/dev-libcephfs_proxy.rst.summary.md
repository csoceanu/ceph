This documentation file outlines the design and architecture of the **libcephfs proxy**, a solution designed to solve memory management issues when multiple processes on the same host connect to CephFS using the standard `libcephfs.so` library.

### 1. Primary Purpose
The file documents a "proxy" architecture that centralizes CephFS connections through a single daemon. The goal is to **prevent OOM (Out of Memory) kills** by consolidating individual process caches into a single, shared cache managed by a central daemon, rather than allowing unbounded memory consumption by multiple independent `libcephfs` instances.

### 2. Key Topics Covered
*   **The Problem**: Memory fragmentation and lack of coordination between multiple `libcephfs` instances leads to excessive memory usage.
*   **The Architecture**: 
    *   `libcephfs_proxy.so`: A shim library that mimics the `libcephfs` API but forwards requests via a UNIX socket.
    *   `libcephfsd`: A background daemon that manages the actual `libcephfs.so` connections and handles request execution.
*   **Connection Sharing**: Logic for determining when two processes can share a single Ceph mount (based on identical configuration checksums).
*   **Feature Negotiation**: A protocol allowing the client and server to agree on capabilities and versions.
*   **Virtualization of Mounts**: How the daemon "fakes" root directories and Current Working Directories (CWD) so multiple clients can share one mount without interfering with each other.
*   **Special Function Overrides**: Detailed implementation notes for standard CephFS functions that require special handling in a proxied environment (e.g., `ceph_mount`, `ceph_ll_lookup`).

### 3. Technical Keywords
*   **Libraries/Binaries**: `libcephfs_proxy.so`, `libcephfs.so`, `libcephfsd`.
*   **Communication**: UNIX socket, Serialization (custom, with potential for Cap’n Proto), `LD_PRELOAD`.
*   **Ceph APIs/Functions**: `ceph_mount_info`, `ceph_ll_lookup`, `ceph_ll_walk`, `ceph_mount`, `ceph_unmount`, `ceph_conf_set`, `ceph_conf_read_file`, `AT_FDCWD`.
*   **Concepts**: Feature negotiation bitmap, Virtual Root Inode, Metadata Caching, Checksum-based config matching.

### 4. Target Audience
*   **Ceph Developers**: Specifically those working on the CephFS client side or performance optimization.
*   **System Architects**: Designing high-density environments (like container hosts or Samba servers) where many processes need CephFS access.
*   **Maintainers**: Those managing the `libcephfsd` systemd units or integration with `cephadm`.

### 5. Related Concepts
*   **Samba VFS CephFS**: The proxy is specifically designed to support the subset of functions used by Samba.
*   **CephFS Quotas/Caps**: The daemon must handle capabilities and invalidations to maintain cache consistency.
*   **Memory Management**: Specifically the OOM killer and process-level memory limits.
*   **Virtualization/Namespacing**: The way `libcephfsd` handles `chdir` and root paths is essentially a form of filesystem namespacing.

---

### Maintenance Triggers: When to update this file
This documentation should be updated if any of the following occur in the codebase:
1.  **API Surface Changes**: If new `libcephfs.so` functions are added that must be supported by the proxy.
2.  **Protocol Changes**: If the serialization method moves from the custom implementation to Cap'n Proto.
3.  **Config Logic**: If the criteria for "identical configurations" (used for sharing mounts) change.
4.  **Caching Strategy**: When the currently "planned" metadata caching for `libcephfs_proxy.so` is actually implemented.
5.  **Path Resolution Logic**: If changes are made to how `..`, symbolic links, or `AT_FDCWD` are handled in the daemon.