This documentation file, `dev/crimson/index.rst`, serves as the **primary developer portal and landing page for Crimson**, the next-generation, Seastar-based OSD (Object Storage Daemon) for the Ceph distributed storage system.

### 1. Primary Purpose
The file provides an entry point for developers to build, run, debug, and profile Crimson. It acts as a high-level technical guide for setting up a development environment specifically for the Crimson-OSD and its various storage backends.

### 2. Key Topics Covered
*   **Build Instructions**: Procedures for enabling Crimson at compile-time using CMake flags.
*   **Cluster Orchestration**: Instructions for using `vstart.sh` to deploy a local Crimson-based cluster.
*   **Object Store Backends**: Configuration for different backends including SeaStore, CyanStore, and "alienized" versions of BlueStore and MemStore.
*   **System Architecture & Logging**: Explanation of Crimson’s non-daemonizing nature (due to Seastar’s threading model) and the mapping of Ceph log levels to Seastar severity levels.
*   **Performance Profiling**: Specialized workflows using `fio` via `crimson-store-nbd` and the Ceph Benchmarking Tool (CBT).
*   **Debugging**: Tooling for crash analysis, specifically converting hex backtraces to human-readable code using `seastar-addr2line`.

### 3. Technical Keywords
*   **Core Components**: `crimson-osd`, `Seastar`, `SeaStore`, `CyanStore`, `FuturizedStore`, `crimson-store-nbd`.
*   **Configuration/Flags**: `WITH_CRIMSON=ON`, `--crimson`, `--seastore`, `--bluestore`, `--smp` (core count), `--osd-args`.
*   **Tools**: `vstart.sh`, `fio`, `libnbd`, `cbt`, `gdb`, `seastar-addr2line`, `ASan` (AddressSanitizer).
*   **Logging Levels**: `derr`, `error`, `warn`, `info`, `debug`, `trace`.

### 4. Target Audience
*   **Ceph Core Developers**: Those contributing to the Crimson-OSD codebase.
*   **Performance Engineers**: Individuals benchmarking the next-generation Ceph storage layer.
*   **System Integrators**: Developers testing SeaStore or other specialized backend storage engines.

### 5. Related Concepts
*   **Seastar Framework**: The asynchronous, shared-nothing programming model Crimson is built upon.
*   **SeaStore**: The primary high-performance backend designed specifically for Crimson.
*   **Ceph OSD**: The traditional (classic) OSD which Crimson is designed to eventually replace or supplement.
*   **NBD (Network Block Device)**: Used to export Crimson internal stores for external testing.

---

### Update Triggers for AI Maintenance
This file should be updated if any of the following changes occur in the codebase:
1.  **Build System Changes**: If the CMake flags (e.g., `WITH_CRIMSON`) or dependency requirements (e.g., `libnbd`) change.
2.  **CLI Argument Updates**: If `vstart.sh` or `crimson-osd` introduce new mandatory flags or deprecate existing ones (especially storage backend flags like `--seastore`).
3.  **Logging Logic**: If the mapping between Ceph's `dout` levels and Seastar's severity levels is modified.
4.  **Backend Evolution**: If a new storage backend (similar to CyanStore/SeaStore) is added or if the `FuturizedStore` interface changes.
5.  **Tooling/Scripts**: If the path to Seastar scripts (like `seastar-addr2line`) or Ceph-specific debug scripts changes.
6.  **Structural Documentation**: If new sub-modules (like `PoseidonStore` or `BackfillMachine`) are added to the Crimson directory, the `toctree` at the bottom must be updated.