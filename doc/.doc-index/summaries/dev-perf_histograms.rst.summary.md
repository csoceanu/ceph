This documentation file describes the **Perf Histograms** subsystem within Ceph, which provides a multi-dimensional view of performance data gathered by Ceph daemons.

### 1. Primary Purpose
The file documents the infrastructure used to collect, organize, and export performance histogram data. Unlike standard counters that provide a single cumulative value, these histograms track the frequency distribution of events (typically latency and data sizes) over time, allowing for more granular performance analysis and bottleneck identification.

### 2. Key Topics Covered
*   **Infrastructure Overview**: How perf histograms build upon the existing `perf counters` framework using 64-bit unsigned integers.
*   **Access Mechanisms**: Procedures for retrieving histogram data from running daemons.
*   **Organizational Hierarchy**: The grouping of histograms into "Collections" based on subsystems (e.g., OSD, AsyncMessenger, Throttle).
*   **Data Schema**: Interpretation of the JSON-formatted schema, specifically the bitfield-based "type" system.
*   **Data Dump Format**: Detailed breakdown of the JSON output, including axes, buckets, quantization, and history entries.
*   **Visualization**: Reference to internal tools for processing the raw JSON data.

### 3. Technical Keywords
*   **Commands**: `ceph daemon <name> perf histogram schema`, `ceph daemon <name> perf histogram dump`.
*   **Schema Bitfields**: 
    *   `1`: Floating point
    *   `2`: Unsigned 64-bit integer
    *   `4`: Average (sum + count)
    *   `8`: Counter (vs. gauge)
    *   `16` (5th bit): Mandatory bit for histograms (e.g., Type 18 = 16 + 2).
*   **Data Structures**: `axes`, `buckets`, `quant_size`, `scale_type` (`log2` or `linear`), `ranges`.
*   **Tools**: `src/tools/histogram_dump.py`.
*   **Subsystems**: `AsyncMessenger`, `WBThrottle`, `OSD`, `Objecter`.

### 4. Target Audience
*   **Ceph Developers**: Adding new performance metrics to a subsystem.
*   **Performance Engineers**: Analyzing latency distributions or I/O patterns.
*   **Tooling Developers**: Writing external monitoring scripts or integrations (e.g., Prometheus exporters) that need to parse Ceph’s admin socket output.

### 5. Related Concepts
*   **Perf Counters**: The base layer upon which histograms are built.
*   **Admin Socket**: The communication channel used to query the daemon directly.
*   **JSON Serialization**: The format used for all schema and data exports.
*   **Latency/Throughput Analysis**: The primary use case for these histograms (e.g., tracking `op_r_latency_out_bytes`).

---

### Maintenance Trigger: When to update this file
This documentation should be updated if any of the following code changes occur:
1.  **Bitfield Definitions**: If the logic for the `type` bitfield changes in the source code (e.g., adding new data types beyond floating point or 64-bit integers).
2.  **Command Syntax**: If the `ceph daemon` command arguments for performance metrics are renamed or modified.
3.  **JSON Structure**: If the structure of the JSON output for `dump` or `schema` is altered (e.g., adding new fields to the `axes` or `ranges` objects).
4.  **New Scale Types**: If the system introduces new scaling logic beyond `log2` and `linear`.
5.  **Tooling Changes**: If `histogram_dump.py` is moved, renamed, or replaced by a new standard tool.