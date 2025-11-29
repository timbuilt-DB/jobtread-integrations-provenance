# Intake/SOW Phase Status Dashboard (Derived)

> **Derived from canonical capsule:** `timbuilt-DB/Automation_Protocols/active/5xx_Pipelines/intake-sow-pipeline.md`\n> This file is a derived dashboard view for provenance only. Canonical source of truth for Intake/SOW phase definitions and status logic lives in `Automation_Protocols`.

Date: 2025-11-28
Status: draft
Version: v1
Capsule Type: intake-sow-derived

This capsule describes a phase status dashboard for the Intake/SOW pipeline in a provenance-friendly way. It must not be edited as the primary spec; instead, edits should be applied to the canonical capsule in `Automation_Protocols` and, if needed, reflected here.

---

## 1. Canonical Phase Definitions

- Repository: `timbuilt-DB/Automation_Protocols`
- Path: `active/5xx_Pipelines/intake-sow-pipeline.md`

That capsule defines the phases, transitions, and status semantics for Intake/SOW.

---

## 2. Role of This Derived Capsule

- Provides a dashboard-oriented view of those phases and statuses.
- Useful for operators and provenance, but not as the canonical definition.

For complete logic and evolution of phases, refer to the canonical capsule.

---

## 3. Governance Note

- Do not add new canonical Intake/SOW phase definitions here.
- For changes to phases or status logic, update the canonical capsule in `Automation_Protocols`, then refresh this derived view via PR if desired.