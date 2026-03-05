This documentation file, `developing-cephadm.rst`, serves as the primary technical guide for developers contributing to the **Cephadm orchestrator** and related Ceph management components (MGR modules, Dashboard). It outlines the various environments and workflows used to build, test, and debug Ceph clusters managed by cephadm.

### 1. Primary Purpose
The file documents the **setup and maintenance of development environments** specifically for `cephadm`. It bridges the gap between raw source code and a running, managed Ceph cluster, providing multiple pathways depending on whether the developer is working on compiled C++ binaries, Python-based MGR modules, or the cephadm command-line tool itself.

### 2. Key Topics Covered
*   **vstart --cephadm**: Integrating the traditional `vstart.sh` script with cephadm to allow a hybrid environment where core daemons (MON/MGR) run locally while others are orchestrated by cephadm.
*   **cstart and cpatch**: A workflow for launching "production-like" containerized clusters and "patching" local code changes into running containers to avoid full image rebuilds.
*   **Kcli Integration**: Using a virtualization management tool to deploy multi-node, "close-to-production" labs across various hypervisors (libvirt, GCP, AWS, etc.).
*   **Cephadm Box**: An experimental "Podman-in-Podman" (or Docker) environment for rapid, isolated cluster testing without the overhead of VMs.
*   **Dashboard Development**: Specific instructions for building the Angular frontend and reflecting changes in a managed cluster.
*   **Technical Architecture Notes**: Constraints on synchronous network calls and standardized naming conventions for services and daemons.
*   **Build/Compilation**: Instructions for "compiling" the cephadm Python script into a standalone Zipapp.

### 3. Technical Keywords
*   **Scripts/Commands**: `vstart.sh`, `cstart.sh`, `ckill.sh`, `cpatch`, `box.py`, `build.py`, `ceph orch`, `ceph mgr fail`.
*   **Configuration Options**: `--cephadm`, `--shared_ceph_folder`, `--single-host-defaults`, `-o container_image`.
*   **Tooling**: `podman-compose`, `kcli`, `libvirt`, `Zipapp (PEP 441)`, `pip`, `npm/nodejs`.
*   **Naming Schema**: `service_type`, `service_id`, `daemon_id`, `fsid`.

### 4. Target Audience
*   **Ceph Core Developers**: Those working on the C++ backend who need to test how their binaries interact with orchestration.
*   **Orchestrator/MGR Developers**: Developers writing Python modules (like the cephadm MGR module) who need to rapidly iterate on code.
*   **Dashboard Developers**: Frontend/Backend developers needing a live environment to test UI changes.
*   **QA/QE Engineers**: Individuals looking for reproducible ways to stand up test labs that mimic production environments.

### 5. Related Concepts
*   **Ceph Orchestrator (MGR Module)**: The logic that lives inside the Ceph Manager.
*   **Containers/Images**: Heavily relies on `quay.io` images and container runtimes (Podman/Docker).
*   **vstart**: The legacy local-process development script for Ceph.
*   **LVM/Loopback devices**: Used specifically in the "Box" and OSD development workflows to simulate physical storage.

---

### Triggering Updates: When to Edit This File
This file should be updated by an AI or developer if any of the following code changes occur:
1.  **New Scripting/Tooling**: If a new helper script is added to `src/script/` or `src/cephadm/` for cluster lifecycle management.
2.  **Dependency Changes**: If the `cephadm` build process (in `build.py`) changes its requirements or the way it bundles Python dependencies (Zipapp).
3.  **Bootstrap Argument Changes**: If the `cephadm bootstrap` command adds or deprecates flags relevant to development (like `--shared_ceph_folder`).
4.  **Workflow Architecture**: If the internal naming convention for `daemon_id` or `service_name` is modified, as this affects how developers debug their environments.
5.  **Dashboard Build Pipeline**: If the frontend build process (`npm run build`) or the directory structure for MGR modules changes.