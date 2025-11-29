# Intake/SOW Health-Check Bundle Spec (Derived)

> **Derived from canonical capsule:** `timbuilt-DB/Automation_Protocols/active/5xx_Pipelines/intake-sow-pipeline.md`\n> This file is a derived health-check spec for provenance only. Canonical source of truth for Intake/SOW pipeline and health checks lives in `Automation_Protocols`.

Date: 2025-11-28
Status: draft
Version: v1
Capsule Type: intake-sow-derived

This capsule describes, in a provenance-friendly way, the Intake/SOW health-check bundle. It must not be edited as the primary spec; instead, edits should be applied to the canonical capsules in `Automation_Protocols` and, if needed, reflected here.

---

## 1. Canonical Health-Check Definition

- Repository: `timbuilt-DB/Automation_Protocols`
- Path: `active/5xx_Pipelines/intake-sow-pipeline.md`

That capsule defines the authoritative Intake/SOW health-check bundle and its relationship to the pipeline.

---

## 2. Role of This Derived Capsule

- Summarizes the expected health checks for operators and provenance.
- Can be used by downstream tooling as a reference, but not as the source of truth.

For full definitions, thresholds, and CI wiring, refer to the canonical capsule.

---

## 3. Governance Note

- Do not add new canonical Intake/SOW health-check logic here.
- For changes to health-check behavior, update the canonical capsule in `Automation_Protocols`, then refresh this derived view via PR if needed.