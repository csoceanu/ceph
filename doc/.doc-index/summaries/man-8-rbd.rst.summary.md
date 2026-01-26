This documentation file serves as the official manual page for the `rbd` (RADOS Block Device) command-line utility. It provides comprehensive instructions for managing block device images within a Ceph storage cluster.

### 1. Primary Purpose
The file documents the **`rbd` CLI tool**, which is the primary interface for creating, modifying, mapping, and managing RBD images. These images are virtual block devices that are striped across objects in a RADOS object store and are commonly used by Linux kernel drivers and QEMU/KVM for cloud storage.

### 2. Key Topics Covered
*   **Image Lifecycle Management**: Creation (`create`), deletion (`rm`), resizing (`resize`), and renaming (`mv`).
*   **Snapshot & Cloning**: Mechanisms for creating snapshots, protecting them, and creating copy-on-write clones.
*   **Device Operations**: Mapping and unmapping RBD images to local block devices via the kernel (`krbd`), `nbd`, or `ggate`.
*   **Data Protection & Maintenance**: Commands for image encryption, disk usage (`du`) calculation, and "trash" management for deferred deletion.
*   **Mirroring & Migration**: Tools for asynchronous replication between clusters (mirroring) and moving images between pools/formats (migration).
*   **Performance & Tuning**: Configuration for data striping ("fancy" striping), benchmarking (`bench`), and object map management.
*   **Metadata & Configuration**: Setting image-level or pool-level configuration overrides and custom metadata.

### 3. Technical Keywords
*   **APIs/Drivers**: `librbd`, `krbd` (kernel RBD), `nbd`, `KVM/QEMU`.
*   **Image Formats**: Format 1 (legacy), Format 2 (current, supports cloning/features).
*   **Feature Flags**: `layering`, `exclusive-lock`, `object-map`, `fast-diff`, `journaling`, `deep-flatten`.
*   **Configuration Options**: `--stripe-unit`, `--stripe-count`, `--object-size`, `--image-feature`, `--data-pool`.
*   **Commands**: `snap`, `clone`, `mirror`, `map`, `unmap`, `sparsify`, `flatten`, `export-diff`, `import-diff`.
*   **Kernel Options**: `ms_mode`, `crush_location`, `read_from_replica`, `rxbounce`.

### 4. Target Audience
*   **Storage Administrators**: Managing Ceph cluster resources and storage pools.
*   **Cloud Engineers**: Integrating Ceph with virtualization platforms like OpenStack or Kubernetes.
*   **DevOps Engineers**: Automating block storage provisioning and snapshot/backup workflows.
*   **Kernel/System Developers**: Debugging block device mappings and performance tuning.

### 5. Related Concepts
*   **RADOS**: The underlying distributed object store where RBD data resides.
*   **Ceph Monitors (MONs) & OSDs**: The components `rbd` communicates with to manage metadata and data.
*   **Copy-on-Write (CoW)**: The mechanism used for efficient RBD cloning.
*   **CRUSH Hierarchy**: Used in kernel mapping options to determine data locality and replica selection.
*   **LUKS (luks1/luks2)**: The encryption standard used for RBD image-level encryption.

---

### Update Triggers for AI Systems
This file must be updated if code changes occur in the following areas:
1.  **CLI Argument Parser**: If new flags are added to existing subcommands or if global options (like `--namespace`) are introduced.
2.  **RBD Feature Bits**: If new image features (e.g., a new type of compression or locking) are added to `librbd`.
3.  **Kernel Driver (krbd)**: If new mapping options are added to the Linux kernel module (e.g., new `ms_mode` or replica selection logic).
4.  **Mirroring/Migration Logic**: If the workflow for cross-cluster replication or image migration steps is altered.
5.  **New Subcommands**: If new management capabilities (similar to the recent additions of `namespace` or `encryption`) are implemented.