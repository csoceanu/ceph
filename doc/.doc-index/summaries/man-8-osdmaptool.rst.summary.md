This analysis summarizes the `osdmaptool.rst` documentation, which serves as the manual page for the Ceph OSD map manipulation utility.

### 1. Primary Purpose
The file documents **osdmaptool**, a CLI utility used to create, view, and manipulate OSD (Object Storage Device) cluster maps within the Ceph distributed storage system. Its primary role is to allow administrators to perform offline map analysis, CRUSH map extractions/imports, and PG (Placement Group) distribution simulations without affecting a live cluster.

### 2. Key Topics Covered
*   **Map Management**: Creating "simple" maps from scratch or from configuration files, and printing existing maps in plaintext or JSON.
*   **CRUSH Integration**: Exporting and importing CRUSH maps (the algorithm-driven map that determines data placement) to/from the OSD map.
*   **PG Mapping Analysis**: Testing how Placement Groups map to specific OSDs, including tools to dump summaries and analyze acting sets.
*   **State Simulation**: Manually marking OSDs as "up," "in," or "out" to test distribution changes.
*   **Balancer Simulation (Upmap & Read)**:
    *   **Upmap**: Simulating the optimization of PG distribution to reduce standard deviation across OSDs.
    *   **Read**: Simulating primary affinity adjustments to balance read requests across OSDs in replicated pools.

### 3. Technical Keywords
*   **APIs/Commands**: `osdmaptool`, `crushtool`, `ceph`.
*   **Configuration Options**: 
    *   `--createsimple`, `--import-crush`, `--export-crush`.
    *   `--upmap`, `--upmap-max`, `--upmap-deviation`, `--upmap-active`.
    *   `--test-map-pgs`, `--test-map-pgs-dump`, `--test-crush`.
    *   `--read`, `--read-pool` (Read balancer specific).
*   **Terms**: OSD (Object Storage Device), CRUSH (Controlled Replication Under Scalable Hashing), PG (Placement Group), `pg_num`, `pgp_num`, acting set, primary OSD, `pg_temp`, `primary_temp`, standard deviation.

### 4. Target Audience
*   **Ceph Storage Administrators**: For troubleshooting data distribution and testing CRUSH changes.
*   **System Architects**: For modeling cluster expansions or evaluating balancing strategies before deployment.
*   **Developers**: Working on Ceph’s mapping or balancing logic who need to verify map outputs.

### 5. Related Concepts
*   **CRUSH Maps**: This tool is the primary bridge between the OSD map and `crushtool`.
*   **Ceph Balancer Module**: `osdmaptool` provides an offline simulation of the logic used by the online `mgr` balancer module.
*   **Data Durability**: Understanding PG-to-OSD mapping is critical for evaluating fault domains and data safety.

---

### Update Triggers for AI Systems
This documentation file should be updated if code changes occur in the following areas:
1.  **CLI Arguments**: Any changes to `Args.cc` or the command-line parsing logic in `osdmaptool.cc`.
2.  **Mapping Logic**: Updates to how PGs are calculated or how `pg_num` is shifted (relevant to `--pg-bits`).
3.  **Balancer Algorithms**: If the `upmap` or `read` balancing logic is modified, the simulation output examples in this doc will become obsolete.
4.  **Map Format**: If the internal structure of the OSDMap or CRUSHMap changes (e.g., new health checks or map variables), new `--dump` or `--print` flags may be added.