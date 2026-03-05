This documentation file, `basic-workflow.rst`, serves as the definitive onboarding guide for developers contributing to the Ceph project. It outlines the lifecycle of a contribution from identifying an issue to the final merge.

### 1. Primary Purpose
The file documents the **standard operational procedures (SOP)** for contributing code and documentation to the Ceph ecosystem. It defines the relationship between upstream repositories, personal forks, and local development environments, ensuring that all contributors follow a uniform version control and reporting discipline.

### 2. Key Topics Covered
*   **Issue Tracking**: Requirements for using the Redmine-based Ceph tracker and rules for when a ticket is mandatory (e.g., major doc changes vs. simple cleanups).
*   **Repository Architecture**: The "Three-Repo Strategy" involving Upstream (`ceph/ceph`), Fork (`user/ceph`), and Local Working Copy.
*   **Environment Setup**: Instructions for cloning, configuring Git identity, and setting up remotes.
*   **Development Lifecycle**: Synchronizing with upstream `main`, branch creation, committing with signed-off tags, and pushing changes.
*   **Pull Request (PR) Management**: Guidelines for opening PRs, understanding automated CI (Jenkins/Make Check), and handling peer reviews.
*   **Validation & Testing**: Introduction to unit tests (`make check`) and integration testing via the `teuthology` framework and the Sepia lab.
*   **Post-Submission**: Procedures for amending PRs (rebasing/force-pushing), merge commit formatting, and updating issue tracker statuses (Resolved/Pending Backport).

### 3. Technical Keywords
*   **Version Control**: `git clone`, `git remote add ceph`, `git fetch`, `git reset --hard`, `git commit -as` (sign-off), `git push --force`.
*   **Infrastructure/CI**: `Jenkins`, `make check`, `teuthology`, `Sepia lab`, `GitHub Pull Request`.
*   **Tools**: `Redmine` (Issue Tracker), `ptl-tool.py` (Merge tool), `Ceph GitHub Helper Extension`.
*   **Process Terms**: `Upstream`, `Fork`, `Signed-off-by`, `Backport`, `Rebase`, `needs-qa`.

### 4. Target Audience
*   **New Contributors**: Developers or technical writers looking to make their first contribution.
*   **Experienced Maintainers**: Project leads needing a reference for proper merge commit formats or automated CI triggers.
*   **Internal Teams**: QA and Dev teams needing to understand the integration of `teuthology` and `ceph-qa-suite` into the dev flow.

### 5. Related Concepts
*   **Testing Suites**: Closely related to `dev-testing-unit-tests` and `integration-testing-teuthology-intro`.
*   **Release Management**: Relates to the `backporting` and `merging` chapters for stable release cycles.
*   **Governance**: References Ceph’s `Submitting Patches` policy regarding legal and community standards.

---

### When to Update This File
This file should be updated if any of the following changes occur in the Ceph development ecosystem:
1.  **Tooling Shifts**: If Ceph moves from Redmine to another issue tracker (e.g., GitHub Issues) or replaces Jenkins with a different CI provider (e.g., GitHub Actions).
2.  **Workflow Modifications**: If the project changes its primary branch name (e.g., from `main` to something else) or alters the "single commit per PR" best practice.
3.  **Governance Changes**: If the requirements for `Signed-off-by` tags or the format of merge commits are redefined.
4.  **Integration Testing Paths**: If the process for requesting access to the Sepia lab or triggering `teuthology` suites changes.
5.  **Automation**: If new scripts (like `ptl-tool.py`) are introduced or deprecated for handling PRs and merges.