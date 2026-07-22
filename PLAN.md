# PLAN — handwriting-practice

> Status: Draft · Version: 0.1.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) ·
> Lane: donated · Risk tier: low (Latin) → medium (non-Latin scripts, see §8)

**Open, printable letter- and number-formation practice sheets — one rigorously-sourced
formation model per script, rendered into accessible PDFs anyone can print for free.**

handwriting-practice is not a font and not a worksheet PDF dump. It is a **data-first
generator**: a small, openly-licensed model of *how each glyph is formed* (strokes, order,
direction, guideline geometry) plus a deterministic renderer that turns that model into
trace-and-copy sheets in many paper sizes, styles, and accessibility variants. The data is the
asset; the PDFs are a projection of it. Get the formation model right, sourced, and reviewed —
and every sheet, in every script, follows for free.

---

## 1. Executive summary

Millions of early-literacy learners, adults acquiring a new script, and learners with
handwriting difficulty lack free, correct, printable practice material. What exists online is
mostly (a) behind paywalls, (b) of unknown pedagogical accuracy, (c) built on **proprietary
school fonts** whose licenses forbid redistribution, or (d) silently wrong for non-Latin
scripts (wrong stroke order, wrong contextual forms, wrong directionality).

This project produces a **commons of formation models** — structured, cited, reviewed JSON
describing how to write each letter and digit in a given script — and an **MIT-licensed
generator** that renders them into CC-BY printable sheets. The headline gate is
**license/provenance**: every glyph outline must come from an OFL/PD/CC font, every stroke-order
claim must cite an authoritative source, and non-Latin scripts ship only after a
**script-literate reviewer** signs off (medium risk). We make no clinical or "best method"
claims: this is educational practice material, offered in multiple recognized styles, never
presented as therapy or the single correct way to write.

The MVP wedge is a single script — **Latin (basic print, A–Z, a–z, 0–9)** — shipped fully
end-to-end through the model → renderer → accessible-PDF → reviewed → handed-to-a-real-teacher
loop. One script done *excellently* (sourced, accessible, accepted by a real educator) beats ten
scripts dumped as unsourced PDFs.

---

## 2. Problem & beneficiaries

### The problem
- **Cost & access.** Quality formation worksheets are overwhelmingly commercial. Families,
  under-resourced schools, refugee/migrant education programs, and adult-literacy tutors who
  most need them are the least able to pay.
- **Correctness is unverifiable.** Free PDFs rarely cite a source for stroke order or formation.
  Wrong formation taught early is hard to unlearn.
- **License landmines.** The "nice" handwriting/school fonts (dotted-trace, cursive-join fonts)
  are almost all proprietary; redistributing sheets built from them is infringement. Most free
  generators quietly ignore this.
- **Non-Latin scripts are underserved and often wrong.** Arabic (RTL, contextual joining forms),
  Hebrew (RTL), Devanagari and other Indic scripts (conjuncts, matras, headline/shirorekha),
  Hangul (syllabic blocks), Greek, Cyrillic — generic tools mangle stroke order, direction, and
  letter forms.
- **Accessibility is an afterthought.** Left-handed learners, low-vision learners, and learners
  with dysgraphia need variants (line spacing, contrast, start-dots, ink-saving) that generic
  worksheets don't offer.

### Who is helped (beneficiaries)
- **Early-literacy children** (pre-K–grade 2) and their teachers/parents.
- **Adult learners** acquiring literacy or a second script (e.g., a Latin-script speaker
  learning Arabic or Cyrillic, or vice-versa).
- **Refugee, migrant, and mother-tongue education programs** teaching under-resourced or
  minority scripts.
- **Homeschoolers and micro-schools** with no worksheet budget.
- **Learners with handwriting difficulty** (e.g., dysgraphia) and the educators/parents
  supporting them — supported with *practice material*, explicitly **not** therapy.
- **Translators/localizers and OER authors** who can reuse the formation data and sheets.

### Verified need — **TO BE SECURED**
The *general* need is well-documented (cost, correctness, license, and accessibility gaps above
are real). The *per-deliverable* need — a named educator, school, or literacy/refugee-education
org that will accept and use specific sheets — is **not yet secured**. All tasks therefore carry
`verifiedNeed: false` and `requestor: TO BE SECURED` until a partner confirms (see §11).

### Partner / requestor — **TO BE SECURED**
Target partners: an adult-literacy or refugee-education NGO, a public-school early-literacy
teacher network, a homeschool co-op, an OER repository (e.g., an open-courseware or teacher
resource portal), and — for non-Latin scripts — language-community organizations and SIL-style
literacy programs who can supply script-literate reviewers.

---

## 3. Goals and non-goals

### Goals
- A **versioned, openly-licensed formation-model schema** that captures, per glyph: strokes
  (ordered paths), stroke direction, start points, guideline alignment, contextual/positional
  forms, and a citation to an authoritative formation source.
- A **deterministic MIT-licensed generator** (model + template → SVG → PDF) producing
  print-faithful, accessible sheets in standard paper sizes (A4 and US Letter at minimum).
- A **fully-sourced Latin print model** (A–Z, a–z, 0–9) reviewed and accepted by a real
  educator — the proof the loop works.
- **License/provenance rigor**: every font and every stroke-order source recorded, verified
  open, and attributed; an automated check that fails the build on an unverified asset.
- **Accessibility as a first-class output**, not a toggle bolted on later (see §6).
- A **repeatable per-script playbook** so adding Cyrillic, Greek, Arabic, Hebrew, Devanagari,
  Hangul, etc. is a known process gated by script-literate review.

### Non-goals
- **Not a font foundry.** We do not design or ship new typefaces; we *consume* OFL/PD/CC fonts.
  If a script needs a font we may not lawfully use, that script is blocked until an open font
  exists — we will not redraw proprietary forms.
- **Not handwriting *recognition* or capture.** No scanning, no ML scoring of a learner's
  writing, no camera/upload. (That would change the privacy surface entirely.)
- **Not a curriculum or a methodology endorsement.** We do not declare cursive-vs-print or any
  national style "correct." We offer recognized styles side by side and cite each.
- **Not therapy or clinical material.** No claims of treating dysgraphia or any condition;
  no diagnostic or remediation claims.
- **Not personalized-data products.** No accounts, no learner profiles, no storing names typed
  into a sheet (see §6, §14).
- **Not print fulfillment.** We produce printable files; we do not sell, mail, or print.

---

## 4. Success metrics (outcomes)

Outcome-based (beneficiary-centric), not vanity downloads.

| Metric | Baseline | Target (12 mo) | How measured |
|---|---|---|---|
| Scripts shipped with a **sourced + reviewed** model | 0 | ≥ 3 (Latin + 2 others) | Repo + review sign-off artifacts |
| Sheets **accepted/adopted by a named educator or org** | 0 | ≥ 1 confirmed partner using sheets in real teaching | Steward-recorded adoption evidence |
| Distinct learners/classrooms reached (reported) | 0 | ≥ 200 learners (partner-reported, opt-in) | Partner attestation, not analytics |
| Formation accuracy (reviewer-confirmed glyphs) | 0% | 100% of shipped glyphs reviewer-signed | Per-script review checklist |
| License-clean assets (fonts + sources) | n/a | 100% (build fails otherwise) | Automated provenance gate |
| Accessibility variants per shipped script | 0 | ≥ 4 (print/left-handed/low-vision/ink-saving) | Generator output matrix |
| External reuse events (OER reuse, fork, translation) | 0 | ≥ 3 verifiable | Outcome ledger |

Anti-metric: we explicitly do **not** optimize for raw download count or sheet quantity; an
unsourced or unreviewed sheet counts as zero.

---

## 5. Scope

### In scope
- Letters (upper/lower where applicable) and digits 0–9 for a target script.
- Basic punctuation that shares the writing system (optional, per script).
- Formation model: strokes, order, direction arrows, start dots, guideline geometry,
  positional/contextual forms (e.g., Arabic isolated/initial/medial/final).
- Sheet templates: trace rows, fade-to-blank rows, copy rows, free-practice rows; configurable
  guideline systems (e.g., 4-line/3-zone for Latin, single baseline, grid for CJK).
- Output: SVG (source) → PDF (print). A4 + US Letter; portrait; grayscale-safe.
- Accessibility variants (see §6).
- Style variants where recognized and openly sourceable (print; precursive/continuous-cursive
  for Latin if an open model + reviewer exist).
- A small static **web preview** (optional, M3+) for selecting/printing — client-side only.

### Out of scope
- New typeface design; any proprietary font or proprietary worksheet form.
- Handwriting recognition, scoring, capture, scanning, or ML on learner writing.
- Accounts, learner tracking, analytics that profile individuals.
- Clinical/therapeutic content or claims (dysgraphia treatment, OT protocols).
- Endorsing one national handwriting style or the cursive-vs-print debate.
- Languages/scripts for which no open font and/or no script-literate reviewer is available
  (blocked, not faked).
- Print/mail fulfillment and any paid service.

---

## 6. Solution approach & architecture

Data-first. The **formation model is the durable asset**; the renderer and PDFs are projections.

### Components
1. **`packages/schema` (formation model spec)** — JSON Schema for `ScriptModel`, `GlyphModel`,
   `Stroke`, `GuidelineSystem`, `SheetTemplate`, `StyleVariant`, and `AssetProvenance`
   (font + source license records). Versioned; CC-BY for the spec docs.
2. **`packages/core` (renderer)** — pure, deterministic: `(GlyphModel, SheetTemplate,
   StyleVariant, PaperSize) → SVG`. No vendor logic, no network. SVG → PDF via an
   openly-licensed conversion library (decision in §"Key decisions").
3. **`packages/cli` (generator)** — `handwriting render <script> --template=trace
   --paper=A4 --variant=left-handed`. Batch-builds a script's full sheet set. Emits a manifest
   + per-file provenance footer.
4. **`provenance-gate` (build check)** — fails CI if any referenced font or stroke-order source
   lacks a verified open-license record (OFL/PD/CC) and required attribution.
5. **`data/<script>/`** — the formation models themselves (one folder per script), each with a
   `SOURCES.md` citing authoritative formation references and the chosen open font.
6. **Web preview (M3+, optional)** — static, client-side SVG/PDF preview; no backend, no storage.

### Data model (core entities)
- **`ScriptModel`**: `script` (ISO 15924, e.g. `Latn`, `Cyrl`, `Arab`), `language?`, `direction`
  (`ltr`|`rtl`), `guidelineSystem`, `glyphs[]`, `font` (AssetProvenance), `styleVariants[]`,
  `reviewers[]`, `schemaVersion`.
- **`GlyphModel`**: `codepoint`, `name`, `strokes[]`, `guidelineAlignment` (ascender/x-height/
  baseline/descender or script-specific zones), `positionalForms?` (Arabic
  isolated/initial/medial/final, etc.), `notes`, `sourceRef`.
- **`Stroke`**: `order`, `path` (SVG path data), `startPoint`, `directionHint` (arrow), `pause?`.
- **`GuidelineSystem`**: zone definitions + line styles (e.g., Latin 4-line; single baseline;
  square grid; Devanagari with headline).
- **`SheetTemplate`**: row recipe (trace / fade / copy / blank), repetitions, paper size,
  margins, guideline style, header (title only — no learner PII fields stored).
- **`StyleVariant`**: `print` | `precursive` | `cursive` | `left-handed` | `low-vision` |
  `ink-saving` (variants compose: e.g., left-handed + low-vision).
- **`AssetProvenance`**: `name`, `kind` (`font`|`source`), `license` (`{id,url,permits
  Redistribution,permitsDerivatives,reservedFontName?,snapshotRef}`), `attribution`,
  `retrievedAt`.

### Interfaces
- CLI is the primary interface; the renderer is importable as a library; models are plain JSON
  validated against the published schema. A stable **model→sheet contract** keeps adding scripts
  cheap (mirrors Ofelia's "faculty contract" idea: every script implements the same model
  interface).

### Key decisions (locked)
- **Stack:** TypeScript + ESM + pnpm workspaces (per CLAUDE.md). MIT for code; CC-BY-4.0 for
  formation models, sheet templates, and generated PDFs.
- **Glyph outlines come only from OFL/PD/CC fonts** (e.g., SIL **Andika** — OFL, literacy-
  designed — as a primary Latin candidate, pending gate verification). We never trace
  proprietary forms.
- **We author our own stroke/order overlay** (arrows, start-dots, sequence) as original CC-BY
  work, *citing* an authoritative formation source — we do not copy a proprietary worksheet.
- **Determinism:** identical inputs → byte-stable SVG (golden-file tested), so review is
  meaningful and diffs are reviewable.
- **No network at render time**; all assets vendored with provenance.
- **SVG is the source of truth; PDF is generated** (so we can re-render at any paper size).

---

## 7. Data, licensing & compliance  *(CRITICAL)*

This is the project's central gate. Two asset classes each have a hard rule.

### A. Fonts (glyph outlines)
- **Allowed:** SIL Open Font License (OFL), Public Domain / CC0, or CC fonts whose terms
  **permit redistribution and embedding/derivatives**. GPL-with-Font-Exception acceptable if
  redistribution of generated PDFs is clearly permitted.
- **OFL specifics enforced:** (1) Reserved Font Names — if a model "modifies" a font we must not
  ship under the reserved name; our overlay is a separate layer, but any actual outline
  modification triggers renaming. (2) OFL fonts may not be sold on their own; our PDFs bundle
  outlines as document content, which OFL permits, with the OFL text included in the repo and
  attribution in each sheet footer.
- **Excluded:** any proprietary "school"/"handwriting" font, any font with "no embedding,"
  "no redistribution," or non-commercial-only terms that would block free reuse.

### B. Stroke-order / formation sources
- Formation order/direction must **cite an authoritative source** (national curriculum
  handwriting guidance, an established literacy program's published formation guide, a
  standards body, or — for CJK/Han — **KanjiVG**, CC-BY-SA, with share-alike honored).
- **We do not copy proprietary worksheet artwork.** We record the *facts of formation* (which
  facts are not copyrightable) and express them in our own original SVG overlay.
- Each script's `SOURCES.md` records: source name, URL, license, retrieval date, and a license
  **snapshot** (committed copy + SHA-256 + Wayback URL), mirroring Hee-Lee Oss's open-data provenance
  practice.

### C. Share-alike handling
- If a source is **CC-BY-SA** (e.g., KanjiVG) or otherwise share-alike, any derived model layer
  built *from* it inherits share-alike; that model ships under the compatible SA license and is
  recorded as such, kept separable from CC-BY-only models. A `policy` doc decides SA/NC handling
  before any SA source is used (NC sources are excluded outright).

### D. Provenance model
- Every shipped artifact carries machine-readable provenance: font record + source record(s) +
  reviewer(s) + schema version. The **provenance-gate build check** fails if any referenced
  asset lacks a complete, verified record. Default is **deny** (no record ⇒ build fails), never
  default-allow.

### E. Privacy / PII
- The project handles **no personal data**. Sheets may include an optional printed name *field*
  (an empty line for a learner to write on paper) but the tool **never stores, transmits, or
  requires** a name. Any future "type your name to personalize" feature must be **client-side
  only, in-memory, never persisted or logged** (see §14). No accounts, no analytics that profile
  individuals.

### F. Cultural & correctness compliance
- Non-Latin scripts: directionality, joining behavior, diacritics/matras, and stroke order are
  **verified by a script-literate reviewer** before ship (medium risk). We do not guess.
- Non-prescriptive framing: each style is labeled with its source/region; no "this is the right
  way to write" language. Civic/curricular neutrality where a national style is referenced.

### G. Output licensing
- Code: **MIT.** Formation models, templates, and generated PDFs: **CC-BY-4.0** (or CC-BY-SA
  where a share-alike source compels it). OFL font license text shipped alongside; attribution
  in every sheet footer.

---

## 8. Quality, review & risk gates

### Risk tier
- **Latin (and other scripts with a confident maintainer-level formation source): low** —
  standard technical + maintainer review.
- **Non-Latin scripts (Cyrillic, Greek, Arabic, Hebrew, Devanagari, Hangul, etc.): medium** —
  requires a **script-literate reviewer** (native/fluent writer or qualified teacher of that
  script) to sign off on stroke order, direction, contextual forms, and diacritics before merge.
- **No high-tier content.** We deliberately exclude clinical/therapeutic claims that would push
  into high risk. Any sheet aimed at handwriting *difficulty* is framed as practice material,
  "not therapy," and gets an extra plain-language disclaimer reviewed by the maintainer.

### Required review before "done"
1. **Technical review** — schema-valid model; deterministic render; CI green
   (`pnpm build && pnpm test && pnpm lint`); golden-file SVG match; DCO sign-off.
2. **Provenance review** — provenance-gate passes; font + source licenses verified open;
   attribution present; snapshots recorded.
3. **Pedagogical/script review** — formation matches the cited source; for non-Latin, the
   **script-literate reviewer** signs the per-glyph checklist.
4. **Accessibility review** — required variants render correctly and legibly (contrast,
   spacing, left-handed mirroring of guides/arrows where appropriate).
5. **Steward last-mile** — at least one real educator/org has the sheets and (for the headline
   metric) confirms use.

### Definition of Shipped
A script (or sheet set) is **delivered, not merged** when: model is schema-valid and sourced;
provenance-gate green; formation reviewer-signed (script-literate for non-Latin); accessibility
variants produced; CC-BY PDFs + CC-BY model committed with attribution and license texts; and
the output is **handed to a named beneficiary** (educator/org) — or, if no partner yet,
published to the open repo *and* explicitly marked `verifiedNeed: false` with the adoption
blocker surfaced.

---

## 9. Roadmap & milestones

Phased; each milestone has measurable exit criteria. Constraints-as-identity: **no script ships
unsourced; no non-Latin script ships unreviewed; no asset ships unlicensed.**

### M0 — Foundation & cold-start (thin slice)
**Goal:** Prove the model → renderer → accessible-PDF → review loop on the smallest real unit.
**Exit criteria:**
- Formation-model JSON Schema v0 published (ScriptModel/GlyphModel/Stroke/GuidelineSystem/
  SheetTemplate/StyleVariant/AssetProvenance).
- Provenance-gate check exists and fails the build on a missing/unverified asset.
- One **open Latin font verified through the license gate** (record + snapshot committed).
- Renderer produces a deterministic A4 + Letter **trace sheet for a 5-glyph pilot**
  (e.g., `a c o l 1`) with a 4-line guideline system.
- Script-literate/maintainer formation reviewer **role named**; pilot glyphs reviewer-signed.
- Partner-outreach thread opened (≥ 1).

### M1 — Complete Latin print + accessibility
**Goal:** A full, sourced, accessible Latin print set.
**Exit criteria:**
- A–Z, a–z, 0–9 modeled, sourced (`SOURCES.md`), and reviewer-signed.
- Templates: trace, fade-to-blank, copy, free-practice — all in A4 + Letter.
- ≥ 4 accessibility variants render correctly (print, left-handed, low-vision, ink-saving).
- Provenance-gate green across the whole set; every sheet footer carries attribution + license.
- ≥ 1 educator has reviewed real printed output and given feedback.

### M2 — Second & third scripts (medium-risk path proven)
**Goal:** Prove the per-script playbook on non-Latin scripts with script-literate review.
**Exit criteria:**
- Two additional scripts shipped (candidate order: **Cyrillic, Greek** as lower-complexity
  non-Latin; or **Arabic/Hebrew** if RTL+joining reviewer secured) — each reviewer-signed.
- RTL and/or positional-form handling demonstrated and tested where the script requires it.
- Per-script `SOURCES.md` + provenance-gate green for each.
- Per-script reviewer recruited and credited.

### M3 — Distribution, preview & adoption
**Goal:** Make sheets easy to find, print, and adopt; close the outcomes loop.
**Exit criteria:**
- Static client-side web preview (select script/template/variant → print), no backend/storage.
- A discoverable release bundle per script (PDF pack + model + license/attribution).
- ≥ 1 **confirmed partner** using sheets in real teaching (`verifiedNeed: true` for those tasks).
- Outcome ledger records ≥ 1 verifiable adoption/reuse event.

### M4 — Scale & sustainability
**Goal:** Repeatable script onboarding + maintenance.
**Exit criteria:**
- Documented "add a script" playbook + contribution guide + reviewer-recruitment process.
- ≥ 2 further scripts or styles (e.g., Latin continuous-cursive, Devanagari, Hangul) onboarded
  via the playbook.
- Staleness/refresh process for fonts/sources; ≥ 3 verifiable external reuse events.

---

## 10. Work breakdown

The itemized, schema-mapped backlog lives in **TASKS.md** — milestone tables (M0–M4), per-task
acceptance criteria, Definitions of Done, a backlog, and a complete schema-valid example Task
JSON. Each task maps to a Hee-Lee Oss Task JSON (`packages/schema/src/schemas.ts`). All tasks start
`verifiedNeed: false` / `requestor: TO BE SECURED` until a partner is confirmed.

---

## 11. Governance, roles & stakeholders

- **Maintainer (Owner: TBD)** — accountable for the project, schema, and release quality.
- **Technical reviewer** — schema validity, determinism, CI, license headers.
- **Provenance/License reviewer** — runs the font + source license gate; veto power; blocking,
  non-skippable role.
- **Script-literate reviewer(s)** — per non-Latin script; native/fluent writer or qualified
  teacher; signs the per-glyph formation checklist. **TO BE SECURED per script.**
- **Pedagogical reviewer** — checks formation matches cited sources and framing is
  non-prescriptive.
- **Accessibility reviewer** — validates variants (low-vision, left-handed) with the relevant
  lived-experience/expertise where possible.
- **Steward (last-mile owner)** — owns getting sheets into a real educator's/org's hands and
  recording the outcome. **TO BE SECURED.**
- **Partner / requestor** — the educator/NGO/school requesting and adopting sheets.
  **TO BE SECURED.**

Conflict-of-interest and veto checklist per Hee-Lee Oss governance; license/provenance and
script-review roles cannot be waived.

---

## 12. Dependencies & integrations

- **Open fonts** (OFL/PD/CC) — e.g., SIL Andika and other literacy/script fonts; subject to
  the gate. External upstreams; vendored with license + snapshot.
- **Stroke-order sources** — national/curricular handwriting guidance; **KanjiVG** (CC-BY-SA)
  for any Han content; established literacy-program formation guides.
- **SVG→PDF library** — an openly-licensed converter (decision recorded; pinned version).
- **Hee-Lee Oss pieces** — task schema (`packages/schema`), CLI workspace conventions, the donated-
  lane PR workflow, the outcome-ledger pattern, and the open-data provenance/snapshot practice
  reused here for fonts/sources.
- **Distribution** — repo releases; optional static host for the web preview (no backend).

---

## 13. Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Proprietary font slips into a sheet (license breach) | Medium | High | Default-deny provenance-gate fails build on any unverified asset; OFL/PD/CC allowlist; license snapshots committed | Provenance reviewer |
| Wrong stroke order/forms (esp. non-Latin) taught to learners | Medium | High | Mandatory script-literate reviewer sign-off (medium risk); cite authoritative source per glyph; per-glyph checklist | Script reviewer |
| No partner/educator adopts (deliverable unused) | High | High | Early outreach (M0); steward role; mark `verifiedNeed:false`; self-serve OER publication as fallback path to use | Steward / Maintainer |
| OFL Reserved Font Name violation on modification | Low | Medium | Keep overlay separate from outlines; if outlines are modified, rename per OFL; legal-text checklist | Provenance reviewer |
| Share-alike (KanjiVG/CC-BY-SA) license contamination of CC-BY models | Medium | Medium | SA/NC policy decided up front; SA-derived models kept separable and SA-licensed; NC excluded | Maintainer |
| "Best method"/cursive-vs-print bias or national-style partisanship | Medium | Medium | Non-prescriptive framing; label each style with source/region; offer multiple styles | Pedagogical reviewer |
| Mistaken as dysgraphia therapy / clinical advice | Medium | High | Explicit "practice material, not therapy/clinical advice" framing; no efficacy claims; maintainer-reviewed disclaimers | Maintainer |
| Accessibility variant looks fine on screen but fails in print (contrast/spacing) | Medium | Medium | Print-target review on real paper; grayscale-safe defaults; accessibility reviewer | Accessibility reviewer |
| Non-determinism breaks reviewability/diffs | Low | Medium | Golden-file SVG tests; pinned renderer + fonts; deterministic build | Technical reviewer |
| Optional name-personalization leaks PII | Low | High | Client-side, in-memory only; never persisted/logged; documented constraint | Maintainer |
| Reviewer scarcity blocks a script | Medium | Medium | Block (don't fake); recruit via language-community partners; ship only when reviewer secured | Maintainer |

---

## 14. Security & privacy

- **Threat surface is small by design**: a CLI + static assets, no backend, no accounts, no
  user data store. Largest real risks are (a) shipping an unlicensed asset and (b) any future
  personalization feature leaking PII.
- **No PII collected or stored.** Optional name fields are blank lines printed on paper. Any
  future "type a name to personalize" feature is **client-side, in-memory, never persisted,
  never logged, never sent over the network** — and is opt-in. This constraint is part of the
  product's identity (see §3 non-goals).
- **No secrets** in repo, logs, footers, or PDFs (per CLAUDE.md). No API keys needed (donated
  lane; no funded escrow planned).
- **Supply chain:** pinned dependencies; vendored fonts with checksums; provenance-gate as a
  CI guard; web preview ships only static, audited assets (no third-party trackers).
- **Abuse/misuse:** the content is benign practice material; main misuse vector is
  redistribution under a wrong license — mitigated by clear per-file attribution + license and
  the provenance gate. Refusal guardrails (CLAUDE.md) apply if any task is steered toward harm.

---

## 15. Sustainability & maintenance

- **Maintainer + reviewer rotation** carries the project post-delivery; a steward tracks
  adoption/outcomes in the ledger (not download analytics).
- **Refresh process:** fonts and sources are versioned; a staleness check flags upstream font
  updates or source/license changes; affected scripts become `maintenance` tasks and are
  re-reviewed.
- **Low ongoing cost:** static outputs, no servers required. The web preview (if shipped) is
  static hosting.
- **Community contribution:** the "add a script" playbook + reviewer-recruitment guide lets new
  language communities contribute their own scripts under the same gates.

---

## 16. Open questions

1. **SVG→PDF toolchain** — which openly-licensed library gives print-faithful, deterministic
   output across paper sizes? (Decision needed in M0.)
2. **Primary Latin font** — confirm SIL Andika (or alternative) passes the gate and reads well
   as a *formation* model (some literacy fonts are reading-optimized, not formation-optimized).
3. **Cursive scope** — is there an *open* source + reviewer for a continuous-cursive Latin
   model, or do we ship print only until one exists?
4. **Second/third script choice** — sequence by reviewer availability (Cyrillic/Greek vs.
   Arabic/Hebrew RTL) — which partner can supply a reviewer first?
5. **Share-alike posture** — adopt CC-BY-SA models (KanjiVG-derived) in-repo, or keep the
   commons strictly CC-BY and defer Han scripts? (Policy decision.)
6. **National-style neutrality** — when multiple national print styles differ (e.g., the `a`/`g`
   single vs double-storey, `4` open vs closed), do we ship multiple labeled variants by default?
7. **Partner-first vs. publish-first** — do we hold a script until a partner is secured, or
   publish to OER and pursue adoption in parallel? (Affects `verifiedNeed` timing.)
8. **Accessibility expertise** — can we secure low-vision and left-handed reviewers with lived
   experience, not just heuristics?

---

## 17. References

- Hee-Lee Oss `CLAUDE.md` — work rules, lanes, quality bar, refusal guardrails.
- Hee-Lee Oss `docs/good-deed-definition.md` — five criteria + risk tiers.
- Hee-Lee Oss `packages/schema/src/schemas.ts` — Task JSON schema (TASKS.md maps to it).
- Hee-Lee Oss `planning/ROADMAP.md` — portfolio context (handwriting-practice, Track 3).
- Sibling pattern: `planning/projects/open-data-datasheets/` — provenance/snapshot + gate model
  reused here for fonts/sources.
- SIL Open Font License (OFL) 1.1 — font licensing terms and Reserved Font Name rules.
- SIL **Andika** — literacy-oriented OFL font (candidate Latin source, pending gate).
- **KanjiVG** (CC-BY-SA) — stroke-order data for any future Han content (share-alike).
- ISO 15924 — script codes used in `ScriptModel.script`.
- Creative Commons CC-BY-4.0 / CC-BY-SA-4.0 — output content licenses.

---

## Appendix A — Improvements applied

The following 25 specific improvements were identified against the first draft and **applied**
to the PLAN above and to TASKS.md.

1. **Risk tier split made explicit** — Latin = low, non-Latin = medium with mandatory
   script-literate sign-off (rather than a flat "low"). Applied in §8 and per-task risk in TASKS.
2. **Provenance gate is default-deny** — build fails on any asset lacking a complete verified
   record; no default-allow. Applied in §7D, §13.
3. **OFL Reserved Font Name handling spelled out** — overlay kept separate from outlines; rename
   on actual outline modification. Applied in §7A, risk table.
4. **Share-alike (KanjiVG/CC-BY-SA) contamination addressed** — SA-derived models kept
   separable and SA-licensed; NC excluded; policy decided before use. §7C, §13, open Q5.
5. **"Not therapy / not clinical advice" framing added** — dysgraphia/handwriting-difficulty
   content is practice material only, with reviewed disclaimers. §3, §8, §13.
6. **Non-prescriptive, non-partisan stance** — no cursive-vs-print or national-style
   endorsement; styles labeled with source/region. §3, §7F, §13.
7. **License snapshots (committed copy + SHA-256 + Wayback)** adopted from Hee-Lee Oss open-data
   practice for fonts and sources. §7B, tasks.
8. **Determinism as a tested property** — golden-file SVG tests + pinned fonts/renderer so
   review and diffs are meaningful. §6 decisions, §13.
9. **Accessibility promoted to first-class output** with a required variant matrix
   (print/left-handed/low-vision/ink-saving), not a late toggle. §5, §6, §9 M1.
10. **Paper-size realism** — A4 *and* US Letter mandated from M0 (global beneficiaries).
    §5, §6, §9.
11. **PII stance hardened** — optional name is a printed blank line; any digital personalization
    is client-side, in-memory, never persisted/logged. §3, §7E, §14.
12. **Outcome metrics de-vanitized** — adoption by a named educator and reviewer-confirmed
    accuracy replace download counts; explicit anti-metric. §4.
13. **Verified-need honesty** — all tasks `verifiedNeed:false`, `requestor: TO BE SECURED`
    until partner confirmed; flip rule defined. §2, §11, TASKS mapping.
14. **Steward / last-mile role added** to own getting sheets into real use and recording it.
    §8, §11.
15. **Per-glyph formation checklist** as the review artifact for accuracy (esp. non-Latin).
    §8, tasks.
16. **Source-citation requirement per glyph** — `sourceRef` in `GlyphModel`; `SOURCES.md` per
    script. §6, §7B.
17. **Positional/contextual forms modeled** (Arabic isolated/initial/medial/final) and RTL
    direction in the schema and the M2 exit criteria. §6, §9.
18. **Guideline-system abstraction** generalized beyond Latin (4-line, single baseline, grid,
    Devanagari headline). §6.
19. **"Add a script" playbook + reviewer-recruitment** added for sustainability/scale. §9 M4,
    §15.
20. **Cold-start thin slice** reduced to a 5-glyph pilot so the whole loop is proven before
    scaling. §9 M0.
21. **Self-serve OER publication fallback** when no partner yet — a realistic path to use, with
    the blocker surfaced. §8 DoS, §13.
22. **Staleness/refresh process** for font/source updates and license changes. §15, M4.
23. **Explicit "no recognition/capture/ML-scoring" non-goal** to bound the privacy/scope
    surface. §3, §5, §14.
24. **Font suitability caveat** — literacy/reading fonts may not be formation-optimized; verify
    fitness, not just license. §16 open Q2.
25. **Schema includes `AssetProvenance` as a first-class entity** so provenance is structural,
    not prose, and the gate can enforce it. §6 data model, §7D.

---

## Review sign-off

**Reviewer:** Senior staff engineer + TPM (drafting author), self-review pass.
**Date:** 2026-06-28 · **Version reviewed:** 0.1.0

**Completeness:** All 17 required PLAN sections present and ordered per the spec; metadata header
present; TASKS.md present with milestone tables, per-task acceptance criteria, DoDs, backlog, and
a schema-valid example Task JSON. Appendix A (25 applied improvements) and this sign-off included.

**Correctness checks performed & fixes:**
- **Schema conformance:** verified every field used in TASKS' example JSON exists in
  `packages/schema/src/schemas.ts` and uses only allowed enum values
  (`type`, `lane`, `priority`, `deliverable`, `tokenEstimate`, `status`, `riskTier`). No
  `dataset` deliverable is used (we ship `document`/`pr`); `verifiedNeed:false` everywhere a
  partner is unconfirmed; no `funded` tasks, so `fundedBudgetUsd` correctly omitted.
- **Guardrail conformance (CLAUDE.md + good-deed-definition):** public benefit ✔, freely
  available (MIT/CC-BY) ✔, no for-profit primary benefit ✔, no harm/disinfo ✔, AI-executable
  with review ✔. License/provenance gate, privacy/PII stance, non-prescriptive framing, and
  medium-risk expert (script-literate) review all encoded.
- **Honesty:** verified need and partner explicitly marked **TO BE SECURED**; outcome metrics
  beneficiary-centric with an anti-metric; no invented adopters.
- **Internal consistency:** risk tiers in §8 match per-task risk in TASKS; roadmap M0–M4 in §9
  matches the milestone sections in TASKS; roles in §11 match reviewers named in task tables.

**Residual items requiring a human decision** (see §16): SVG→PDF toolchain choice; primary Latin
font fitness; cursive open-source availability; second/third script sequencing by reviewer
availability; CC-BY-SA (Han) policy; partner-first vs publish-first timing.

**Disposition:** Approved as Draft v0.1.0 for maintainer review. No blocking defects found; open
questions are scoped and assigned, not gaps.
