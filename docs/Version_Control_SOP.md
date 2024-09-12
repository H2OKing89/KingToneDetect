# **Version Control SOP for Documentation:**

1. **Versioning Structure**:
   - Use **semantic versioning** for your documents, similar to how software versions are handled. A typical version format is:
     - **Major Version**: Incremented for significant updates or restructuring (e.g., 2.0.0).
     - **Minor Version**: Incremented for new sections, features, or non-breaking changes (e.g., 1.1.0).
     - **Patch Version**: Incremented for small edits, typo fixes, or clarifications (e.g., 1.0.1).
   - Example: `v1.1.0` signifies the first major update, and the first minor change or feature addition.

2. **Changelog**:
   - **Changelog** should appear at the top of each document.
   - Keep a detailed **changelog** section in the documentation to track what changes have been made. Each change should be timestamped and signed off (i.e., who made the change).
   - Example format for a changelog entry:
     ```markdown
     ### Changelog
     - **v1.0.1** (September 13, 2024): Minor corrections in workflow overview, fixed typos in "Simultaneous Tone Detection".
     - **v1.0.0** (September 11, 2024): Initial release of documentation covering core workflow, error handling, and heartbeat functionality.
     ```

3. **Commit Message Standards**:
   - When updating documents on GitHub (or any version control system), use clear, concise commit messages that explain **what** was changed and **why**.
   - Example commit messages:
     - “Updated workflow section to include Heartbeat monitoring changes.”
     - “Fixed incorrect tone detection logic in version 1.0.2.”

4. **Change Descriptions**:
   - For each update, explain:
     - **What was changed**: Specific sections or features updated.
     - **Why it was changed**: The reasoning behind the modification or addition.
     - **How it impacts the project**: Any cascading effects or dependencies that need to be watched (e.g., changes that affect config file structure).

5. **Review Process**:
   - Before finalizing changes, document what has been **reviewed** and whether a **peer review** was conducted.
   - Mark the document as **draft** or **final** based on whether the review is complete.
   - Use tags such as `pending review`, `reviewed`, and `finalized` within the changelog or commit history.

6. **Document Sections**:
   For each version, include:
   - **Version Number**: Clearly labeled in the title and changelog.
   - **Date**: When the version was finalized.
   - **Author**: Who made the changes.
   - **Summary of Changes**: A brief summary of what was added, removed, or updated.

---

### **Example Version Control Section in Documentation**:

```markdown
# KingToneDetect Documentation

## Version Control

### Changelog
- **v1.1.0** (September 20, 2024): Added new section on error handling, updated workflow logic for parallel notifications.
- **v1.0.1** (September 13, 2024): Fixed formatting in Disk Space Exhaustion scenario. Added comments to tone detection logic.
- **v1.0.0** (September 11, 2024): Initial release covering tone detection, notifications, and heartbeat.

### Document History

| Version  | Date               | Author           | Description                                                       |
|----------|--------------------|------------------|-------------------------------------------------------------------|
| v1.1.0   | September 20, 2024  | Quentin King      | Added error handling and notification logic.                      |
| v1.0.1   | September 13, 2024  | Quentin King      | Minor corrections in tone detection and notification handling.    |
| v1.0.0   | September 11, 2024  | Quentin King      | Initial document release.                                         |

```

---

### **Procedures for Implementing Changes**:

1. **Identify Need for Change**:
   - Changes can be prompted by new features, bug fixes, or optimizations. Identify the specific area in the documentation or project that requires an update.

2. **Draft Changes**:
   - Implement the necessary changes in the **draft** version of the document.
   - **Use placeholders** for sections that need further input and mark these sections as `TBD` (To Be Determined).
   
3. **Review and Approve**:
   - Once changes are drafted, review the document internally or with collaborators. If you’re working with multiple contributors, this is where a **reviewer** or **approver** comes in.
   - Add **review comments** as necessary before finalizing.

4. **Update Version Number**:
   - Increment the version number based on the scale of the change:
     - Major updates: Increase the major version.
     - Minor updates: Increase the minor version.
     - Patch: For small corrections.
   
5. **Log Changes**:
   - Update the **changelog** and version control section to reflect what has changed. Ensure all relevant details (who, what, why) are captured.

6. **Push to GitHub**:
   - Once the document is finalized, push it to GitHub, ensuring the changelog and commit messages match the updates.
   - Make sure that the README or any other associated documents reference the current version of the workflow or planning documents.

7. **Announce Changes**:
   - If necessary, notify collaborators or users of major updates, especially if they affect workflow or configuration files (e.g., changes in `config.ini`).

---

### **Information to Include in Version Updates**:
- **Version Number**: Update to reflect semantic versioning.
- **Date**: The date when the update was finalized.
- **Change Description**: A summary of what has been updated (new sections, revisions, or deletions).
- **Author/Editor**: The person responsible for the change.
- **Review Status**: Whether the change has been reviewed and approved, or if it’s still a draft.

