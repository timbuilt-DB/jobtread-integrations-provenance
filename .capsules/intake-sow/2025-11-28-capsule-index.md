# Intake/SOW Capsules Index (Derived)

> **Derived from canonical capsule:** `timbuilt-DB/Automation_Protocols/active/5xx_Pipelines/intake-sow-pipeline.md`\n> This file is a derived index for provenance only. Canonical source of truth for Intake/SOW pipeline lives in `Automation_Protocols`.

Date: 2025-11-28
Status: draft
Version: v1
Capsule Type: intake-sow-derived

This capsule provides a provenance-friendly index of Intake/SOW-related capsules. It must not be edited as the primary spec; instead, edits should be applied to the canonical capsules in `Automation_Protocols` and, if needed, reflected here.

---

## 1. Canonical Intake/SOW Pipeline Capsule

- Repository: `timbuilt-DB/Automation_Protocols`
- Path: `active/5xx_Pipelines/intake-sow-pipeline.md`

That capsule defines the end-to-end Intake/SOW pipeline, phases, and handoffs.

---

## 2. Related Derived Capsules in Provenance

The following files in this repo are derived views for provenance and operator convenience:

- `.capsules/intake-sow/2025-11-28-health-check-bundle-spec.md`
- `.capsules/intake-sow/2025-11-28-operator-runbook.md`
- `.capsules/intake-sow/2025-11-28-phase-status-dashboard.md`

Each of these files should be treated as a summary or operator-facing view, not as the canonical design.

---

## 3. Governance Note

- Do not add new canonical Intake/SOW design decisions here.
- For any changes to Intake/SOW behavior or pipeline shape, update the canonical capsule in `Automation_Protocols/active/5xx_Pipelines/intake-sow-pipeline.md`.
- After canonical changes are merged, this index and the related derived capsules can be refreshed as needed via PR.