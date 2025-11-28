# Email Capture Blueprint - Make Scenario (Dry-Run)

Date: 2025-11-28
Session: A (Capture Layer)
Status: draft
Version: v1
Capsule Type: capture-blueprint

This capsule defines the email → IntakeItems capture route as a Make scenario in dry-run mode. It uses CapturePayload v1 and targets the Airtable `IntakeItems` table in base `appkXIMhPS34Lm7tF`.

---

## 1. Scope

- Stream: **Email → IntakeItems**
- Mode: **dry-run** (no permanent Airtable writes; upserts flagged as dry-run)
- Goal: Prove that one capture route can run end-to-end and produce normalized IntakeItems consistent with CapturePayload v1 and the Airtable schema.

---

## 2. Make Scenario: `capture-email-to-intakeitems`

### 2.1 Trigger
- Module: Email (Gmail/Outlook/IMAP) "Watch emails"
- Filter: messages sent to the designated Intake alias (e.g., `intake@...`).

### 2.2 Normalize to CapturePayload v1

1. **Parse email**
   - Extract: `messageId`, `from`, `to`, `subject`, `body_text`, `body_html`, attachments.

2. **Optional Intake AI call**
   - Call `intake.ai_structured_extract` with:
     - `text = subject + '

' + body_text`
     - `source = "Email"`
     - `mode = "intake-items"`
   - If multiple items are returned, we create one CapturePayload per item.

3. **Build CapturePayload object(s)**

For each logical IntakeItem derived from the email:

- `source_type`: `email.inbound`
- `source_system`: `email`
- `source_id`: `email:<messageId>` or `email:<messageId>:<index>` if multiple items
- `artifact_type`: `email_message`
- `author`: sender email (From)
- `created_at`: email sent timestamp in ISO 8601
- `job_id` / `job_name`: parsed from subject/body if present (e.g., `[22PKCUGRwQGg]`)
- `room`, `element`: optional, from structured extract if available
- `notes`: structured summary or full body text
- `file_url`: link back to the email or storage location if any
- `attachments`: list of attachment descriptors (url, filename, content_type)
- `raw_context`: JSON blob with raw email headers and IDs
- `capture_version`: `capture_payload_v1`
- `debug_trace_id`: unique ID for this run, e.g. `cap-email-<timestamp>-<uuid>`

In Make, this is typically implemented as:
- A JSON aggregator/transform module that emits an array of CapturePayload objects for downstream use.

---

## 3. Airtable Upsert (Dry-Run)

### 3.1 Mapping to `IntakeItems`

For each CapturePayload:
- `source_id` → Airtable `source_id`
- `notes` → `notes`
- `author` → `author`
- `job_id` → `job_id`
- `job_name` → `job_name`
- `artifact_type` → `artifact_type`
- `created_at` → `created_at`
- `room` → `room`
- `element` → `element`
- `file_url` → `file_url`
- `attachments` → `Attachments` (converted to Airtable attachment objects)

### 3.2 Dry-Run behavior

- The Make module calling Airtable uses the MCP Airtable integration (or a gateway) set to **dry-run mode**, meaning:
  - No production Airtable records are permanently modified.
  - The system logs which upserts *would* be performed.
  - The CapturePayload and the would-be Airtable rows are emitted for inspection/logging.

Implementation detail:
- A runtime flag (e.g., `mode = "dry-run"`) is passed alongside the CapturePayload.
- When `mode=dry-run`, the scenario writes to a log or dev/test base instead of the production Intake base, or uses a gateway endpoint that simulates Airtable upsert.

---

## 4. CI: Blueprint Drift Check

This scenario participates in blueprint drift checking via `make.get_blueprint_drift_bundle`.

### 4.1 Canonical blueprint storage
- The canonical Make blueprint for `capture-email-to-intakeitems` is stored in a code repo under:
  - `make/blueprints/capture-email-to-intakeitems.json`

### 4.2 CI workflow (conceptual)

1. CI job loads the canonical blueprint JSON from Git.
2. CI calls `make.get_blueprint_drift_bundle` with:
   - `scenarioId` = ID of `capture-email-to-intakeitems` in Make
   - `canonical_blueprint` = contents of the JSON file
3. The drift bundle computes differences in:
   - modules present/removed
   - connections between modules
   - field mappings to CapturePayload and Airtable
4. CI fails if:
   - Any module that affects CapturePayload shape or Airtable writes changes without a corresponding PR update to the canonical blueprint.

---

## 5. Flip Path: Dry-Run → Apply

This capsule explicitly documents, but does not execute, the path to move from dry-run to apply-mode.

### 5.1 Flag-based control
- Introduce a configuration flag (e.g., `CAPTURE_EMAIL_MODE`) with allowed values: `dry-run`, `apply`.
- In the Make scenario, branch behavior based on this flag:
  - `dry-run`: log payloads and intended Airtable changes only.
  - `apply`: perform real Airtable upserts using the same mapping, with idempotency by `source_id`.

### 5.2 Governance guardrails
- Require:
  - CI green on CapturePayload schema tests.
  - CI green on Airtable schema drift guard (especially `job_id`).
  - CI green on Make blueprint drift guard.
- Only after those checks pass can the flag be flipped from `dry-run` to `apply` in production.

### 5.3 Provenance linkage
- This capsule references the Session A Capture Layer capsule (`2025-11-28-session-a-capture-layer.md`) as its parent design artifact.
- Any change to this blueprint must update both the canonical blueprint JSON and this capsule to keep provenance in sync.

---

## 6. Success Condition

- When this scenario runs in dry-run mode against test email traffic:
  - It produces CapturePayload objects matching the v1 schema.
  - It produces would-be IntakeItems rows consistent with the Airtable `IntakeItems` schema.
  - CI is able to compare the live Make scenario to the canonical blueprint and flag drift.

At that point, Session A's requirement for a single runnable capture stream (email) in dry-run mode is satisfied.
