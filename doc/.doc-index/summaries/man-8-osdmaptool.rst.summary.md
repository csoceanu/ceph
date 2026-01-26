This documentation file describes **`osdmaptool`**, a command-line utility used to manage and manipulate OSD (Object Storage Device) maps within the Ceph distributed storage system.

### 1. Primary Purpose
The primary purpose of `osdmaptool` is to provide a toolset for offline or manual manipulation of OSD cluster maps. It allows users to create new maps, view existing map metadata, import/export CRUSH maps, and simulate data balancing strategies (like `upmap`) to optimize placement group (PG) distribution across OSDs.

### 2. Key Topics Covered
*   **Map Creation & Modification**: Creating simple OSD maps from scratch or based on configuration files.
*   **CRUSH Integration**: Procedures for extracting (exporting) the embedded CRUSH map from an OSD map or injecting (importing) a modified CRUSH map.
*   **Data Placement Testing**: Tools to simulate and debug how PGs and specific objects map to OSDs.
*   **Balancer Simulation (`upmap` and `read`)**: 
    *   **Upmap**: Calculating optimizations to balance PG distribution.
    *   **Read**: Balancing primary PG affinity to distribute read load across replicas.
*   **State Manipulation**: Manually marking OSDs as up, in, or out, and adjusting CRUSH weights for testing purposes.
*   **Reporting**: Generating plaintext or JSON dumps of map health, trees, and distribution statistics (standard deviation, min/max).

### 3. Technical Keywords
*   **Core Entities**: `osdmap`, `crushmap`, `PG` (Placement Group), `pool`.
*   **APIs/Commands**: `osdmaptool`.
*   **Configuration/Options**:
    *   `--createsimple`, `--import-crush`, `--export-crush`.
    *   `--upmap`, `--upmap-max`, `--upmap-deviation`, `--upmap-active`.
    *   `--read`, `--read-pool`.
    *   `--test-map-pgs`, `--test-map-object`.
    *   `--pg-bits`, `--pgp-bits`.
*   **Output Formats**: Plaintext, JSON, Tree view.

### 4. Target Audience
*   **Storage Administrators**: For troubleshooting data distribution issues and manually managing cluster maps.
*   **Ceph Developers**: For testing changes to CRUSH algorithms or map handling logic without requiring a live cluster.
*   **Performance Engineers**: For simulating different balancing scenarios (`upmap`) to predict the impact of cluster changes.

### 5. Related Concepts
*   **CRUSH Algorithm**: The underlying logic determining data placement.
*   **crushtool**: A sister utility focused specifically on CRUSH map manipulation.
*   **Ceph Balancer Module**: The live cluster component that implements the `upmap` logic simulated by this tool.
*   **PG States**: Concepts like `acting set`, `primary OSD`, and `pg_temp`.

---

### Update Triggers for Maintenance
This documentation should be updated if any of the following code changes occur in the Ceph repository:
1.  **New Balancer Modes**: If a new optimization mode (beyond `upmap` and `read`) is added to the OSD map logic.
2.  **CLI Argument Changes**: Adding, renaming, or deprecating flags in the `osdmaptool` source code (typically found in `src/tools/osdmaptool.cc`).
3.  **Map Metadata Changes**: If the structure of the OSD map evolves (e.g., new fields for health checks or PG mapping attributes) that would change the output of `--print` or `--dump`.
4.  **Algorithm Updates**: Changes to how standard deviation or distribution scores are calculated, necessitating updates to the "Example" output section.