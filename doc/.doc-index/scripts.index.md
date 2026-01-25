# SCRIPTS Documentation Index

## Overview
This documentation area provides instructions and utility scripts for generating architectural visualizations of the Ceph storage system. Its primary purpose is to automate the creation of state machine diagrams that represent complex internal logic, specifically for OSD peering states.

## Files Summary
*   **scripts/README.md**: Provides step-by-step shell commands for generating, scaling, and rendering the Peering State Model diagram using Graphviz tools.

## Code Changes That Would Require Documentation Updates
*   **PeeringState Logic Transitions**: Any modifications to the state machine transitions, event handling, or state hierarchy within `src/osd/PeeringState.h` or `src/osd/PeeringState.cc`.
*   **Script Relocation or Renaming**: If `gen_state_diagram.py` is moved, renamed, or its command-line arguments are altered.
*   **Visual Formatting Requirements**: Changes to the desired output resolution or scaling (currently handled by the `sed` command in the README) would require updating the documentation's post-processing instructions.
*   **Build System/Path Changes**: Any restructuring of the Ceph source tree that changes the relative paths of the OSD source files or the output directory for generated `.dot` and `.svg` files.
*   **Dependency Updates**: If the generation process shifts away from `dot` (Graphviz) to a different visualization engine.

## Key Technical Concepts
*   **PeeringState**: The internal finite state machine (FSM) used by Ceph OSDs to agree on the state of a Placement Group (PG).
*   **gen_state_diagram.py**: A parser script designed to extract state machine definitions from C++ source/header files.
*   **DOT Language**: The graph description language used by Graphviz to define the nodes and edges of the peering graph.
*   **SVG Rendering**: The final vector graphics format used for displaying the state machine in web-based or technical documentation.
*   **Graph Scaling**: The specific `sed` manipulation of the "7,7" size attribute to "1080,1080" for high-resolution output.

## Related Components
*   **Ceph OSD (Object Storage Daemon)**: The primary daemon responsible for data storage, where peering logic resides.
*   **Placement Groups (PGs)**: The logical collection of objects for which the PeeringState manages replication and recovery states.
*   **Graphviz**: The external software suite required to process the generated `.dot` files.
*   **Ceph Developer Documentation**: The `doc/dev/` directory where the generated artifacts are intended to be stored and viewed.