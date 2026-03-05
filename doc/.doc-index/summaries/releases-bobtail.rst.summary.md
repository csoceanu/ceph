This documentation file, `releases/bobtail.rst`, serves as the **Release Notes and Change Log** for the "Bobtail" release series (v0.56.x) of the Ceph storage system. It tracks the evolution of the second stable release of Ceph, documenting new features, critical bug fixes, and manual upgrade procedures.

### 1. Primary Purpose
The file documents the lifecycle of the Ceph **Bobtail (v0.56)** release. It provides a historical record of updates from the initial stable release through subsequent point releases (v0.56.1 to v0.56.7). Its primary function is to inform administrators of **breaking changes, security fixes, and mandatory upgrade steps** required to maintain cluster health.

### 2. Key Topics Covered
*   **Upgrade Instructions**: Critical steps for moving from "Argonaut" (v0.48) to "Bobtail," and specific warnings about daemon restart orders (Monitors → OSDs → RGW).
*   **Component Updates**:
    *   **OSD (Object Storage Daemon)**: Deep scrubbing, threading improvements, and peering refactors.
    *   **RBD (RADOS Block Device)**: Introduction of copy-on-write clones, image locking, and client-side caching.
    *   **RGW (RADOS Gateway)**: OpenStack Keystone integration, usage logging for billing, and S3/Swift API parity.
    *   **Monitors**: Changes to quorum protocols and CRUSH map defaults.
*   **Breaking Compatibility Changes**: Changes in command-line syntax (e.g., `osd create` requiring UUIDs), default authentication (Cephx enabled by default), and CRUSH map root types.
*   **System Integration**: New support for RPM packages, XFS/ext4 automation, and `systemd`/`upstart` job files.

### 3. Technical Keywords
*   **Core Daemons**: `ceph-osd`, `ceph-mon`, `ceph-mds`, `radosgw` (RGW).
*   **APIs/Protocols**: S3, Swift, Keystone, RADOS, `librados`, `librbd`.
*   **Configuration Options**: `auth cluster required`, `osd scrub min interval`, `mon max pool pg num`, `rgw enable ops log`.
*   **Storage Concepts**: CRUSH maps, Placement Groups (PGs), Deep Scrubbing, Copy-on-Write (CoW) Clones, OMAP, Quorum.
*   **Commands**: `ceph osd tree`, `rbd lock list`, `ceph-disk-prepare`, `mkcephfs`, `rados bench`.

### 4. Target Audience
*   **Storage Administrators**: Managing production Ceph clusters and planning version upgrades.
*   **DevOps Engineers**: Automating Ceph deployments using Chef, puppet, or `ceph-deploy`.
*   **Software Developers**: Utilizing `librados` or `librbd` bindings (Python/Java) to interact with Ceph storage.
*   **Distro Maintainers**: Packaging Ceph for Debian, Fedora, or RHEL/CentOS.

### 5. Related Concepts
*   **Upstream Releases**: Relates directly to **Argonaut** (predecessor) and **Cuttlefish** (successor).
*   **Filesystem Support**: Closely tied to Linux kernel features like `fdatasync`, `syncfs(2)`, and XFS/ext4/btrfs stability.
*   **Cloud Integration**: Relates to OpenStack (via Keystone and RGW) and QEMU/KVM (via `librbd`).

---

### AI Update Trigger Guide
*   **Update this file when**:
    1.  A new point release (v0.56.x) is tagged.
    2.  A critical bug (data corruption, hang, or security vulnerability) is backported to the Bobtail branch.
    3.  The CLI syntax for a core command is modified in a way that affects backward compatibility.
    4.  The on-disk format for a component (like the MDS or OSD) changes.
    5.  Default configuration values (like `cephx` authentication) are altered.