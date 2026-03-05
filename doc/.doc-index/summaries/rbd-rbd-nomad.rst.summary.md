This documentation file, `rbd/rbd-nomad.rst`, serves as the definitive guide for integrating **Ceph Block Devices (RBD)** with **HashiCorp Nomad** using the Container Storage Interface (CSI).

### 1. Primary Purpose
The file provides a technical walkthrough for enabling persistent block storage in a Nomad cluster via Ceph. It details the architecture, installation of the `ceph-csi` driver, and the lifecycle of an RBD volume from creation to attachment within a Nomad job.

### 2. Key Topics Covered
*   **Architecture Overview**: A visual and textual explanation of how Nomad, `ceph-csi` (controller and node plugins), and the Ceph RADOS protocol interact.
*   **Storage Preparation**: Steps to create and initialize a Ceph pool specifically for Nomad.
*   **Security & Authentication**: Creating Ceph users (`cephx`) with scoped profiles for RBD access.
*   **Nomad Host Configuration**: Prerequisites for Nomad nodes, including enabling privileged Docker containers and loading the `rbd` kernel module.
*   **CSI Deployment**: Detailed Nomad job specifications for running the `ceph-csi` controller and node plugins.
*   **Volume Management**: How to define and register CSI volumes in Nomad.
*   **Application Integration**: A practical example using a MySQL container to demonstrate stateful workload persistence.

### 3. Technical Keywords
*   **APIs/Drivers**: `ceph-csi`, `csi_plugin`, `docker` (task driver), `rbd.csi.ceph.com`.
*   **Configuration**: `allow_privileged`, `attachment_mode` (file-system), `access_mode` (single-node-writer), `fsid`, `monitors`.
*   **Commands**: `nomad volume create`, `nomad job run`, `rbd pool init`, `ceph auth get-or-create`, `modprobe rbd`.
*   **Components**: `controller plugin`, `node plugin`, `RADOS`, `OSD`, `Monitor`, `rbd-nbd`, `librbd`.

### 4. Target Audience
*   **Cloud Architects/SREs**: Designing stateful infrastructure using Nomad.
*   **Storage Administrators**: Configuring Ceph to support container orchestration.
*   **DevOps Engineers**: Deploying persistent applications (like databases) on Nomad clusters.

### 5. Related Concepts
*   **Container Storage Interface (CSI)**: The industry-standard protocol this implementation follows.
*   **Kubernetes RBD Integration**: Mentioned as a parallel use case for `ceph-csi`.
*   **Stateful Workloads**: The broader category of applications requiring persistent data across restarts.
*   **Kernel Tunables**: Mentioned regarding `CRUSH` and `RBD image features` compatibility with kernel modules.

---

### Maintenance Summary for AI Systems
To keep this documentation accurate, it should be reviewed or updated when the following code-level or environmental changes occur:

*   **Version Updates**: If the reference versions of Nomad (v1.1.2) or `ceph-csi` (v3.3.1) are deprecated or if breaking changes occur in newer releases.
*   **CSI Spec Changes**: If HashiCorp updates the Nomad HCL syntax for `csi_plugin` stanzas or `volume` registration blocks.
*   **Ceph Auth Changes**: If the syntax for `ceph auth` profiles for RBD changes in the Ceph core.
*   **Driver Defaults**: If `ceph-csi` changes its default behavior regarding RBD kernel modules vs. `rbd-nbd` (userspace).
*   **Configuration Paths**: Changes to standard Nomad directory structures (e.g., `/etc/nomad.d/`) or Docker plugin defaults.