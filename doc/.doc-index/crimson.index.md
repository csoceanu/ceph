# CRIMSON Documentation Index

## Overview
This documentation covers **Crimson**, the next-generation Ceph Object Storage Daemon (`crimson-osd`). It details the project's architecture, deployment via `cephadm`, CPU resource management, and the various storage backends (SeaStore, BlueStore, CyanStore) designed to leverage high-performance NVMe and modern networking through the Seastar framework.

## Files Summary
*   **crimson/crimson.rst**: The primary landing page for Crimson; it provides deployment instructions, configuration requirements for CPU sharding, details on storage backends, and methods for metrics collection.

## Code Changes That Would Require Documentation Updates
*   **Seastar Integration Changes**: Any updates to how `crimson-osd` utilizes the Seastar reactor or changes to the threading model.
*   **New Storage Backends**: Addition of new native or non-native backends beyond SeaStore, CyanStore, BlueStore, and MemStore.
*   **Configuration Schema Updates**: Changes to mandatory parameters like `crimson_cpu_num`, `crimson_cpu_set`, or `crimson_bluestore_num_threads`.
*   **Deployment Workflow**: Modifications to `cephadm` bootstrap flags, container image naming conventions (e.g., changing the `crimson-release` tag logic), or experimental feature flag requirements.
*   **Feature Graduation**: When Crimson moves from "Tech Preview" to "General Availability," or when SeaStore moves out of its early stages.
*   **CLI/Admin Socket Commands**: Adding or deprecating commands under `ceph tell osd.X` related to metrics or OSD management.
*   **Compatibility Changes**: Updates to the `allow_crimson` OSDMap flag logic or pool-level restrictions for Crimson OSDs.
*   **Metrics & Monitoring**: Changes to the Prometheus exporter integration or the structure of `MPGStats` reported to the Manager.

## Key Technical Concepts
*   **SeaStore**: Native Crimson storage backend optimized for NVMe.
*   **CyanStore**: Native, zero-storage backend used for performance benchmarking/overhead measurement.
*   **Alien Threads**: Worker threads not managed by Seastar, used for non-native backends like BlueStore.
*   **Reactor Sharding**: The asynchronous, shard-based architecture where each reactor runs on a dedicated CPU core.
*   **CPU Pinning**: The `crimson_cpu_set` configuration for manual core assignment.
*   **Crimson Pools**: Specific OSD pools (flagged `crimson`) that restrict operations to those supported by the new OSD architecture.
*   **Experimental Flags**: `enable_experimental_unrecoverable_data_corrupting_features` and `osd_pool_default_crimson`.
*   **MPGStats**: The message format for reporting stats to the Ceph Manager.

## Related Components
*   **Seastar**: The underlying high-performance asynchronous engine.
*   **DPDK/SPDK**: Frameworks leveraged for fast network and storage I/O.
*   **cephadm**: The deployment and orchestration tool used to manage Crimson containers.
*   **BlueStore**: The legacy storage backend supported via a thread-pool proxy.
*   **Ceph Manager (mgr)**: The component responsible for collecting and displaying Crimson's reported PG and OSD stats.
*   **OSDMap**: The cluster map that must be updated with the `allow-crimson` flag to permit Crimson OSDs to boot.