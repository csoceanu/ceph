This documentation file serves as the official manual page for the `rbd` (RADOS Block Device) command-line utility. It is the primary reference for managing block device images within a Ceph storage cluster.

### 1. Primary Purpose
The file documents the **usage, syntax, and options of the `rbd` CLI tool**. This tool is used to create, modify, view, and delete RBD images, which are virtual block devices striped across objects in a Ceph RADOS cluster. It also covers the management of snapshots, clones, mirroring, and the mapping of these images to local block devices via the Linux kernel (krbd) or userspace drivers.

### 2. Key Topics Covered
*   **Image Management**: Creating, resizing, copying, importing/exporting, and deleting images.
*   **Snapshots and Clones**: Creating read-only snapshots, protecting them, and creating copy-on-write (COW) clones.
*   **Kernel Device Mapping (krbd)**: Mapping/unmapping RBD images to host devices (e.g., `/dev/rbd0`) and configuring driver-specific behaviors.
*   **Mirroring and Replication**: Configuring asynchronous mirroring between clusters, including pool and image-level replication modes.
*   **Trash Management**: A "recycle bin" system for images to prevent accidental deletion and handle deferred removal.
*   **Advanced Storage Logic**: Configuration of "fancy" striping (stripe units/counts), namespaces, and object-map optimizations.
*   **Security & Encryption**: LUKS-based image encryption (luks1/luks2) and locking mechanisms for shared access.
*   **Maintenance & Metadata**: Journaling, performance benchmarking, and image-level metadata key-value pairs.

### 3. Technical Keywords
*   **Commands**: `create`, `clone`, `snap`, `device map`, `mirror`, `trash`, `sparsify`, `flatten`, `migration`.
*   **Configuration Options**: `--image-format` (1 vs 2), `--object-size`, `--stripe-unit`, `--stripe-count`, `--image-feature`.
*   **Features**: `layering`, `exclusive-lock`, `object-map`, `fast-diff`, `journaling`.
*   **APIs/Drivers**: `librbd`, `krbd` (kernel rbd), `RADOS`, `QEMU/KVM`.
*   **Addressing Syntax**: `pool-name/namespace-name/image-name@snap-name`.

### 4. Target Audience
*   **Storage Administrators**: Managing the lifecycle and health of block storage.
*   **Cloud Architects**: Integrating Ceph with platforms like OpenStack or Kubernetes.
*   **System Engineers**: Troubleshooting performance or kernel-level mapping issues.
*   **DevOps Engineers**: Automating storage provisioning and backup/DR workflows.

### 5. Related Concepts
*   **RADOS**: The underlying object store where RBD data is actually kept.
*   **Ceph Monitor (MON) & OSD**: The cluster components `rbd` interacts with for metadata and data storage.
*   **CRUSH Hierarchy**: Used for locality-aware replica reads (`read_from_replica`).
*   **Copy-on-Write (COW)**: The mechanism allowing efficient snapshots and clones.

---

### Update Triggers for AI Systems
This file must be updated if any of the following code changes occur:
1.  **CLI Changes**: Any addition of new subcommands or modification/deprecation of existing flags in the `rbd` C++ source code.
2.  **New Image Features**: When a new RBD Format 2 feature is added (e.g., a new bit in `rbd_default_features`).
3.  **Kernel Driver Updates**: When the Linux `krbd` module introduces new mount-style options or supports new wire protocols (like `msgr2.1`).
4.  **Mirroring Logic**: Changes to how `rbd-mirror` handles snapshot scheduling or peer cluster authentication.
5.  **Defaults**: Changes to `rbd_default_order`, default object sizes, or the transition of experimental features to stable.