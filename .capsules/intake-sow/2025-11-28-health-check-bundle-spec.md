---
title: "Intake/SOW Health Check Bundle Spec"
date: "2025-11-28"
role: "Session F — Intake/SOW Operations & Observability"
---

# Intake/SOW Health Check Bundle Spec

> Goal: a single tool/bundle that answers "Is Intake/SOW healthy right now?" with a simple traffic-light summary per layer. No writes, no apply-mode.

## 1. Proposed shape

A future MCP or GitHub Action tool should return a JSON payload of the form:

```json
{
  "capture": { "fixtures_ok": true, "schema_ok": true, "last_run": "2025-11-28T12:34:56Z" },
  "ai_layer": { "tests_pass": true, "drift_reports": [] },
  "sow_engine": { "dry_run_ok": true, "scores": { }, "drift": { } }
}
```

Internally, the tool can also compute a traffic light for each layer:

- `green` — everything passes and is recent.
- `yellow` — minor drift or stale runs, but not fundamentally broken.
- `red` — recent failures or missing required checks.

## 2. Inputs and dependencies

The health check bundle is read-only and may call into:

- GitHub APIs / MCP tools for:
  - Workflow run status and timestamps.
  - Test summaries (pass/fail) for capture, AI, and SOW engine workflows.
  - Artifacts with drift reports or score summaries.
- Make.com (optional) for:
  - Recent status of capture scenarios, if capture is Make-driven.
- Airtable / JobTread (optional, via MCP) for:
  - Verifying that reference fixtures / mapping tables exist and are reachable.

All calls should be defensive and treat missing data as a signal (e.g. "no recent run" becomes `yellow` or `red` depending on age).

## 3. Capture layer health

Suggested checks:

- **Fixtures present:**
  - Confirm ZZ job `22PKCUGRwQGg` fixtures exist in the provenance repo or as GitHub artifacts.
- **Schema alignment:**
  - If there is a canonical IntakeItems schema, verify it matches what capture produces (e.g. via a schema-validation CI job).
- **Recent run:**
  - Check the most recent successful capture workflow run for the ZZ job (and optionally a small set of other jobs).

Example rules:

- `green` if fixtures exist, schema check passes, and a successful run is newer than N days.
- `yellow` if fixtures exist but last successful run is older than N days or schema check is missing.
- `red` if fixtures are missing or last run failed.

## 4. AI layer health

Suggested checks:

- **Tests pass:**
  - AI-layer CI workflow for classification/mapping/sectioning is green on ZZ job and at least one additional job.
- **Drift under control:**
  - If drift reports are produced, ensure they are within expected thresholds (e.g. small number of changed labels vs reference).

Example rules:

- `green` if tests pass and drift reports show no critical changes.
- `yellow` if tests pass but drift reports indicate medium changes or are stale.
- `red` if tests fail or are missing for required jobs.

## 5. SOW engine health

Suggested checks:

- **Dry-run ok:**
  - SOW engine dry-run workflow is green on ZZ job.
- **Scores sane:**
  - If scoring/quality metrics are produced, they fall within expected ranges (as defined in SOW engine capsules).
- **Drift guards:**
  - Drift guard capsule and logic exist and are being exercised in CI.

Example rules:

- `green` if dry-run passes and scores are within expected bounds.
- `yellow` if dry-run passes but scores drift or drift guards report non-critical issues.
- `red` if dry-run fails or cannot produce a SOW draft.

## 6. Wiring into MCP / CI

A future implementation could expose this as:

- A dedicated GitHub Action workflow in `Jobtread-Integrations` (e.g. `intake-sow-health-check.yml`) which:
  - Calls underlying test workflows or reuses their last results.
  - Uses a small script to aggregate statuses into the JSON shape above.
  - Emits a machine-readable artifact (`health.json`) and a human-readable summary.
- An MCP bundle tool (e.g. `intake.get_intake_sow_health_bundle`) which:
  - Wraps the GitHub and Make calls described above.
  - Returns the same JSON structure.

## 7. Consumption

Operators and automations could use this health check to:

- Block apply-mode SOW publishing if any layer is `red`.
- Show a dashboard badge in a future UI (one light per layer).
- Drive alerts (e.g. if status flips from `green` to `red`).

This spec is intentionally implementation-agnostic so that Session D or a future automation pass can implement it using the existing GitHub / MCP tooling without redesigning the checks themselves.
