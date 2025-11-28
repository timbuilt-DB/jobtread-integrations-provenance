---
title: "Intake/SOW Capsule Index"
date: "2025-11-28"
role: "Session F — Intake/SOW Operations & Observability"
---

# Intake/SOW Capsule Index

Single directory for all Intake/SOW-related capsules across the provenance and automation repos. Update this file whenever a new Intake/SOW capsule is added.

## jobtread-integrations-provenance

- `.capsules/capture/2025-11-28-session-a-capture-layer.md` — Capture layer overview and design. **Owner:** Session A.
- `.capsules/capture/2025-11-28-email-capture-make-blueprint.md` — Email → Make capture blueprint for Intake/SOW. **Owner:** Session A.
- `.capsules/capture/2025-11-28-capture-fixtures.md` — ZZ job capture fixtures for Intake/SOW pipeline. **Owner:** Session A.
- `.capsules/ai-layer/2025-11-28-session-b-ai-layer-overview.md` — AI layer design and mapping for Intake/SOW. **Owner:** Session B.
- `.capsules/ai-layer/2025-11-28-ai-layer-tests.md` — Test strategy for AI classification/mapping/sectioning. **Owner:** Session B.
- `.capsules/intake-sow/2025-11-28-phase-status-dashboard.md` — Intake/SOW phase status dashboard. **Owner:** Session F.
- `.capsules/intake-sow/2025-11-28-operator-runbook.md` — Intake/SOW operator runbook. **Owner:** Session F.
- `.capsules/intake-sow/2025-11-28-health-check-bundle-spec.md` — Intake/SOW health check bundle spec. **Owner:** Session F.

## Jobtread-Integrations

- `.capsules/sow-engine/2025-11-28-sow-draft-builder.md` — SOW draft builder design. **Owner:** Session C.
- `.capsules/sow-engine/2025-11-28-sow-sectioning.md` — SOW sectioning strategy. **Owner:** Session C.
- `.capsules/sow-engine/2025-11-28-sow-publisher-airtable.md` — Publish SOWs into Airtable. **Owner:** Session C.
- `.capsules/sow-engine/2025-11-28-sow-publisher-jt.md` — Publish SOWs back into JobTread. **Owner:** Session C.
- `.capsules/sow-engine/2025-11-28-capsule-publisher.md` — Capsule publisher for SOW artifacts. **Owner:** Session C.
- `.capsules/sow-engine/2025-11-28-sow-drift-guards.md` — Drift guards for Intake/SOW engine. **Owner:** Session C.

## How to add a new capsule

1. Create the capsule in the appropriate repo using the `YYYY-MM-DD-topic-slug.md` naming convention under a relevant directory (e.g. `capture`, `ai-layer`, `sow-engine`, `intake-sow`).
2. Add a new bullet to the correct repo section above with:
   - The relative path to the capsule.
   - A one-line description of what it covers.
   - The owning session (A, B, C, D, F) or future team name.
3. If the capsule changes operational behavior (CI, JT automation, Make scenarios), consider also updating:
   - `.capsules/intake-sow/2025-11-28-phase-status-dashboard.md`
   - `.capsules/intake-sow/2025-11-28-operator-runbook.md`

This index is the canonical entry point for Intake/SOW capsules; other docs should link back here instead of duplicating lists.
