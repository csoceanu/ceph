This documentation provides a technical guide for integrating **Ceph RADOS Block Devices (RBD)** with the **libvirt** virtualization API. It primarily focuses on enabling QEMU/KVM virtual machines to use Ceph storage backends.

### 1. Primary Purpose
The file documents the end-to-end workflow for configuring a Ceph storage cluster and a libvirt-managed virtualization host so that Virtual Machines (VMs) can utilize Ceph RBD images as network-attached disks.

### 2. Key Topics Covered
*   **Architectural Overview**: Explains the stack relationship: `libvirt` → `QEMU` → `librbd` → `librados` → `Ceph OSDs/Monitors`.
*   **Ceph Cluster Configuration**: Steps to create pools, initialize them for RBD, and create specific CephX users/permissions for libvirt.
*   **Image Management**: Using `qemu-img` and `rbd` commands to provision block device images.
*   **VM Creation & Management**: Guidelines for using `virt-manager` (GUI) and `virsh` (CLI) to define virtual machine domains.
*   **Domain XML Configuration**: Detailed instructions on modifying VM XML files to include RBD disk definitions and authentication secrets.
*   **Authentication & Security**: Procedures for defining libvirt secrets to store CephX keys securely.
*   **Verification**: Commands to validate connectivity between the VM, the hypervisor, and the Ceph cluster.

### 3. Technical Keywords
*   **APIs/Libraries**: `libvirt`, `librbd`, `librados`, `QEMU/KVM`.
*   **CLI Tools**: `virsh`, `rbd`, `ceph`, `qemu-img`, `virt-manager`.
*   **Configuration Elements**: `<disk type='network'>`, `<source protocol='rbd'>`, `<auth>`, `<secret>`, `virtio` (bus type).
*   **Ceph Concepts**: `cephx` (authentication), `mon profile rbd`, `osd profile rbd pool`, `admin socket`.
*   **Libvirt Commands**: `virsh edit`, `virsh secret-define`, `virsh secret-set-value`, `virsh qemu-monitor-command`.

### 4. Target Audience
*   **System Administrators**: Managing private clouds or virtualization clusters.
*   **Cloud Architects**: Integrating Ceph with platforms like OpenStack, CloudStack, or OpenNebula.
*   **DevOps Engineers**: Automating VM provisioning with Ceph storage backends.

### 5. Related Concepts
*   **Cloud Platforms**: OpenStack, OpenNebula, and CloudStack (which use this libvirt/RBD integration under the hood).
*   **Hypervisors**: Specifically QEMU/KVM as the primary supported consumer of RBD via libvirt.
*   **RBAC/Security**: CephX user management and libvirt secret management for secure storage access.

---

### Update Triggers for Maintenance
This file should be updated if any of the following code or system changes occur:
*   **XML Schema Changes**: If the libvirt `<disk>` or `<auth>` XML syntax for the `rbd` protocol changes.
*   **Ceph Capabilities**: If the recommended `ceph auth` profiles (e.g., `profile rbd`) are deprecated or updated.
*   **Default Ports**: If the default Ceph monitor port (6789) or protocol changes (e.g., msgr2/v2 protocol).
*   **Deprecations**: If `qemu-img` changes its syntax for RBD source strings or if specific bus types (like `virtio`) are superseded.
*   **Logging/Debugging**: If the pathing for admin sockets or client log naming conventions in `ceph.conf` are altered.