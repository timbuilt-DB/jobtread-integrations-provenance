# Session A - Capture Layer Implementation (Derived)

> **Derived from canonical capsule:** `timbuilt-DB/Automation_Protocols/active/2025-11-28--session-a-capture-layer.md`\n> This file is a derived summary for provenance only. Canonical source of truth lives in `Automation_Protocols`.

Date: 2025-11-28
Session: A (Capture Layer)
Status: draft
Version: v1
Capsule Type: capture-derived

This capsule syncs the Session A Capture Layer design into the JobTread Integrations provenance repo as a derived view. It must not be edited as the primary spec; instead, edits should be applied to the canonical capsule in `Automation_Protocols` and optionally reflected here.

---

## 1. Airtable Intake Schema (Summary)

See canonical capsule for full details.

- Base ID: `appkXIMhPS34Lm7tF`
- Table: `IntakeItems`
- Key fields: `source_id`, `notes`, `author`, `job_id`, `job_name`, `artifact_type`, `created_at`, `room`, `element`, `file_url`, `Attachments`, `Attachment Summary`.
- Known drift: filter formula referenced `jobid` while Airtable field is `job_id`.

---

## 2. Source Taxonomy (Summary)

### `source_type` enum (examples)
- `fireflies.meeting`
- `bubbles.note`
- `email.inbound`
- `photo.ocr`
- `jt.daily_log`
- `jt.other`
- `manual`
- `other`

### `artifact_type` enum (examples)
- `meeting_transcript`
- `meeting_summary`
- `voice_note`
- `email_message`
- `photo`
- `photo_ocr_text`
- `jt_daily_log`
- `document`
- `other`

---

## 3. CapturePayload v1 (Summary)

CapturePayload v1 is the canonical ingestion schema used by all capture routes before mapping into Airtable. It defines fields such as:

- `source_type`, `source_system`, `source_id`, `artifact_type`
- `job_id`, `job_name`
- `author`, `created_at`
- `room`, `element`
- `notes`, `file_url`, `attachments[]`
- `raw_context`, `capture_version`, `debug_trace_id`

See canonical capsule for the exact JSON schema and mapping rules.

---

## 4. Capture Routes (Summary)

This derived capsule records the existence of five capture routes, with full specs in the canonical capsule:

1. Fireflies → Make → Airtable
2. Bubbles → Make → Airtable
3. Email → IntakeItems
4. Photos/OCR → IntakeItems
5. JobTread Daily Logs → IntakeItems

Each route:
- Normalizes its source into CapturePayload v1.
- Upserts (or dry-runs) into Airtable IntakeItems keyed by `source_id`.

---

## 5. Drift Guard Blueprint (Summary)

The canonical capsule defines drift guards for:

- Airtable schema (IntakeItems)
- Make scenario blueprints
- CapturePayload schema

This file simply notes their existence; implementation details and updates must be maintained in `Automation_Protocols`.

---

## 6. Governance Note

This file exists only as a provenance-friendly summary.

- Do not add new canonical design decisions here.
- Do not treat this as the source of truth for Intake/SOW or Capture.
- For any edits or new behavior, update the canonical capsule in `Automation_Protocols/active/` and then, if needed, refresh this summary via PR.