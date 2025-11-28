---
title: "Intake/SOW Operator Runbook"
date: "2025-11-28"
role: "Session F — Intake/SOW Operations & Observability"
---

# Intake/SOW Operator Runbook

This runbook describes how to operate the Intake/SOW pipeline for the ZZ fixture job and future jobs. It assumes automation lives in:

- `timbuilt-DB/jobtread-integrations-provenance` (design, capsules, fixtures)
- `timbuilt-DB/Jobtread-Integrations` (automation, CI workflows, publishers)

## 1. How to run a full Intake/SOW dry-run for job `22PKCUGRwQGg`

> Goal: starting from existing JT data for job `22PKCUGRwQGg`, produce a SOW draft + logs **without** writing back to JT or Airtable.

### Step 1 — Run Capture in dry-run

- Ensure capture capsules are up to date (see capsule index):
  - `.capsules/capture/2025-11-28-session-a-capture-layer.md`
  - `.capsules/capture/2025-11-28-email-capture-make-blueprint.md`
  - `.capsules/capture/2025-11-28-capture-fixtures.md`
- Use the documented trigger in the capture capsule (Make scenario or GitHub Action) to run capture in **dry-run** mode.
- Expected outputs:
  - Normalized text/JSON for JT files and daily logs for `22PKCUGRwQGg`.
  - Stored either as GitHub artifacts or in a known fixtures directory in the provenance repo.

### Step 2 — Run AI-layer tests

- Once AI-layer tests exist (Session B):
  - Run the AI-layer CI workflow in `Jobtread-Integrations` (see AI-layer capsules for workflow name).
  - Confirm tests pass for the ZZ job fixture and any additional jobs configured.
- Expected outputs:
  - Test reports (pass/fail) for classification, mapping, and sectioning.
  - Optional drift summary relative to reference fixtures.

### Step 3 — Run SOW engine dry-run workflow

- Trigger the SOW engine dry-run workflow in `Jobtread-Integrations` (GitHub Action).
- Ensure the workflow is configured to:
  - Use capture/AI outputs for job `22PKCUGRwQGg`.
  - Produce SOW draft markdown and associated metadata **as artifacts only** (no writes back to JT/Airtable).
- Expected outputs:
  - SOW draft markdown (per job) as a build artifact.
  - Optional JSON summary with scores and section-level metadata.

### Step 4 — Locate artifacts

- In `Jobtread-Integrations` CI:
  - Navigate to the dry-run workflow run.
  - Download artifacts containing:
    - SOW draft markdown.
    - Any drift/score reports.
- Optional follow-up (future): attach the SOW draft back to JobTread using a guarded publisher workflow.

## 2. What to check after CI runs fail

### Capture failures

- Check capture capsules for schema or fixture changes that did not update tests:
  - `.capsules/capture/2025-11-28-session-a-capture-layer.md`
  - `.capsules/capture/2025-11-28-capture-fixtures.md`
- Typical issues:
  - New JT file/daily-log types not covered by fixtures.
  - Schema changes to IntakeItems not reflected in capture normalization.

### AI-layer failures

- Check AI-layer capsules for updated logic vs tests:
  - `.capsules/ai-layer/2025-11-28-session-b-ai-layer-overview.md`
  - `.capsules/ai-layer/2025-11-28-ai-layer-tests.md`
- Typical issues:
  - Classification outputs changed but golden tests weren’t updated.
  - Mapping rules changed without updating reference fixtures.

### SOW engine failures

- Check SOW-engine capsules in `Jobtread-Integrations`:
  - `.capsules/sow-engine/2025-11-28-sow-draft-builder.md`
  - `.capsules/sow-engine/2025-11-28-sow-sectioning.md`
  - `.capsules/sow-engine/2025-11-28-sow-publisher-airtable.md`
  - `.capsules/sow-engine/2025-11-28-sow-publisher-jt.md`
  - `.capsules/sow-engine/2025-11-28-sow-drift-guards.md`
- Typical issues:
  - Schema mismatch between AI outputs and SOW engine inputs.
  - New sections drafted without updating drift/score expectations.

## 3. Where to log decisions

Use decision capsules in the provenance repo for Intake/SOW decisions that affect behavior. Suggested pattern:

- Directory: `.capsules/decisions/`
- Name: `YYYY-MM-DD-intake-sow-<short-topic>.md`
- Content: concise decision record, including:
  - Context and options considered.
  - Final decision.
  - Impact on capture/AI/SOW.
  - Any follow-up tasks or owners.

Link decision capsules from the phase status dashboard and relevant design capsules when they materially change behavior.

## 4. How to safely change Intake/SOW logic

1. **Touching capture**
   - Update capture capsules and schemas first.
   - Extend fixtures to cover the new cases.
   - Update or add CI checks to ensure new fixtures pass.

2. **Touching AI layer**
   - Update AI-layer design capsules describing the new behavior.
   - Add or adjust tests for classification/mapping/sectioning.
   - Ensure tests run in CI and are green before enabling new behavior in production workflows.

3. **Touching SOW engine**
   - Update SOW engine capsules (builder, sectioning, publishers, drift guards).
   - Verify dry-run workflow is green on ZZ fixture.
   - Document apply-mode rollout plan (including guardrails) in a decision capsule.

4. **When in doubt**
   - Prefer adding a new capsule or decision record over silently changing logic.
   - Keep this runbook and the phase status dashboard in sync with any behavior changes.
