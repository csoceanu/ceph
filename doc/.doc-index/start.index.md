# START Documentation Index

## Overview
The "Start" documentation folder serves as the primary entry point for new users and contributors to the Ceph ecosystem. It covers fundamental architectural concepts, deployment prerequisites (hardware and OS), quick-start procedures for block storage, and the administrative workflows required to contribute to the project's documentation and source code.

## Files Summary
*   **beginners-guide.rst**: Provides a high-level conceptual overview of Ceph’s distributed architecture and a step-by-step guide for setting up a `vstart` development environment.
*   **hardware-recommendations.rst**: Details hardware selection criteria, focusing on CPU/RAM requirements per daemon type and the performance trade-offs between HDD and SSD storage.
*   **os-recommendations.rst**: Outlines supported Linux distributions, container host compatibility, and kernel version requirements for various Ceph releases.
*   **index.rst**: The root landing page that defines core cluster daemons (Mon, Mgr, OSD, MDS, RGW) and maps out the introductory documentation path.
*   **documenting-ceph.rst**: A comprehensive guide for contributors on how to modify, build, and submit documentation changes using Git and Sphinx.
*   **quick-rbd.rst**: A streamlined tutorial for initializing pools and mapping RADOS Block Devices (RBD) to a client node.
*   **get-involved.rst**: A directory of community resources, including mailing lists, IRC/Slack channels, and bug tracking links.

## Code Changes That Would Require Documentation Updates
*   **Daemon Resource Consumption**: Changes to default memory targets (e.g., `osd_memory_target`), threading models in MDS, or CPU scaling logic.
*   **Storage Backend Evolution**: Updates to BlueStore, the introduction of new backends, or changes in how WAL/DB devices are managed.
*   **Architecture Support**: Adding support for new CPU architectures (e.g., expanded ARM support for SMB services) or retiring old ones.
*   **OS/Kernel Compatibility**: Dropping support for older Linux distributions, requiring newer kernel versions for RBD/CephFS features, or changes in Windows client status.
*   **Build System Modifications**: Changes to `do_cmake.sh`, `admin/build-doc`, or Python dependencies (Sphinx, Doxygen) that affect how developers build the project.
*   **Cluster Orchestration**: New default behaviors for CRUSH maps, changes in quorum requirements for Monitors, or new mandatory Manager modules.
*   **Feature Availability**: Introduction of new stable releases (requiring updates to the platform compatibility matrices) or deprecation of legacy components like Filestore.
*   **CLI/API Syntax**: Modifications to `rbd` command-line arguments, `ceph` pool initialization workflows, or `vstart.sh` flags.

## Key Technical Concepts
*   **Daemons**: `ceph-mon` (Monitor), `ceph-osd` (Object Storage Daemon), `ceph-mgr` (Manager), `ceph-mds` (Metadata Server), `ceph-radosgw` (RGW).
*   **Storage Backends**: BlueStore, Filestore, WAL (Write Ahead Log), DB (Metadata database).
*   **Configuration Options**: `osd_memory_target`, `osd_memory_target_autotune`, `mon_osd_cache_size`, `mds_cache_memory_limit`.
*   **Data Placement**: CRUSH algorithm, Placement Groups (PGs), Pools (Replicated vs. Erasure Coded).
*   **Client Interfaces**: RBD (Block), CephFS (File), RADOS (Object).
*   **Development Tools**: `vstart.sh`, `build-doc`, `serve-doc`, `ninja`, `cmake`, `fio`.
*   **Performance Metrics**: IOPS per core, DWPD (Drive Writes Per Day), PLP (Power Loss Protection).

## Related Components
*   **RADOS**: The underlying reliable autonomic distributed object store.
*   **RBD**: The block storage interface.
*   **CephFS**: The POSIX-compliant file system.
*   **Ceph Manager Dashboard**: The web-based management GUI.
*   **Git/GitHub**: Used for the "Fork and Pull" contribution workflow.
*   **Linux Kernel**: Specifically the RBD and CephFS kernel modules.
*   **Sphinx/reStructuredText**: The documentation processing engine and syntax.