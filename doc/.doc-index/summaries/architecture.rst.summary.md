This documentation file, `architecture.rst`, serves as the high-level structural blueprint for the **Ceph Storage System**. It explains the theoretical and operational foundations that allow Ceph to provide unified object, block, and file storage.

### 1. Primary Purpose
The file documents the **system-level architecture** of Ceph. It explains how data moves from a client to physical disks, how the cluster maintains consistency and availability without a central bottleneck, and the roles of the core daemons. It is designed to provide the "big picture" of how Ceph achieves exabyte-scale distribution.

### 2. Key Topics Covered
*   **The Ceph Storage Cluster (RADOS):** The foundation of all Ceph services, focusing on the Reliable Autonomic Distributed Object Store.
*   **Core Daemons:** Roles of Monitors (MONs), OSDs, Managers (MGRs), and Metadata Servers (MDS).
*   **CRUSH Algorithm:** The mathematical method used to calculate data placement, eliminating the need for centralized lookup tables.
*   **Scalability & High Availability:** How Ceph uses Paxos for monitor consensus and `cephx` for secure, decentralized authentication.
*   **Data Management Operations:** Detailed explanations of Replication, Peering, Rebalancing, and Scrubbing.
*   **Storage Strategies:** Comprehensive breakdown of Replication vs. Erasure Coding (including how chunks are read/written).
*   **Client Interfaces:** Architecture of the three main interfaces: RBD (Block), RGW (Object), and CephFS (File).
*   **Advanced Features:** Data striping logic, Cache Tiering (noted as deprecated), and "Ceph Classes" for extensibility.

### 3. Technical Keywords
*   **Daemons/Components:** `ceph-osd`, `ceph-mon`, `ceph-mgr`, `ceph-mds`, `RADOS Gateway (RGW)`.
*   **APIs/Libraries:** `librados`, `librbd`, `libcephfs`, `FastCGI`, `S3/Swift-compatible APIs`.
*   **Algorithms/Protocols:** `CRUSH`, `Paxos`, `cephx`, `Erasure Coding (K+M shards)`, `BlueStore`.
*   **Data Structures:** `Cluster Map`, `Monitor Map`, `OSD Map`, `PG Map`, `CRUSH Map`, `MDS Map`.
*   **Operational Terms:** `Placement Groups (PGs)`, `Acting Set`, `Up Set`, `Peering`, `Scrubbing`, `Subops`.
*   **Commands:** `ceph mon dump`, `ceph osd dump`, `ceph fs dump`, `crushtool`.

### 4. Target Audience
*   **System Architects:** Designing large-scale storage infrastructures.
*   **Storage Administrators:** Troubleshooting data placement, peering issues, or performance bottlenecks.
*   **Developers:** Writing custom clients using `librados` or extending Ceph via Object Classes.
*   **New Users:** Seeking a conceptual understanding before diving into deployment.

### 5. Related Concepts
*   **Distributed Systems:** Consensus protocols (Paxos) and decentralized hashing.
*   **Virtualization/Cloud:** Integration with QEMU, KVM, and OpenStack.
*   **POSIX Compliance:** How CephFS maps object storage to a standard file system hierarchy.
*   **Network Security:** Authentication without bottlenecks (Kerberos-like behavior).

---

### Update Trigger Guide (For AI/Automation)
This file should be updated if code changes occur in the following areas:

*   **Daemon Evolution:** If a new core daemon type is introduced or if the fundamental responsibility of an existing daemon (e.g., the Manager) changes.
*   **Map Structure:** If the "Five Maps" of the Cluster Map are expanded, or if the metadata stored within the Monitor or OSD maps is significantly restructured.
*   **Data Placement Logic:** If the CRUSH algorithm is superseded or if the way PGs are calculated or mapped to OSDs changes.
*   **Authentication Changes:** If `cephx` is replaced or if the ticket/session key flow is altered.
*   **New Storage Strategies:** If a new method of data protection beyond Replication and Erasure Coding is implemented.
*   **Deprecation Lifecycle:** When deprecated features (like Cache Tiering) are fully removed from the codebase.
*   **Client Interface Architecture:** If the relationship between `librados` and higher-level libraries (`librbd`, etc.) is changed (e.g., a new middle-layer introduction).