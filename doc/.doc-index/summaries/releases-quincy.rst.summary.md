This documentation file, `releases/quincy.rst`, serves as the official release notes and lifecycle log for **Ceph Quincy (v17.2.x)**, the 17th stable release of the Ceph distributed storage system.

### 1. Primary Purpose
The file documents the evolution of the Quincy release series. It provides a chronological record of stable releases, hotfixes, and backports. Its main goal is to inform users of new features, critical bug fixes (including security vulnerabilities), breaking changes, and upgrade procedures from previous versions (Octopus and Pacific).

### 2. Key Topics Covered
*   **Release Lifecycle & Versions**: Detailed entries for versions 17.2.0 through 17.2.9.
*   **Infrastructure & OS Support**: Migration from CentOS 8 to CentOS 9; EOL (End-of-Life) declarations for specific distributions; and OpenSSL 3.5.0 compatibility.
*   **Subsystem Updates**:
    *   **RADOS**: Deprecation of Filestore, introduction of the `mclock_scheduler` as the default for BlueStore, and PG autoscaler enhancements.
    *   **RGW (Object Storage)**: Rate limiting, multi-site replication fixes for encrypted objects, and S3 API adjustments.
    *   **RBD (Block Storage)**: Persistent write-log caching, `rbd-nbd` enhancements, and performance improvements for QEMU live disk sync.
    *   **CephFS (File System)**: Client eviction logic, disaster recovery procedures, and session metadata management.
    *   **Cephadm & Dashboard**: Automated upgrade workflows, SNMP support, and host management via the web UI.
*   **Upgrade Procedures**: Step-by-step guides for both `cephadm`-managed and non-managed clusters.

### 3. Technical Keywords
*   **Storage Engines**: BlueStore, Filestore, RocksDB, SnapMapper.
*   **APIs/Interfaces**: librados, librbd, S3, Swift, CSI, Python/C++ bindings.
*   **Performance/Logic**: mClock, QoS, backfill/recovery, pg_autoscaler, fast-diff.
*   **Management/Orchestration**: `cephadm`, `ceph orch`, `ceph-mgr`, `ceph-volume`.
*   **Security/Vulnerabilities**: CVE-2022-0670, CVE-2023-43040, CephX.
*   **Configuration Options**: `osd_op_queue`, `bluestore_zero_block_detection`, `noautoscale`.

### 4. Target Audience
*   **Storage Administrators**: Responsible for maintaining, upgrading, and troubleshooting Ceph clusters.
*   **System Architects**: Designing storage backends for OpenStack, Kubernetes (Rook), or private clouds.
*   **DevOps/SREs**: Using `cephadm` or CLI tools to automate storage deployments.
*   **Software Developers**: Utilizing Ceph APIs (librados, RBD) who need to track deprecated methods or performance changes.

### 5. Related Concepts
*   **Post-Quincy Releases**: Reef and Squid (referenced in the build/test matrix).
*   **Underlying Infrastructure**: Linux Kernels (5.14+ for specific RBD features), SSL/TLS libraries, and container runtimes (Podman/Docker).
*   **Ecosystem Integrations**: OpenStack Manila (CephFS shares), Rook (Kubernetes orchestration), and CERN (community package provider for EL8).

### Maintenance Trigger: When to update this file
This file is a **living document** and must be updated by an AI or human maintainer when:
1.  **New Version Tags**: A new minor release (e.g., v17.2.10) is finalized.
2.  **Backports**: Pull requests (PRs) tagged with `[quincy]` are merged.
3.  **Critical Regressions**: A bug is identified in a Quincy version that requires a "Notable Change" or "Hotfix" warning (e.g., the BlueStore regression in 17.2.8).
4.  **Security Patches**: A CVE is addressed that affects the Quincy series.
5.  **Platform Changes**: Support for a new OS is added or a specific distribution version reaches EOL for Ceph builds.