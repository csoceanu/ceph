This documentation file provides the manual page for `crushtool`, a fundamental utility in the Ceph storage ecosystem used for managing and validating CRUSH (Controlled Replication Under Scalable Hashing) maps.

### 1. Primary Purpose
The file documents the CLI usage, operational modes, and simulation capabilities of `crushtool`. Its primary role is to provide a guide for administrators to create, compile (binary), decompile (plain text), and test the data distribution logic of a Ceph cluster without affecting a live environment.

### 2. Key Topics Covered
*   **Operational Modes**:
    *   **Compilation/Decompilation**: Converting between human-readable `.txt` maps and binary `.map` files used by the cluster.
    *   **Map Building**: Creating hierarchical topologies (rows, racks, nodes) from scratch using command-line arguments.
    *   **Testing/Simulation**: Running dry runs of data placement to analyze distribution balance and failure scenarios.
    *   **Reclassification**: Migrating legacy maps to use modern "device class" features.
*   **Simulation Metrics**: Detailed reporting on device utilization, mapping success rates, and the number of attempts (`tries`) required to find a valid mapping.
*   **Hierarchy Construction**: Defining bucket types (uniform, list, tree, straw, straw2) and their relationships.

### 3. Technical Keywords
*   **Core Commands**: `crushtool`, `--compile (-c)`, `--decompile (-d)`, `--build`, `--test`.
*   **Simulation Parameters**: `--num-rep`, `--min-x`, `--max-x`, `--simulate`, `--set-choose-total-tries`.
*   **Output/Reporting**: `--show-statistics`, `--show-mappings`, `--show-utilization`, `--output-csv`.
*   **Bucket Types**: `uniform`, `list`, `tree`, `straw`, `straw2`.
*   **Configuration**: `CEPH_ARGS`, `device class`, `Placement Groups (PGs)`.

### 4. Target Audience
*   **Storage Administrators**: For troubleshooting data distribution issues or planning cluster expansions.
*   **System Architects**: For designing hierarchical failure domains (e.g., ensuring replicas land in different racks).
*   **Ceph Developers**: For testing changes to the CRUSH algorithm or validating new map tunables.

### 5. Related Concepts
*   **Ceph OSDs & PGs**: The physical devices and logical units that CRUSH manages.
*   **CRUSH Rules**: The policy engine that dictates how data is replicated or erasure-coded.
*   **RADOS**: The underlying object store that relies on these maps.
*   **osdmaptool**: A related utility for manipulating the broader OSD map (of which the CRUSH map is a part).

---

### Maintenance Triggers: When to Update This File
This documentation should be updated if code changes occur in the following areas:
1.  **CRUSH Algorithm Updates**: If new bucket types (beyond `straw2`) or new tunables are added to the CRUSH source code.
2.  **CLI Argument Changes**: If new flags are added for simulation (e.g., new types of statistics or output formats).
3.  **Default Value Shifts**: If the default range for mapping simulations (`0-1023`) or default "tries" limits are changed in the tool's source.
4.  **Feature Deprecation**: If older bucket types (like `list` or `tree`) are phased out or if the reclassification logic changes.
5.  **Integration with Device Classes**: If the way CRUSH handles hardware properties (SSD vs. HDD) evolves, requiring changes to the `reclassify` or `build` sections.