This documentation file, `rados/operations/add-or-rm-mons.rst`, is the definitive guide for managing the lifecycle and network configuration of **Ceph Monitors (MONs)**. It provides both the theoretical requirements for cluster consensus and the practical CLI procedures for altering the monitor cluster membership.

### 1. Primary Purpose
The file documents the administrative procedures for **scaling, maintaining, and recovering** the Ceph Monitor quorum. It outlines how to add or remove monitor daemons and how to transition monitors to new IP addresses or networks while ensuring the cluster maintains a Paxos-driven consensus.

### 2. Key Topics Covered
*   **Quorum and Paxos Theory**: Explains the necessity of an odd number of monitors to avoid "split-brain" scenarios and the requirement for a majority (50% + 1) to be active.
*   **Manual Deployment**: Step-by-step instructions for manually creating monitor data directories, fetching keyrings/maps, and initializing daemons.
*   **Monitor Removal**: Procedures for graceful removal and emergency removal from "unhealthy" (non-quorum) clusters.
*   **IP/Network Migration**: 
    *   The "Preferred Method" (additive replacement).
    *   The "Advanced Method" (direct `monmap` manipulation).
*   **Cephadm Integration**: Specific workflows for modern Ceph deployments using `cephadm` to change the public network and host addresses.

### 3. Technical Keywords
*   **Daemons/Components**: `ceph-mon`, `ceph-mgr`, `OSD`.
*   **Tools/CLI**: `monmaptool`, `cephadm`, `ceph-mon --mkfs`, `ceph auth get`, `ceph mon getmap`, `ceph orch host set-addr`.
*   **Configuration/Maps**: `monmap`, `ceph.conf`, `keyring`, `fsid`, `public-addr`, `public-network`.
*   **Concepts**: Paxos algorithm, Quorum, Split-brain, Epoch, Bootstrap.
*   **Directories**: `/var/lib/ceph/mon/`, `/var/lib/ceph/{FSID}/`.

### 4. Target Audience
*   **Ceph Cluster Administrators**: Responsible for cluster health and scaling.
*   **System Operations/SREs**: Handling hardware migrations or network re-addressing.
*   **Infrastructure Engineers**: Automating Ceph deployments or building disaster recovery playbooks.

### 5. Related Concepts
*   **RADOS Cluster Health**: Directly impacts the ability of the cluster to function; without MON quorum, OSDs cannot operate.
*   **Ceph Orchestrator (`cephadm`)**: Modern management layer that abstracts some of these manual steps.
*   **Network Configuration**: Relates to `ceph.conf` and the underlying Linux networking stack.
*   **Hardware Planning**: Tied to recommendations for dedicated monitor nodes to avoid resource contention with OSDs.

### When to Update This Document
This file should be updated if:
1.  **CLI Changes**: Arguments for `ceph-mon` or `monmaptool` are deprecated or added.
2.  **Default Path Changes**: The standard directory structure for Ceph data (e.g., `/var/lib/ceph/mon`) is altered in new releases.
3.  **Quorum Logic**: Changes are made to the Paxos implementation or how Ceph handles monitor elections.
4.  **Orchestrator Evolution**: `cephadm` introduces new commands that simplify or replace the manual `monmap` injection steps.
5.  **Security Standards**: Changes to how monitor keyrings or authentication (`cephx`) are handled.