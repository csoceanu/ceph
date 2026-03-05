This documentation describes **CpuTrace**, a developer-oriented performance profiling tool integrated into the Ceph storage system. It leverages the Linux `perf_event_open` system call to provide high-precision hardware counter measurements for specific code regions.

### 1. Primary Purpose
The file documents the installation, integration, and usage of the `CpuTrace` utility. Its primary goal is to allow Ceph developers to measure the exact CPU cost of execution (instructions, cycles, cache misses, etc.) to validate performance enhancements or compare different algorithmic implementations.

### 2. Key Topics Covered
*   **Build Integration**: How to enable the tool using CMake flags (`WITH_CPUTRACE`).
*   **Measurement Modes**:
    *   **Raw Counter Mode**: Manual start/stop reading of hardware counters.
    *   **RAII Guards**: Automatic measurement using scope-based objects (`HW_guard`, `HW_named_guard`).
    *   **Macro-based Profiling**: Using `HWProfileFunctionF` for quick function-level annotation.
*   **Data Aggregation**: Usage of `measurement_t` to calculate averages, total counts, and context switch distributions.
*   **Runtime Control**: Controlling profiling in a live Ceph daemon via the **Admin Socket** (CLI).
*   **Output Formats**: Exporting data as human-readable strings or structured JSON/YAML/XML via Ceph Formatters.

### 3. Technical Keywords
*   **Core APIs**: `HW_init`, `HW_read`, `HW_clean`, `HW_guard`, `HW_named_guard`, `HWProfileFunctionF`.
*   **Data Structures**: `HW_ctx` (context), `sample_t` (snapshot), `measurement_t` (aggregated stats).
*   **Hardware Counters (Flags)**:
    *   `HW_PROFILE_CYC`: CPU Clock Cycles.
    *   `HW_PROFILE_INS`: Instructions Retired.
    *   `HW_PROFILE_CMISS`: Cache Misses.
    *   `HW_PROFILE_BMISS`: Branch Mispredictions.
    *   `HW_PROFILE_SWI`: Thread/Context Switches.
*   **CLI Commands**: `ceph tell <daemon> cputrace [start|stop|dump|reset]`.
*   **Configuration**: `-DWITH_CPUTRACE=1`.

### 4. Target Audience
*   **Performance Engineers**: Analyzing bottlenecks in the Ceph data path.
*   **Core Developers**: Benchmarking new features or optimizing existing algorithms.
*   **QA/SRE**: Diagnosing performance regressions in live clusters using the admin socket.

### 5. Related Concepts
*   **Linux `perf` subsystem**: The underlying kernel technology.
*   **RAII (Resource Acquisition Is Initialization)**: The C++ design pattern used for the measurement guards.
*   **Ceph Admin Socket**: The communication layer used for runtime control (`ceph tell`).
*   **Ceph Formatter**: The internal library used to standardize JSON/XML output.

### 6. Maintenance Triggers (When to update this file)
This file should be updated if any of the following code changes occur:
*   **New Hardware Metrics**: If support for additional counters (e.g., bus cycles, page faults) is added to `cputrace_flags` or `sample_t`.
*   **API Refactoring**: If the names or arguments of the `HW_` functions, macros (`HWProfileFunctionF`), or guard classes change.
*   **Output Changes**: If the statistics calculated in `measurement_t` are modified or new export formats are added.
*   **Build System Changes**: If the CMake flag `WITH_CPUTRACE` is renamed or the header location `common/cputrace.h` moves.
*   **CLI Changes**: If new subcommands are added to the `cputrace` admin socket interface.