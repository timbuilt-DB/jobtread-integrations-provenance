# Capsules in This Repo

**Important governance note**: This `.capsules/` directory in `timbuilt-DB/jobtread-integrations-provenance` contains only **derived** or **snapshot** views of canonical capsules that live in `timbuilt-DB/Automation_Protocols`.

- Canonical capsules (architecture, Master Map, Intake/SOW, Capture Layer, runbooks, policies, CI specs) live in:\n  - `timbuilt-DB/Automation_Protocols/active/**`
- This repo is allowed to contain:
  - Derived capsule summaries for provenance and operator convenience.\n  - Fixtures under `fixtures/**` for CI/tests.\n  - Provenance artifacts such as SBOMs, manifests, and release metadata.

### Rules for Humans and Agents

1. **Do not author new canonical capsules here.**
   - New designs, pipelines, and policies must be created in `Automation_Protocols/active/**`.

2. **Treat every file under `.capsules/` as derived.**
   - Each capsule should either:
     - Explicitly state which canonical Automation_Protocols file it is derived from, or\n     - Be treated as read-only documentation that must not diverge from Automation_Protocols.

3. **When editing behavior or design:**
   - First update the appropriate capsule(s) in `Automation_Protocols`.
   - Then, if needed, refresh the derived view here via PR.

4. **When in doubt:**
   - Specs and canonical logic → `Automation_Protocols`.\n   - Fixtures and provenance-only views → this repo.

For more detail, see governance capsule:\n- `timbuilt-DB/Automation_Protocols/active/PROVENANCE-REPO-GOVERNANCE.md`
