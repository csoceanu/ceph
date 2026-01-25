# API Documentation Index

## Overview
This documentation area serves as the central hub for all Ceph-related application programming interfaces, covering low-level storage cluster access, block devices, object storage, and filesystem management. It provides a mapping between specific Ceph sub-systems and their respective operational APIs, including RESTful interfaces and internal monitor command structures.

## Files Summary
*   **api/index.rst**: Acts as the master entry point and navigational guide, categorizing Ceph APIs by functional domain (RADOS, RBD, CephFS, RGW) and linking to specific protocol implementations.
*   **api/mon_command_api.rst**: A specialized technical manifest that dynamically generates documentation for Ceph Monitor (MON) and Manager (MGR) commands by parsing source headers and Python bindings.

## Code Changes That Would Require Documentation Updates
*   **Command Signature Modifications**: Any changes to command definitions within `src/mon/MonCommands.h` or `src/mgr/MgrCommands.h` (e.g., changing argument types, adding mandatory flags, or renaming commands).
*   **MGR Module Evolution**: Updates to the `ceph-mgr` Python infrastructure in `src/pybind/mgr` that introduce new management capabilities or alter existing administrative interfaces.
*   **Protocol Support Changes**: Introduction of new API versions or deprecation of existing endpoints for S3, Swift, or the Ceph RESTful module.
*   **New Sub-system Integration**: The addition of a new high-level Ceph component (similar to RBD or CephFS) that requires its own API surface or documentation category.
*   **Admin Ops Extensions**: Modifications to the Rados Gateway (RGW) Admin Ops API that change how users, buckets, or quotas are managed programmatically.
*   **Client Library Updates**: Significant version jumps or architectural changes in the native binding libraries for Python, C++, or Java that would invalidate current API references.

## Key Technical Concepts
*   **MON Commands**: Low-level administrative instructions processed by the Ceph Monitor cluster.
*   **MGR Commands**: Orchestration and dashboard-related commands handled by the Ceph Manager daemon.
*   **RADOS Gateway (RGW)**: The object storage interface supporting S3 and Swift protocols.
*   **Admin Ops API**: A specific subset of the RGW API used for administrative tasks like user management and usage tracking.
*   **RESTful API**: The modern, stateful interface for interacting with the Ceph cluster via the Manager module.
*   **Native Bindings**: Language-specific libraries (e.g., `librados`, `librbd`) used to interface directly with Ceph storage.
*   **Python Bindings (pybind)**: The bridge between Ceph's C++ core and the Python-based Manager modules.

## Related Components
*   **Ceph Monitor (mon)**: The source of truth for cluster maps and the processor for `MonCommands.h`.
*   **Ceph Manager (mgr)**: Hosts the RESTful plugin and manages commands defined in `MgrCommands.h`.
*   **RADOS Gateway (rgw)**: The daemon responsible for translating S3/Swift requests into RADOS operations.
*   **librados / librbd / libcephfs**: The underlying client-side libraries that implement the logic for the APIs listed in the index.
*   **Ceph Dashboard**: Utilizes many of the APIs indexed here to provide a visual management interface.