# RBD Documentation Index

## Overview
This documentation covers the RADOS Block Device (RBD) component of Ceph, detailing how to create, manage, and optimize block storage images. It serves as a comprehensive guide for administrators and developers to integrate RBD with various platforms, manage data lifecycle through snapshots and mirroring, and ensure high availability through specialized gateways like iSCSI and NVMe-oF.

## Files Summary
*   **rbd/iscsi-initiators.rst**: Links to OS-specific configuration guides for iSCSI clients (Linux, Windows, VMware).
*   **rbd/rbd-exclusive-locks.rst**: Explains the mechanism and configuration of exclusive locking for multi-client coordination and data protection.
*   **rbd/rbd-mirroring.rst**: Comprehensive guide on asynchronous replication between clusters using journal-based or snapshot-based modes.
*   **rbd/iscsi-monitoring.rst**: Documentation for `gwtop` and Performance Metrics Domain Agents to monitor iSCSI gateway performance.
*   **rbd/rbd-windows.rst**: Details the usage of RBD on Windows, including the `rbd-wnbd` service and Hyper-V integration.
*   **rbd/nvmeof-target-configure.rst**: Step-by-step instructions for deploying and configuring NVMe-oF gateways using `cephadm` and `nvmeof-cli`.
*   **rbd/iscsi-requirements.rst**: Lists hardware and software prerequisites, specifically focusing on OSD heartbeat tuning for iSCSI.
*   **rbd/nvmeof-initiator-linux.rst**: Procedure for connecting Linux clients to NVMe-oF targets.
*   **rbd/qemu-rbd.rst**: Technical guide on integrating RBD directly with QEMU/KVM, including caching and discard/TRIM support.
*   **rbd/iscsi-targets.rst**: High-level overview of iSCSI target deployment options (Ansible vs. CLI).
*   **rbd/iscsi-initiator-linux.rst**: Specific configuration for Linux iSCSI initiators using multipath-tools.
*   **rbd/rbd-integrations.rst**: Index of 3rd party integrations (Kubernetes, OpenStack, Nomad, etc.).
*   **rbd/rbd-replay.rst**: Instructions for workload capturing and replaying using `lttng` and `rbd-replay` tools.
*   **rbd/libvirt.rst**: Detailed configuration for using RBD with libvirt, including cephx secret management and XML domain definitions.
*   **rbd/rbd-nomad.rst**: Integration guide for Hashicorp Nomad using `ceph-csi`.
*   **rbd/nvmeof-requirements.rst**: Hardware, memory, and networking prerequisites for NVMe-oF gateways.
*   **rbd/rbd-openstack.rst**: Deep-dive into OpenStack (Glance, Cinder, Nova) integration with RBD backends.
*   **rbd/iscsi-target-cli-manual-install.rst**: Low-level manual installation steps for `ceph-iscsi` components from source.
*   **rbd/rbd-snapshot.rst**: Core documentation on image snapshots, cloning, and layering.
*   **rbd/rbd-config-ref.rst**: Reference guide for `librbd` configuration settings including cache, QoS, and image features.
*   **rbd/rados-rbd-cmds.rst**: Manual for basic image management commands (create, list, info, resize, trash).
*   **rbd/iscsi-overview.rst**: Conceptual overview of the Ceph iSCSI Gateway and its maintenance status.
*   **rbd/rbd-encryption.rst**: Details on client-side image encryption using LUKS formats.
*   **rbd/rbd-live-migration.rst**: Guide on live-migrating images between pools or from external sources (S3, HTTP, NBD).
*   **rbd/nvmeof-initiators.rst**: Index of NVMe-oF initiator guides.
*   **rbd/iscsi-initiator-win.rst**: Specific MPIO and registry tuning for Windows iSCSI initiators.
*   **rbd/rbd-persistent-write-log-cache.rst**: Documentation for PWL cache using PMEM or SSD devices.
*   **rbd/rbd-operations.rst**: High-level index of RBD operational tasks.
*   **rbd/iscsi-target-cli.rst**: Guide for manual configuration of iSCSI targets using the `gwcli` tool.
*   **rbd/nvmeof-overview.rst**: Conceptual overview of NVMe-oF gateways, scale-out, and high availability.
*   **rbd/index.rst**: Main landing page for Ceph Block Device documentation.
*   **rbd/iscsi-target-ansible.rst**: Instructions for deploying iSCSI gateways using `ceph-ansible`.
*   **rbd/rbd-ko.rst**: Operations for the Linux kernel RBD (krbd) module.
*   **rbd/iscsi-initiator-esx.rst**: Configuration steps for VMware ESX iSCSI initiators.
*   **rbd/rbd-persistent-read-only-cache.rst**: Documentation for the Shared Read-only Parent Image Cache and its daemon.
*   **rbd/nvmeof-initiator-esx.rst**: Configuration for VMware ESX NVMe-oF initiators.
*   **rbd/rbd-cloudstack.rst**: Integration guide for Apache CloudStack.
*   **rbd/rbd-kubernetes.rst**: Integration guide for Kubernetes using `ceph-csi`.
*   **rbd/api/librbdpy.rst**: Python API documentation for `librbd`.

## Code Changes That Would Require Documentation Updates
*   **New Image Features**: Adding new bits to `rbd_default_features` (e.g., new compression types, data handling).
*   **Librbd API changes**: Modifications to `librbd.h` or the Python `rbd` module signatures.
*   **Command Line Interface**: Adding new subcommands or flags to the `rbd` tool, `gwcli`, or `nvmeof-cli`.
*   **Gateway Logic**: Changes to `ceph-iscsi`, `tcmu-runner`, or the `nvmeof` daemon's handling of targets/subsystems.
*   **CSI Driver**: Updates to `ceph-csi` affecting how volumes are provisioned for Kubernetes or Nomad.
*   **Cache Architectures**: Modifications to PWL (Persistent Write Log), `rwl`, or immutable object cache logic.
*   **Encryption**: Adding new LUKS versions, cipher algorithms, or changing how secrets are handled.
*   **Migration Streams**: Adding support for new external data sources (e.g., Azure Blob, different S3 providers) in live-migration.
*   **Kernel Module (krbd)**: Backporting features previously limited to `librbd` (like encryption or live-migration) to the kernel.
*   **Configuration Defaults**: Changing default values for `rbd_cache`, QoS limits, or heartbeat intervals.

## Key Technical Concepts
*   **Core Commands**: `rbd create`, `rbd snap`, `rbd clone`, `rbd device map`, `gwcli`, `nvmeof-cli`.
*   **Image Features**: `layering`, `exclusive-lock`, `object-map`, `fast-diff`, `journaling`, `deep-flatten`.
*   **Caching**: `write-back`, `write-through`, `PWL`, `RWL`, `SSD cache`, `immutable-object-cache`.
*   **Networking/Protocols**: `iSCSI`, `NVMe-oF`, `NVMe/TCP`, `LIO`, `TCMU`, `ALUA`, `MPIO`.
*   **Architecture**: `Thin provisioning`, `Copy-on-Write (COW)`, `Striping`, `Fencing`, `Blocklisting`.
*   **Encryption**: `LUKS1`, `LUKS2`, `dm-crypt`, `AES-256`, `passphrase-file`.
*   **Migration/Mirroring**: `journal-based mirroring`, `snapshot-based mirroring`, `import-only migration`, `source-spec`.

## Related Components
*   **RADOS**: The underlying object store (OSDs and Monitors).
*   **LIO**: The Linux IO Target kernel subsystem used by iSCSI.
*   **Librbd**: The primary userspace library for block device access.
*   **Ceph-CSI**: The interface for container orchestrators (Kubernetes/Nomad).
*   **QEMU/KVM & Libvirt**: The primary virtualization stack for RBD.
*   **OpenStack (Cinder/Glance/Nova)**: Cloud infrastructure components utilizing RBD storage.
*   **WNBD**: The Windows driver for RBD.
*   **NVMe-oF Gateway**: The specialized daemon for NVMe over Fabrics.