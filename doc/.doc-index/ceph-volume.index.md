# CEPH-VOLUME Documentation Index

## Overview
The `ceph-volume` documentation provides a comprehensive guide to the modular tool used for deploying and managing Ceph OSDs (Object Storage Daemons). It details the transition from the deprecated `ceph-disk` utility to a more robust, LVM-based architecture that supports various device technologies, automation via drive-groups, and integration with systemd for lifecycle management.

## Files Summary
*   **intro.rst**: Explains the rationale for `ceph-volume`, its modular design, and the technical limitations of the legacy `ceph-disk` tool it replaces.
*   **index.rst**: The main entry point and sitemap for the documentation, covering migration paths and high-level subcommand categories.
*   **systemd.rst**: Describes the integration between `ceph-volume` and systemd, specifically the naming conventions for unit files and retry logic.
*   **inventory.rst**: Documents the subcommand used to discover and report hardware information and disk usability for a host.
*   **drive-group.rst**: Explains how to pass high-level drive group specifications (JSON) to automate OSD deployment.
*   **lvm/index.rst**: Acts as the landing page for the LVM-specific implementation and internal workflows.
*   **lvm/prepare.rst**: Detailed guide on the first stage of OSD deployment, covering metadata tagging for BlueStore and Filestore.
*   **lvm/activate.rst**: Describes the second stage of OSD deployment, focusing on mounting devices and enabling systemd units via LVM tags.
*   **lvm/create.rst**: Documents the single-step convenience command that combines the "prepare" and "activate" workflows.
*   **lvm/batch.rst**: Detailed instructions on deploying multiple OSDs simultaneously, including automatic disk sorting and sizing rules.
*   **lvm/list.rst**: Explains how to query and report on devices currently associated with Ceph via LVM metadata.
*   **lvm/zap.rst**: Provides instructions for cleaning and destroying partitions or logical volumes to make them reusable.
*   **lvm/migrate.rst**: Describes moving BlueFS data between devices, such as migrating metadata from slow HDDs to fast SSDs.
*   **lvm/newdb.rst** & **newwal.rst**: Guides for attaching new DB or WAL logical volumes to existing OSDs.
*   **lvm/scan.rst**: Placeholder for the (internal/upcoming) LVM volume discovery functionality.
*   **lvm/systemd.rst**: Deep dive into how the LVM subcommand interacts with systemd triggers and OSD mounting.
*   **lvm/encryption.rst**: Details the dmcrypt/LUKS workflow, including key management and TPM2 enrollment.
*   **simple/index.rst**: Overview of the `simple` subcommand for managing legacy OSDs previously deployed by `ceph-disk`.
*   **simple/scan.rst**: Explains how to capture metadata from existing OSDs and persist it as JSON for management takeover.
*   **simple/activate.rst**: Procedures for activating OSDs managed by the `simple` workflow, including disabling `ceph-disk` triggers.
*   **simple/systemd.rst**: Describes the systemd integration specifically for the `simple` subcommand metadata.
*   **zfs/index.rst** & **zfs/inventory.rst**: Documents the ZFS-based OSD deployment specifically for FreeBSD platforms.

## Code Changes That Would Require Documentation Updates
*   **New Subcommands or Plugins**: Adding support for new storage technologies (e.g., direct NVMe/SPDK support).
*   **Changes to LVM Tagging**: Altering the metadata keys (e.g., `ceph.osd_id`) or the way tags are stored/queried.
*   **CLI Argument Modifications**: Adding or deprecating flags in `prepare`, `batch`, `zap`, or `inventory`.
*   **Systemd Unit Logic**: Changes to the naming convention of `ceph-volume@` units or the OSD mounting path conventions.
*   **Default Backend Transitions**: If BlueStore is replaced or a new default objectstore (like SeaStore) is introduced.
*   **Encryption Workflows**: Updating LUKS versions supported, changing key retrieval methods from the Monitor, or modifying TPM2 integration.
*   **Batch Sizing Algorithms**: Adjusting how `batch` calculates default DB/WAL sizes or how it auto-sorts rotational vs. non-rotational media.
*   **Migration/Compatibility**: Changes to the migration path from `ceph-disk` or the lifecycle of the `simple` subcommand.
*   **Platform Support**: Expanding ZFS/GEOM support beyond FreeBSD or modifying Linux-specific LVM filter logic.

## Key Technical Concepts
*   **LVM Tags**: Metadata stored on logical volumes used for device discovery.
*   **dmcrypt/LUKS2/TPM2**: Technologies used for OSD encryption at rest.
*   **BlueStore**: The primary OSD backend (utilizing `block`, `block.db`, and `block.wal` devices).
*   **Systemd Oneshot**: The service type used for the activation and trigger process.
*   **PARTUUID**: The unique identifier for GPT partitions used in device discovery.
*   **Drive Groups**: High-level JSON specifications for declarative OSD deployment.
*   **Idempotency**: The ability to run commands (like `activate` or `batch`) multiple times without side effects.
*   **BlueFS Spillover**: The scenario where metadata overflows to slow devices, requiring migration.

## Related Components
*   **LVM2**: The underlying Linux volume management system.
*   **Ceph Monitors (MON)**: Used for OSD ID allocation and encryption key storage.
*   **systemd**: Manages the lifecycle and boot-time startup of OSDs.
*   **UDEV**: Legacy interaction component (largely avoided by `ceph-volume` but relevant to migration).
*   **ceph-osd**: The actual OSD daemon started after `ceph-volume` completes activation.
*   **GEOM/ZFS**: Relevant for FreeBSD-based deployments.
*   **Cephadm**: Often orchestrates `ceph-volume` in modern containerized deployments.