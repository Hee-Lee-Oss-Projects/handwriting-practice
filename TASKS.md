# TASKS — handwriting-practice

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

## How these tasks map to Hee-Lee Oss

Each task below becomes a Hee-Lee Oss **Task JSON** validated against
`packages/schema/src/schemas.ts`. Field mapping:

- `id` — stable slug from the tables (e.g. `handwriting-practice-schema-001`).
- `title` — the table's Title.
- `project` — `handwriting-practice`.
- `type` — one of `code | research | writing | data | design-spec | maintenance` (per row).
- `lane` — `donated` for all tasks here (no funded escrow planned). A funded task would add
  `fundedBudgetUsd`.
- `priority` — `high | medium | low`.
- `domain` — array, e.g. `["education","literacy","accessibility","open-content"]`.
- `riskTier` — `low | medium | high`. **Latin = low; non-Latin script content = medium**
  (script-literate reviewer required). No `high` tasks (no clinical claims).
- `urgent` — boolean; `false` for all current tasks.
- `deliverable` — `pr | dataset | document | translation`. Code → `pr`; models/docs/sheets →
  `document`. We never use `dataset` (this project is content, not a dataset).
- `tokenEstimate` — `small | medium | large` (Size column).
- `status` — `open | in-progress | review | delivered | done`; all start `open`.
- `context`, `objective`, `acceptanceCriteria[]`, `resources[]`, `output` — per task.
- `requestor` — **TO BE SECURED** until a partner/educator is confirmed.
- `verifiedNeed` — **`false`** until a named educator/org agrees to adopt sheets (general need
  is real; per-deliverable adoption is unproven). Flips to `true` per task only on confirmation.
- `outputLicense` — **MIT** for code; **CC-BY-4.0** for formation models, templates, and PDFs
  (**CC-BY-SA-4.0** where a share-alike source compels it).

Reviewer roles referenced below: **Technical**, **Provenance** (license/provenance gate),
**Script** (script-literate reviewer, per non-Latin script), **Pedagogical**, **Accessibility**,
**Maintainer**, **Steward** (last-mile adoption).

---

## Milestone M0 — Foundation & cold-start

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| handwriting-practice-schema-001 | Formation-model JSON Schema + canonical data model | design-spec | medium | low | document | — | Technical |
| handwriting-practice-gate-002 | Font + source license/provenance gate (blocking) | design-spec | small | medium | document | — | Provenance |
| handwriting-practice-font-003 | Verify + vendor one open Latin font through the gate | research | small | medium | document | gate-002 | Provenance |
| handwriting-practice-reviewer-004 | Name/secure formation reviewer role (blocking gate role) | research | small | low | document | — | Maintainer |
| handwriting-practice-renderer-005 | Deterministic SVG renderer (model + template → SVG) | code | medium | low | pr | schema-001 | Technical |
| handwriting-practice-pdf-006 | SVG→PDF pipeline + A4/Letter paper sizing | code | medium | low | pr | renderer-005 | Technical |
| handwriting-practice-pilot-007 | 5-glyph Latin pilot sheet, sourced + reviewed, end-to-end | data | medium | low | document | schema-001, gate-002, font-003, reviewer-004, renderer-005, pdf-006 | Pedagogical, Provenance |
| handwriting-practice-outreach-008 | Partner/educator outreach + adoption-path shortlist | research | small | low | document | — | Steward |

**Acceptance criteria — key tasks**

- **schema-001 (formation-model schema)**
  - [ ] JSON Schema defines `ScriptModel`, `GlyphModel`, `Stroke`, `GuidelineSystem`,
        `SheetTemplate`, `StyleVariant`, and `AssetProvenance` with all fields from PLAN §6.
  - [ ] `GlyphModel` includes ordered `strokes[]` (path, order, startPoint, directionHint),
        `guidelineAlignment`, optional `positionalForms`, and a required `sourceRef`.
  - [ ] `ScriptModel` includes `script` (ISO 15924), `direction` (ltr|rtl), `font`
        (`AssetProvenance`), `reviewers[]`, and `schemaVersion`.
  - [ ] `AssetProvenance` captures license `{id,url,permitsRedistribution,permitsDerivatives,
        reservedFontName?,snapshotRef}`, attribution, retrievedAt.
  - [ ] Schema is versioned; spec doc licensed CC-BY-4.0; at least one worked example model
        validates against it; commit DCO signed-off.

- **gate-002 (license/provenance gate)**
  - [ ] Enumerates accepted licenses (OFL, CC0/PD, CC-BY, CC-BY-SA with SA handling) and
        excluded ones (proprietary, no-embed/no-redistribution, NC-only).
  - [ ] **Default-deny:** an asset PASSES only if an open license permitting redistribution
        (+ derivatives where the model overlays/modifies) is recorded with a cited URL; missing
        or unparseable evidence ⇒ FAIL (no default-allow).
  - [ ] Requires a committed license **snapshot** (local copy + SHA-256 + Wayback URL) per asset.
  - [ ] Encodes OFL Reserved Font Name rule and the CC-BY-SA share-alike/NC-exclusion policy.
  - [ ] Produces a committed PASS/FAIL artifact per asset and a CI check that fails the build on
        any unverified referenced asset.

- **pilot-007 (5-glyph end-to-end pilot)**
  - [ ] Models 5 Latin glyphs (e.g. `a c o l 1`) against the schema with per-glyph `sourceRef`
        to an authoritative formation source.
  - [ ] Glyph outlines come only from the gate-verified open font; gate-002 artifact attached.
  - [ ] Renders a deterministic trace sheet in **A4 and US Letter** with a 4-line guideline
        system; golden-file SVG match in CI.
  - [ ] Formation reviewer signs the per-glyph checklist (order/direction correct vs source).
  - [ ] PDFs + model committed CC-BY-4.0; every sheet footer carries attribution + font license.
  - [ ] Handed to an educator from outreach-008 if available; otherwise published to repo and
        marked `verifiedNeed:false` with the adoption blocker surfaced.

- **outreach-008 (partner outreach)**
  - [ ] ≥ 3 candidate partners contacted (literacy/refugee-ed NGO, teacher network, OER portal).
  - [ ] A realistic adoption path documented (direct educator use and/or OER self-publish
        fallback).
  - [ ] Any partner that confirms is recorded; corresponding tasks updated toward
        `verifiedNeed:true` / `requestor` set.

**M0 Definition of Done:** schema v0 + provenance gate published; one open Latin font verified
and vendored with snapshot; formation reviewer role named; deterministic renderer + SVG→PDF
pipeline producing A4/Letter; a 5-glyph Latin pilot sourced, reviewer-signed, and CC-BY-published
(handed to an educator if one is secured, else published with the blocker surfaced); ≥ 1 outreach
thread opened.

---

## Milestone M1 — Complete Latin print + accessibility

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| handwriting-practice-latin-009 | Full Latin print model (A–Z, a–z, 0–9) + SOURCES.md | data | large | low | document | pilot-007 | Pedagogical, Provenance |
| handwriting-practice-templates-010 | Sheet templates: trace / fade / copy / free-practice | code | medium | low | pr | renderer-005 | Technical |
| handwriting-practice-a11y-011 | Accessibility variants (left-handed/low-vision/ink-saving) | code | medium | low | pr | templates-010 | Accessibility, Technical |
| handwriting-practice-footer-012 | Per-sheet attribution + license footer + manifest | code | small | low | pr | pdf-006 | Provenance |
| handwriting-practice-fieldtest-013 | Educator field-test of printed Latin set + feedback | research | small | low | document | latin-009, a11y-011 | Steward |

**Acceptance criteria — key tasks**

- **latin-009 (full Latin print model)**
  - [ ] A–Z, a–z, 0–9 modeled and schema-valid; each glyph has a cited `sourceRef`.
  - [ ] `SOURCES.md` records every formation source (name, URL, license, retrieval date,
        snapshot) and the font provenance; provenance-gate green.
  - [ ] National-style variants (e.g. single/double-storey `a`/`g`, open/closed `4`) are
        labeled with source/region rather than presented as the one correct form.
  - [ ] Formation reviewer signs the per-glyph checklist for the full set.

- **a11y-011 (accessibility variants)**
  - [ ] Renders left-handed (guides/arrows/start-dots adjusted appropriately), low-vision
        (contrast + spacing), and ink-saving variants; variants compose with templates.
  - [ ] Output verified legible **on real printed paper** (grayscale-safe), not just on screen.
  - [ ] Accessibility reviewer signs off; determinism preserved (golden-file tests).

- **fieldtest-013 (educator field test)**
  - [ ] ≥ 1 educator prints and uses the Latin set and returns structured feedback.
  - [ ] Feedback logged; defects become tasks; adoption (if confirmed) flips those tasks'
        `verifiedNeed` to `true` with `requestor` recorded.

**M1 Definition of Done:** complete Latin print set modeled, sourced, and reviewer-signed; four
sheet templates and ≥ 4 accessibility variants rendering in A4/Letter; provenance-gate green
across the set with attribution/license on every sheet; ≥ 1 educator field-test completed with
feedback logged.

---

## Milestone M2 — Second & third scripts (medium-risk path proven)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| handwriting-practice-scriptrev-014 | Recruit + credit script-literate reviewer(s) | research | small | low | document | outreach-008 | Maintainer |
| handwriting-practice-script2-015 | Second script model (e.g. Cyrillic) + SOURCES.md | data | large | medium | document | latin-009, scriptrev-014, gate-002 | Script, Provenance |
| handwriting-practice-script3-016 | Third script model (e.g. Greek) + SOURCES.md | data | large | medium | document | latin-009, scriptrev-014, gate-002 | Script, Provenance |
| handwriting-practice-rtl-017 | RTL + positional-form rendering support (Arabic/Hebrew-ready) | code | medium | medium | pr | renderer-005, schema-001 | Technical, Script |

**Acceptance criteria — key tasks**

- **script2-015 (second script)** *(pattern also applies to script3-016)*
  - [ ] All letters + digits of the script modeled, schema-valid, with per-glyph `sourceRef`.
  - [ ] Gate-verified open font for the script (gate-002 artifact); `SOURCES.md` complete with
        snapshots; provenance-gate green.
  - [ ] **Script-literate reviewer** signs the per-glyph checklist (stroke order, direction,
        contextual forms, diacritics). Risk tier `medium`.
  - [ ] Sheets render in A4/Letter with the script-appropriate guideline system; CC-BY (or
        CC-BY-SA if a share-alike source is used) with attribution.

- **rtl-017 (RTL + positional forms)**
  - [ ] Renderer correctly lays out RTL scripts and selects positional forms
        (isolated/initial/medial/final) per the model.
  - [ ] Golden-file tests cover an RTL sample and a joining sample; determinism preserved.
  - [ ] Script reviewer confirms direction and joining behavior on a sample sheet.

**M2 Definition of Done:** two non-Latin scripts shipped, each script-literate-reviewer-signed
(medium risk) with gate-green provenance; RTL/positional-form rendering demonstrated and tested
where the script requires it; per-script reviewer recruited and credited.

---

## Milestone M3 — Distribution, preview & adoption

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| handwriting-practice-preview-018 | Static client-side web preview (no backend/storage) | code | medium | low | pr | templates-010, a11y-011 | Technical, Accessibility |
| handwriting-practice-release-019 | Per-script release bundle (PDF pack + model + licenses) | code | small | low | pr | latin-009, script2-015 | Provenance |
| handwriting-practice-adoption-020 | Confirm first partner adoption + record outcome | research | small | low | document | fieldtest-013, outreach-008 | Steward |

**Acceptance criteria — key tasks**

- **preview-018 (web preview)**
  - [ ] Static, client-side selection of script/template/variant → print/download; **no
        backend, no storage, no analytics that profile individuals.**
  - [ ] Any optional name-personalization is in-memory only, never persisted/logged/transmitted.
  - [ ] Accessible UI (keyboard, contrast); deterministic output matches CLI golden files.

- **adoption-020 (first confirmed adoption)**
  - [ ] A named educator/org confirms use of sheets in real teaching with evidence
        (attestation/photo-of-use/written confirmation).
  - [ ] Outcome ledger records the event; affected tasks flip `verifiedNeed:true`, `requestor`
        set.

**M3 Definition of Done:** static web preview shipped (no backend/PII); per-script release
bundles published with licenses/attribution; ≥ 1 confirmed partner adoption recorded in the
outcome ledger with `verifiedNeed:true` on the relevant tasks.

---

## Milestone M4 — Scale & sustainability

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| handwriting-practice-playbook-021 | "Add a script" playbook + reviewer-recruitment guide | writing | small | low | document | script2-015, script3-016 | Maintainer |
| handwriting-practice-refresh-022 | Font/source staleness + refresh/re-review process | maintenance | small | low | document | gate-002, latin-009 | Provenance |
| handwriting-practice-scale-023 | Onboard 2 further scripts/styles via the playbook | data | large | medium | document | playbook-021, rtl-017 | Script, Provenance |

**Acceptance criteria — key tasks**

- **playbook-021 (add-a-script playbook)**
  - [ ] Step-by-step: source formation reference → verify font at gate → model glyphs → recruit
        script reviewer → render variants → publish with licenses.
  - [ ] Includes the per-glyph review checklist template and reviewer-recruitment guidance.

- **refresh-022 (staleness/refresh)**
  - [ ] Process detects upstream font updates and source/license changes; affected scripts
        become `maintenance` tasks and are re-reviewed.
  - [ ] Re-run provenance-gate on refresh; record outcomes.

**M4 Definition of Done:** add-a-script playbook + reviewer-recruitment guide published;
staleness/refresh process documented and owned; ≥ 2 further scripts/styles onboarded via the
playbook; ≥ 3 verifiable external reuse events recorded.

---

## Backlog / future

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| handwriting-practice-cursive-024 | Latin continuous-cursive model (if open source + reviewer) | data | large | low | document | Blocked on open formation source + reviewer |
| handwriting-practice-devanagari-025 | Devanagari model (headline/conjuncts/matras) | data | large | medium | document | Needs script-literate reviewer |
| handwriting-practice-hangul-026 | Hangul syllabic-block model | data | large | medium | document | Block composition + grid guides |
| handwriting-practice-han-027 | Han/CJK via KanjiVG (CC-BY-SA) | data | large | medium | document | Share-alike → CC-BY-SA output; policy decision |
| handwriting-practice-i18n-028 | Translate sheet labels/instructions per locale | writing | small | medium | translation | Needs language reviewer; type=writing per Hee-Lee Oss schema (translation is a deliverable, not a type) |
| handwriting-practice-dash-029 | Outcome dashboard (adoptions + reuse events) | code | medium | low | pr | Reads the outcome ledger |

---

## Example task JSON

```json
{
  "id": "handwriting-practice-schema-001",
  "title": "Formation-model JSON Schema + canonical data model",
  "project": "handwriting-practice",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["education", "literacy", "accessibility", "open-content"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "handwriting-practice generates printable letter/number formation sheets from a data-first model of how each glyph is written (strokes, order, direction, guideline geometry) rather than from proprietary worksheet fonts. Before modeling any glyph, the project needs one versioned, openly-licensed schema that all scripts, templates, and provenance records conform to, so the renderer is deterministic and the license/provenance gate can be enforced structurally. The model is the durable asset; PDFs are a projection of it.",
  "objective": "Define and publish the versioned JSON Schema and canonical data model (ScriptModel, GlyphModel, Stroke, GuidelineSystem, SheetTemplate, StyleVariant, AssetProvenance) that every script model and generated sheet conforms to.",
  "acceptanceCriteria": [
    "JSON Schema defines ScriptModel, GlyphModel, Stroke, GuidelineSystem, SheetTemplate, StyleVariant, and AssetProvenance with all fields described in PLAN section 6.",
    "GlyphModel includes ordered strokes[] (path, order, startPoint, directionHint), guidelineAlignment, optional positionalForms (e.g. Arabic isolated/initial/medial/final), and a required sourceRef citing an authoritative formation source.",
    "ScriptModel includes script (ISO 15924), direction (ltr|rtl), font (AssetProvenance), reviewers[], and schemaVersion.",
    "AssetProvenance captures license {id,url,permitsRedistribution,permitsDerivatives,reservedFontName?,snapshotRef}, attribution, and retrievedAt so the provenance gate can enforce open licensing by default-deny.",
    "Schema is versioned; the spec document is licensed CC-BY-4.0; at least one worked example model validates against the schema.",
    "pnpm build && pnpm test && pnpm lint pass for any committed tooling; commit is DCO signed-off."
  ],
  "resources": [
    "C:\\code\\hee-lee-oss\\planning\\projects\\handwriting-practice\\PLAN.md",
    "C:\\code\\hee-lee-oss\\packages\\schema\\src\\schemas.ts",
    "SIL Open Font License (OFL) 1.1",
    "ISO 15924 script codes",
    "Datasheets/provenance pattern: planning/projects/open-data-datasheets"
  ],
  "output": "A versioned formation-model JSON Schema plus a canonical data-model document, committed to the project repo and ready for use by all per-script modeling, rendering, and provenance-gate tasks.",
  "requestor": "TO BE SECURED",
  "verifiedNeed": false,
  "outputLicense": "CC-BY-4.0"
}
```

---

## Generated task index

> Auto-generated by Hee-Lee Oss task-decomposition (2026-06-29). Every TASKS.md row now has a
> corresponding `tasks/<id>.json` validated against the Hee-Lee Oss taskSchema (draft-07).
> Total: 29 task files (1 pre-existing seed + 28 generated).

| File | Title | Milestone | Type | Deliverable | Risk |
| --- | --- | --- | --- | --- | --- |
| tasks/handwriting-practice-schema-001.json | Formation-model JSON Schema + canonical data model | M0 | design-spec | document | low |
| tasks/handwriting-practice-gate-002.json | Font + source license/provenance gate (blocking) | M0 | design-spec | document | medium |
| tasks/handwriting-practice-font-003.json | Verify + vendor one open Latin font through the gate | M0 | research | document | medium |
| tasks/handwriting-practice-reviewer-004.json | Name/secure formation reviewer role (blocking gate role) | M0 | research | document | low |
| tasks/handwriting-practice-renderer-005.json | Deterministic SVG renderer (model + template → SVG) | M0 | code | pr | low |
| tasks/handwriting-practice-pdf-006.json | SVG→PDF pipeline + A4/Letter paper sizing | M0 | code | pr | low |
| tasks/handwriting-practice-pilot-007.json | 5-glyph Latin pilot sheet, sourced + reviewed, end-to-end | M0 | data | document | low |
| tasks/handwriting-practice-outreach-008.json | Partner/educator outreach + adoption-path shortlist | M0 | research | document | low |
| tasks/handwriting-practice-latin-009.json | Full Latin print model (A–Z, a–z, 0–9) + SOURCES.md | M1 | data | document | low |
| tasks/handwriting-practice-templates-010.json | Sheet templates: trace / fade / copy / free-practice | M1 | code | pr | low |
| tasks/handwriting-practice-a11y-011.json | Accessibility variants (left-handed/low-vision/ink-saving) | M1 | code | pr | low |
| tasks/handwriting-practice-footer-012.json | Per-sheet attribution + license footer + manifest | M1 | code | pr | low |
| tasks/handwriting-practice-fieldtest-013.json | Educator field-test of printed Latin set + feedback | M1 | research | document | low |
| tasks/handwriting-practice-scriptrev-014.json | Recruit + credit script-literate reviewer(s) | M2 | research | document | low |
| tasks/handwriting-practice-script2-015.json | Second script model (Cyrillic) + SOURCES.md | M2 | data | document | medium |
| tasks/handwriting-practice-script3-016.json | Third script model (Greek) + SOURCES.md | M2 | data | document | medium |
| tasks/handwriting-practice-rtl-017.json | RTL + positional-form rendering support (Arabic/Hebrew-ready) | M2 | code | pr | medium |
| tasks/handwriting-practice-preview-018.json | Static client-side web preview (no backend/storage) | M3 | code | pr | low |
| tasks/handwriting-practice-release-019.json | Per-script release bundle (PDF pack + model + licenses) | M3 | code | pr | low |
| tasks/handwriting-practice-adoption-020.json | Confirm first partner adoption + record outcome | M3 | research | document | low |
| tasks/handwriting-practice-playbook-021.json | "Add a script" playbook + reviewer-recruitment guide | M4 | writing | document | low |
| tasks/handwriting-practice-refresh-022.json | Font/source staleness + refresh/re-review process | M4 | maintenance | document | low |
| tasks/handwriting-practice-scale-023.json | Onboard 2 further scripts/styles via the playbook | M4 | data | document | medium |
| tasks/handwriting-practice-cursive-024.json | Latin continuous-cursive model (if open source + reviewer) | Backlog | data | document | low |
| tasks/handwriting-practice-devanagari-025.json | Devanagari model (headline/conjuncts/matras) | Backlog | data | document | medium |
| tasks/handwriting-practice-hangul-026.json | Hangul syllabic-block model | Backlog | data | document | medium |
| tasks/handwriting-practice-han-027.json | Han/CJK via KanjiVG (CC-BY-SA) | Backlog | data | document | medium |
| tasks/handwriting-practice-i18n-028.json | Translate sheet labels/instructions per locale | Backlog | writing | translation | medium |
| tasks/handwriting-practice-dash-029.json | Outcome dashboard (adoptions + reuse events) | Backlog | code | pr | low |

**Schema notes:**
- All tasks: `lane: donated`, `status: open`, `urgent: false`, `verifiedNeed: false`, `requestor: TO BE SECURED` (per PLAN.md §2 — no partner confirmed yet).
- `i18n-028` type corrected from `translation` (not a valid Hee-Lee Oss type) to `writing` with `deliverable: translation`.
- Fan-out: no fan-out applied. script2-015 and script3-016 are separate rows already enumerated in TASKS.md (Cyrillic and Greek). scale-023 covers two further scripts as a single representative task (specific scripts TBD by reviewer availability — not enumerated in the plan).

