This documentation file serves as the foundational technical guide for **Teuthology**, the integration testing framework for Ceph. It defines how complex, multi-machine tests are structured, scheduled, and executed.

### 1. Primary Purpose
The file provides a high-level and functional introduction to Ceph’s integration testing infrastructure. It explains how the system consumes build packages, how test suites are composed using YAML "facets," and the logistics of running these tests in the Sepia lab or private clusters.

### 2. Key Topics Covered
*   **Package Consumption**: How Teuthology tests actual RPM/DEB packages on supported distros (RHEL/CentOS, Ubuntu).
*   **The "Nightlies"**: Overview of the automated periodic testing and the **Pulpito** dashboard for results.
*   **Job Priority**: A detailed breakdown of the `-p` flag values (0 to 1000) and how they impact queue placement.
*   **Suite Inventory**: Descriptions of core test suites (e.g., `rados`, `rbd`, `fs`, `upgrade`, `powercycle`).
*   **Test Anatomy**: The structure of "singleton" tests vs. directory-based suites, including the role of `roles` and `tasks`.
*   **YAML Composition Operators**: Technical explanation of how `%` (convolution), `+` (concatenation), and `$` (random selection) build test matrices.
*   **Upgrade Testing**: Specific logic for testing 2-x release upgrades in parallel with workloads.

### 3. Technical Keywords
*   **Commands**: `teuthology-suite`, `teuthology-describe`, `make check`.
*   **Operators**: `%` (convolution), `+` (concatenation), `$` (random selection).
*   **Configuration**: `roles`, `tasks`, `yaml`, `facets`, `--filter`, `--subset`, `--limit`, `--dry-run`, `--priority`.
*   **Infrastructure**: Sepia Lab, Pulpito, `smithi` (machine type), `ceph/qa` directory.
*   **Tasks**: `install`, `ceph`, `admin_socket`, `parallel`.

### 4. Target Audience
*   **New Ceph Developers**: Learning how to validate code changes before submission.
*   **QA Engineers**: Understanding how to construct and schedule test matrices.
*   **Release Managers**: Gauging priority levels for urgent release validation.

### 5. Related Concepts
*   **Upstream CI/CD**: Relates to the Ceph build pipeline that produces the packages Teuthology consumes.
*   **Orchestration**: Relates to `cephadm` or `ceph-deploy`, as these are often tasks within the tests.
*   **Regression Testing**: The primary goal of the "Nightlies" and the `upgrade` suites.

---

### Triggering Updates: When to Edit This File
An AI system or maintainer should update this documentation if any of the following code or structural changes occur:

1.  **Framework Command Changes**: If the `teuthology-suite` CLI arguments change (e.g., adding new filtering logic or changing the default priority).
2.  **QA Directory Restructuring**: If the organization of `ceph/qa/suites` is modified or if new core suites are added that warrant inclusion in the "Suites Inventory."
3.  **New Composition Logic**: If the Teuthology framework introduces a new operator (similar to `%` or `+`) for YAML manipulation.
4.  **Platform Support**: If the officially supported distros (currently listed as Ubuntu 18.04/24.04 and RHEL/CentOS 8/10) are updated or deprecated.
5.  **Task API Changes**: If the standard way of defining a `task()` in Python (within `qa/tasks/`) changes, specifically affecting how it reads the YAML input structure.
6.  **Infrastructure Shifts**: If the community moves away from the Sepia Lab or Pulpito for result reporting.