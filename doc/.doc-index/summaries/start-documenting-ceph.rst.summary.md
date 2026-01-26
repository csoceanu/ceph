### 1. Primary Purpose
The `documenting-ceph.rst` file serves as the **official contribution guide and style manual** for the Ceph project’s documentation. It provides a standardized workflow for technical writers and developers to propose corrections, add new documentation, and ensure consistency across the Ceph documentation ecosystem.

### 2. Key Topics Covered
*   **Submission Methods**: Procedures for minor fixes (emailing `ceph-users@ceph.io` with "ATTN: DOCS") versus substantial changes (GitHub Pull Requests).
*   **Repository Structure**: Mapping of documentation source files to specific Ceph components (e.g., `doc/rados`, `doc/rbd`, `doc/cephfs`).
*   **Version Management**: Instructions on how to view documentation for different releases (Reef, Quincy, etc.) by modifying URLs.
*   **Contribution Workflow**: A step-by-step guide to the "Fork and Pull" model, including branch selection (`main`, `next`, or `wip-doc-` branches).
*   **Build Environment**: Detailed prerequisites and commands for building HTML and manpages using **Python Sphinx** and **Doxygen**.
*   **Commit Conventions**: Strict formatting rules for commit messages, including mandatory `doc:` prefixes and `Signed-off-by` tags.
*   **Technical Style Guide**: Standards for headers, line wrapping (80-character limit), internal/external hyperlinking, and the use of reStructuredText (RST) directives like `.. note::` and `.. tip::`.

### 3. Technical Keywords
*   **Tools**: Python Sphinx, Doxygen, Git, `ditaa`, `graphviz`, `ant`.
*   **Commands**: `admin/build-doc`, `admin/serve-doc`, `admin/build-doc linkcheck`, `git rebase -i main` (squashing).
*   **Configuration/APIs**: `.gitconfig`, `reStructuredText (RST)`, `wip-doc-` (branch naming convention).
*   **Directives**: `.. toctree::`, `:ref:`, `.. code-block::`, `.. versionadded::`, `.. deprecated::`.

### 4. Target Audience
*   **New Contributors**: Individuals looking to fix typos or improve clarity.
*   **Ceph Developers**: Engineers adding new features or configuration options who are required to document their changes.
*   **Release Managers**: Personnel managing documentation branches for upcoming or legacy releases.

### 5. Related Concepts
*   **Upstream Ceph Project**: The primary codebase this documentation supports.
*   **Ceph Tracker**: The issue tracking system (`tracker.ceph.com`) used to link documentation fixes to specific bugs.
*   **Continuous Integration (CI)**: The documentation build process (which breaks if hyperlinks are invalid).

---

### AI Update Triggers: When to update this file
An AI system should flag this file for updates if any of the following changes occur in the codebase or project infrastructure:

1.  **Repository Restructuring**: If the directory structure under `ceph/doc/` changes (e.g., adding a new component like a new storage engine or management tool).
2.  **Dependency Changes**: If the project switches from Sphinx to another documentation generator (e.g., MkDocs) or changes the required Python version/libraries.
3.  **Process Shifts**: If the project changes its mailing list address, subject line requirements, or moves from GitHub to a different hosting platform (e.g., GitLab).
4.  **Commit Policy Updates**: If the "Strict" requirements for commit messages change (e.g., changing the `doc:` prefix or the signing-off requirements).
5.  **Build Script Modifications**: If the scripts in `admin/` (like `build-doc` or `serve-doc`) are renamed, deprecated, or receive new mandatory flags.
6.  **Style Guide Evolution**: If the project decides to move away from the 80-character line limit or adopts new RST extensions.