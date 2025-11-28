---
title: "Intake/SOW Phase Status Dashboard"
date: "2025-11-28"
role: "Session F — Intake/SOW Operations & Observability"
---

# Intake/SOW Phase Status Dashboard

This capsule is the single-pane status view for Intake/SOW as of **2025-11-28**. It summarizes layers, ownership, and what is done vs in progress vs next.

## Layer Summary

| Layer | Repo(s) | Status | Notes |
| --- | --- | --- | --- |
| Capture | `jobtread-integrations-provenance` | A: partially automated | Email stream spec + fixtures done; ZZ job daily-log fixture planned as input |
| AI Layer | `jobtread-integrations-provenance` / `Jobtread-Integrations` | B: design done, impl WIP | AI-layer capsules drafted; mapping/tests to be wired into CI |
| SOW Engine | `Jobtread-Integrations` | C: capsules ready, engine WIP | SOW draft builder + publisher capsules drafted; dry-run path being implemented |

## Phase 2 checklist

- [ ] Capture: schemas finalized and versioned.
- [ ] Capture: fixtures complete for ZZ job `22PKCUGRwQGg`.
- [ ] Capture: CI checks for schema/fixture drift.
- [ ] AI: engine implemented behind stable interfaces.
- [ ] AI: tests (classification/mapping/sectioning) integrated into CI.
- [ ] SOW: dry-run workflow green on ZZ fixture.
- [ ] SOW: apply-mode guarded rollout plan agreed.

## Links and references

See `.capsules/intake-sow/2025-11-28-capsule-index.md` for the full cross-repo capsule index.
