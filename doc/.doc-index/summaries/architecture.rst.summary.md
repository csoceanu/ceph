This documentation file, `architecture.rst`, serves as the high-level conceptual blueprint for the **Ceph Storage System**. It explains the underlying logic, daemon interactions, and data distribution strategies that allow Ceph to function as a unified, distributed storage cluster.

An AI system should update this file whenever there are **architectural shifts** in how data is stored, authenticated, or distributed, or when **core daemon responsibilities** change.

---

### 1. Primary Purpose
The file documents the **internal mechanics and structural design** of Ceph. It aims to explain how Ceph achieves "hyperscale" without centralized bottlenecks by offloading intelligence to individual nodes and clients. It provides the "why" and "how" behind Ceph’s reliability and scalability.

### 2. Key Topics Covered
*   **The Ceph Storage Cluster (RADOS):** The foundation of Ceph, utilizing a Reliable Autonomic Distributed Object Store.
*   **Core Daemons:** Roles of Monitors (MONs), OSDs, Managers (MGRs), and Metadata Servers (MDS).
*   **CRUSH Algorithm:** The mathematical foundation for data placement that eliminates the need for a central lookup table.
*   **Data Placement & Management:** Concepts of Pools, Placement Groups (PGs), Peering, and Rebalancing.
*   **High Availability & Security:** Implementation of the Paxos algorithm for monitor consensus and the `cephx` protocol for authentication.
*   **Advanced Features:** Erasure Coding (K+M chunks), Cache Tiering (logic and deprecation status), and Data Striping.
*   **Client Interfaces:** How `librados` powers RBD (Block), RGW (Object), and CephFS (File).

### 3. Technical Keywords
*   **Core Components:** `RADOS`, `OSD Daemon`, `Ceph Monitor`, `Ceph Manager`, `MDS`, `librados`.
*   **Algorithms & Protocols:** `CRUSH`, `Paxos`, `cephx` (authentication), `BlueStore` (backend).
*   **Data Structures:** `Cluster Map` (including Mon, OSD, PG, CRUSH, and MDS maps), `Placement Groups (PG)`, `Acting Set`, `Up Set`.
*   **Operations:** `Peering`, `Scrubbing` (Light vs. Deep), `Rebalancing`, `Subops`, `Watch/Notify`.
*   **Configuration/Commands:** `ceph mon dump`, `ceph osd dump`, `ceph osd getcrushmap`, `osd class dir`.

### 4. Target Audience
*   **System Architects:** Designing large-scale storage infrastructures.
*   **Storage Administrators:** Needing to understand the "under the hood" behavior for troubleshooting (e.g., peering failures, rebalancing).
*   **Software Developers:** Integrating with Ceph via `librados` or creating "Ceph Classes" (shared object classes).

### 5. Related Concepts
*   **Distributed Systems:** Consensus (Paxos), Hashing vs. Lookup Tables.
*   **Data Protection:** Replication vs. Erasure Coding.
*   **Networking:** Heartbeats, latency impacts on monitor quorums, and TCP/IP port requirements.
*   **Virtualization:** Integration with QEMU/KVM and `libvirt`.

---

### Triggering Updates (AI Guidance)
This file should be updated if code changes occur in the following areas:

1.  **Daemon Evolution:** If a new daemon type is introduced or if an existing daemon's primary responsibility changes (e.g., if the Manager daemon takes over a task previously handled by Monitors).
2.  **Algorithm Changes:** Any modification to the **CRUSH** calculation logic or the introduction of a new consensus protocol replacing **Paxos**.
3.  **Authentication Flow:** Changes to the `cephx` handshake or the introduction of new security layers (like transport encryption) that change the architectural diagrams.
4.  **Data Path Alterations:** If the way `librados` interacts with OSDs changes, or if a new storage backend replaces **BlueStore** as the default.
5.  **Deprecations:** As noted with **Cache Tiering**, when a major architectural feature is deprecated or removed.
6.  **New Mapping Layers:** If the hierarchy of Object -> PG -> OSD is modified by a new abstraction layer.