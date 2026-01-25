# RELEASES Documentation Index

## Overview
This documentation area provides a comprehensive history and technical roadmap of the Ceph storage system through its stable and development release notes. It serves as the authoritative record for versioning conventions, upgrade procedures, new feature highlights, and critical bug fixes across the project's evolution.

## Files Summary
*   **releases/tentacle.rst**: Documents the 20th stable release (v20.2.0), highlighting integrated SMB support, the `mgmt-gateway` service, and RADOS FastEC optimizations.
*   **releases/squid.rst**: Covers the 19th release series, including S3 Object Lock enhancements, IAM condition support, and the introduction of first-class User Accounts in RGW.
*   **releases/reef.rst**: Details the 18th release, focusing on mClock scheduler refinements, `try-netlink` mapping for RBD-NBD, and critical BlueStore regression fixes.
*   **releases/quincy.rst**: Documents the 17th release, notably the deprecation of specific RADOS snapshot APIs and enhancements to Python bindings for RBD migration.
*   **releases/pacific.rst**: Covers the 16th release, focusing on localized config dumps and MDS client eviction logic based on session metadata size.
*   **releases/octopus.rst**: Details the 15th release, including the major SnapMapper key format migration and the "volumes" plugin for OpenStack Manila.
*   **releases/nautilus.rst**: Documents the 14th release, highlighting BlueFS buffered I/O performance changes and the "ok-to-stop" OSD command.
*   **releases/mimic.rst**: Covers the 13th release, introducing the MGR-based balancer and MDS cache trimming throttles.
*   **releases/luminous.rst**: Details the 12th release, marking the introduction of the MGR dashboard, BlueStore as the default backend, and the Telemetry module.
*   **releases/kraken.rst**: Documents the 11th release, focusing on snap trimmer throttling and OSD mapping bug fixes.
*   **releases/jewel.rst**: Covers the 10th release, notable for the shift from leveldb to rocksdb for omap data and CephX security hardening.
*   **releases/infernalis.rst**: Details the 9th release, featuring the transition to systemd and running daemons as the 'ceph' user instead of root.
*   **releases/hammer.rst**: Documents the 8th release, introducing SHEC erasure coding and the initial offline bucket resharding tools.
*   **releases/giant.rst**: Covers the 7th release, highlighting Fast-dispatch messenger and the "Degraded vs. Misplaced" health state distinction.
*   **releases/dumpling.rst**: Details the 4th release, focusing on RGW multi-site replication (DR) and xattr spillover logic for OSDs.
*   **releases/emperor.rst**: Documents the 5th release, featuring the introduction of OSD tiering and object copy primitives.
*   **releases/cuttlefish.rst**: Covers the 3rd release, documenting the transition to `ceph-deploy` and unique MDS daemon names.
*   **releases/bobtail.rst**: Details the 2nd release, introducing copy-on-write clones for RBD images and Keystone integration for RGW.
*   **releases/argonaut.rst**: Documents the first stable release of Ceph, establishing the baseline for the OSD capability model and basic RGW bucket limits.
*   **releases/general.rst**: Explains the Project's release cycle (x.y.z numbering), naming conventions (cephalopod species), and EOL (End of Life) calculation logic.
*   **releases/index.rst**: Provides the master navigation tree, Gantt charts for active releases, and links to all archived version pages.

## Code Changes That Would Require Documentation Updates
*   **Release Lifecycle Alterations**: Any changes to the frequency of stable point releases, feature freeze windows, or the support duration (lifetime) of a release.
*   **Deprecation/Removal of Modules**: Removal of MGR modules (e.g., the removal of `zabbix` or `restful` in Tentacle) or APIs (e.g., `get_pool_is_selfmanaged_snaps_mode`).
*   **Configuration Default Changes**: Significant shifts in performance-tuning defaults (e.g., setting `bluefs_buffered_io` to true or changing OSD shard defaults).
*   **CLI Syntax Modifications**: Updates to command structure, new flags (e.g., `--yes-i-really-mean-it` for `max_mds`), or changes in command output formats.
*   **New Daemon/Subsystem Introduction**: Adding new managed services like `mgmt-gateway`, `certmgr`, or `cephfs-proxy`.
*   **On-Disk Format Changes**: Changes to data structures that necessitate offline migration tools or specific upgrade sequencing (e.g., SnapMapper key format changes).
*   **Security Patches**: Integration of CVE fixes that alter default security behaviors (e.g., CephX signature checks or IAM policy evaluation).
*   **New Protocol Support**: Implementation of new frontend protocols (e.g., integrated SMB/Samba support).

## Key Technical Concepts
*   **Versioning**: x.0.z (Dev), x.1.z (RC), x.2.z (Stable).
*   **Backends**: BlueStore, SeaStore (Crimson Tech Preview), FileStore, NewStore.
*   **Placement Groups (PGs)**: `pg_num` adjustments, `upmap` balancing, `pg-upmap-primary`.
*   **Snapshot Management**: Self-managed snaps, SnapMapper, snap-trimming, `pause_cloning`.
*   **RGW/S3 Components**: Bucket resharding, S3 Object Lock, STS (Security Token Service), IAM policies, User Accounts.
*   **RBD Features**: Live migration, fast-diff, mirroring, namespace remapping, NBD streams.
*   **MDS/CephFS Concepts**: Subvolumes, `max_mds`, session metadata thresholds, directory entry normalization.
*   **Networking/Messenger**: msgr2, TLS termination, `mgmt-gateway`, IPv6 subnetting.
*   **Scheduler**: mClock (HDD/SSD IOPS thresholds).

## Related Components
*   **RADOS**: The foundational object store (OSDs, Monitors).
*   **Cephadm**: The orchestrator for service deployment and certificate management.
*   **MGR (Manager)**: Hosts always-on modules, balancing logic, and the Dashboard.
*   **RGW (RADOS Gateway)**: Provides S3/Swift compatible object storage.
*   **RBD (RADOS Block Device)**: Provides thin-provisioned block storage.
*   **CephFS**: The distributed POSIX-compliant filesystem.
*   **Crimson**: The next-generation high-performance OSD implementation.