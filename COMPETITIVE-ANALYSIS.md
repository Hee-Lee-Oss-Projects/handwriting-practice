# Competitive & Improvement Analysis — handwriting-practice

> Scope: open, printable handwriting/penmanship practice worksheets across multiple scripts,
> generated from open fonts. Guardrails: open license; open fonts (OFL); pedagogically-sound
> letter formation; multilingual/multi-script; accessibility; printable/low-resource.
> Reviewed against PLAN.md v0.1.0 (2026-06-28) and TASKS.md.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually strong: it is **data-first** (a cited, reviewed formation model is the
asset; PDFs are a projection), and it correctly identifies that the central risk is not
engineering but **licensing + formation correctness**. Findings, by guardrail:

**Open-font licensing / OFL (mostly correct — one nuance to tighten).**
- The plan's core OFL claims are accurate. OFL **explicitly permits embedding fonts in documents
  under any terms**, and embedding does not change the document's license — so CC-BY PDFs that
  embed/subset an OFL font (e.g. Andika) are clearly permissible
  ([OFL-FAQ](https://openfontlicense.org/ofl-faq/),
  [Wikipedia: SIL OFL](https://en.wikipedia.org/wiki/SIL_Open_Font_License)). Good.
- The **Reserved Font Name (RFN)** handling (§7A) is correct in spirit: an RFN may not be used by
  a *modified font*, and the plan keeps the stroke/arrow **overlay as a separate layer** rather
  than modifying outlines, which avoids the RFN trigger. **Nuance to make explicit:** subsetting
  a font (almost certain in PDF generation) is *not* "modification" under OFL and does **not**
  trigger RFN — the plan should state this so the provenance-gate doesn't false-positive on
  ordinary subsetting. Conversely, if a future variant *re-points/cleans an outline* (even
  slightly), that **is** a derivative and must be renamed; the gate's `permitsDerivatives` +
  `reservedFontName` fields support this but the threshold ("what counts as modifying an outline")
  should be written down.
- The "OFL fonts may not be sold *on their own*" caveat (§7A) is correctly stated and not a
  problem here (fonts ship as document content, with OFL text in-repo).

**Pedagogically-correct letter formation (the strongest design decision; verify the gap).**
- The plan's single best insight: **a font gives you outlines, not formation.** Stroke *order*,
  *direction*, *start points*, and *pen-lift pauses* are not in a font file and cannot be
  inferred from one. The plan authors an **original overlay** and requires a **per-glyph
  `sourceRef`** to an authoritative formation source plus reviewer sign-off. This is exactly
  right and is the thing nearly every competitor gets wrong-or-silent (see §2).
- **Caveat the plan already flags (Q2) and should harden:** a *reading*-optimized literacy font
  (Andika is reading/legibility-optimized) is **not the same as a formation-optimized** letter
  shape. Andika ships **stylistic variants of `a`, `g`, `t`** precisely so publishers can match
  *local handwriting instruction* ([SIL Andika](https://software.sil.org/andika/)). The plan must
  treat "font outline" and "formation target shape" as **two separable decisions**, both
  reviewed — outline legality ≠ outline is the shape children should copy.
- **Missing detail:** the formation model needs **stroke-level pen-lift / continuity** semantics
  for cursive (where the join *is* the lesson). `Stroke.pause?` hints at it but the schema should
  explicitly model joins/entry-exit strokes before M4 cursive.

**Multi-script support (architecture is sound; complexity is under-budgeted).**
- ISO-15924 typing, `direction` (ltr|rtl), `positionalForms` (Arabic isolated/initial/medial/
  final), and a generalized `GuidelineSystem` (Latin 4-line, baseline, grid, Devanagari headline)
  are the right primitives. RTL + joining is genuinely hard and the medium-risk reviewer gate is
  appropriate.
- **Gaps:** (a) **Indic conjuncts and matras** (vowel signs attaching above/below/around a base)
  and **Hangul syllabic-block composition** are *layout/shaping* problems, not just glyph lists —
  they need a **complex-text-shaping step (HarfBuzz)**, which the plan never names. (b) **Arabic
  contextual shaping** likewise requires shaping, not codepoint→glyph. The renderer section (§6)
  assumes `path` SVG data per glyph but doesn't say how positional/conjunct forms are *obtained*
  from the font (cmap + GSUB). This is the biggest technical under-specification.

**Accessibility (first-class — good; one risk).** The required variant matrix (print /
left-handed / low-vision / ink-saving) and "print on real paper" review are excellent and ahead
of every competitor. **Risk:** left-handed support is treated as "mirroring guides/arrows," but
left-handed *formation* sometimes differs (push vs pull strokes, paper angle) — confirm with the
lived-experience reviewer the plan already calls for, don't just mirror.

**Printability / generation pipeline (correct, one open decision).** SVG-source → PDF, A4 +
Letter, grayscale-safe, deterministic golden-file SVG, no network at render time — all correct
and reviewable. The **SVG→PDF library is unchosen (Q1)** and is load-bearing for determinism and
font-subsetting fidelity; this is the right thing to resolve in M0. Recommend evaluating
`resvg`/`typst`/`weasyprint`-class tools with **HarfBuzz-backed shaping** so the same choice also
solves the multi-script shaping gap above.

**Scope / completeness.** All 17 sections present; non-goals are disciplined (no recognition/ML,
no therapy claims, no accounts/PII, no font foundry). `verifiedNeed:false` honesty is correct.
The schema/TASKS mapping is consistent. **Two completeness gaps:** (1) no **font-shaping/
complex-script rendering** component named; (2) **cursive join modeling** deferred without a
schema placeholder. Both are M2+/M4 concerns but should be acknowledged in §6 now.

---

## 2. Competitive landscape

**Free worksheet *generators* (closest substitutes).**
- **handwritingworksheets.com ("Amazing Handwriting Worksheet Maker").** Offers Print, Cursive,
  and **D'Nealian-style** with dotted/dashed/hollow trace variants; lets you set letter/line
  color. *Strengths:* fast, genuinely free, the de-facto default. *Weaknesses:* **no terms/license
  on generated output** (only a bare copyright notice), **no stroke-order/formation guidance**,
  **no non-Latin scripts**, **no accessibility**, and "D'Nealian style" rides on a trademarked
  method (see trademark note below).
  ([homepage, fetched](https://handwritingworksheets.com/))
- **WorksheetWorks.com handwriting** — customizable Latin print/cursive sheets; free-with-ads,
  proprietary terms, Latin-only, no formation sourcing.
  ([WorksheetWorks](https://www.worksheetworks.com/english/writing/handwriting.html))
- **Learning Without Tears "Worksheet Maker Lite"** — vendor tool for the **Handwriting Without
  Tears** method; high pedagogical quality but **proprietary fonts/method**, walled to the brand.
  ([LWTears](https://www.lwtears.com/resources/worksheet-maker-lite))

**Pre-made worksheet libraries.**
- **K5 Learning** — large free printable print + cursive sets, K–grade 5; *Strength:* volume,
  free, well-organized. *Weakness:* Latin-only, **no formation sourcing/stroke order**, site terms
  restrict redistribution; upsells workbooks.
  ([K5 cursive](https://www.k5learning.com/cursive-writing-worksheets))
- **Education.com** — big library but **hard paywall: only 3 free downloads/month**, then
  Premium; non-redistributable.
  ([Education.com free-limit](https://support.education.com/how-many-resources-do-i-get-free-each-month/))
- **Teachers Pay Teachers** — huge marketplace incl. multilingual/Arabic sets, but **licenses are
  per-teacher, non-transferable, personal-classroom-only — redistribution is prohibited**; quality
  and pedagogical accuracy are uneven/uncredited.
  ([TpT Terms of Service](https://www.teacherspayteachers.com/Terms-of-Service))

**Desktop / font tooling.**
- **StartWrite** — paid desktop software whose fonts are **proprietary look-alikes** of
  Zaner-Bloser / D'Nealian / HWT (the real branded fonts are **not sold** and have no identical
  free equivalent). *Strength:* method fidelity; *Weakness:* paid, proprietary, Latin-centric,
  redistribution of outputs constrained.
  ([StartWrite fonts](https://startwrite.com/sample-fonts),
  [ZB vs D'Nealian](https://cursiveworkshop.com/article/dnealian-vs-zaner-bloser-how-do-their-cursive-fonts-differ))
- **"Tracing"/dotted school fonts** (Amaze/Educational Fontware-class) — the convenient ones are
  **proprietary, often no-embed/no-redistribution**, which silently makes any redistributed sheet
  built from them infringing — exactly the landmine PLAN §7 targets.

**Open building blocks (allies, not competitors).**
- **SIL Andika (OFL)** — literacy-designed, single-story `a`/`g`, disambiguated `I/l/1`, low-vision
  variant, **stylistic alternates for handwriting-instruction matching** — an ideal Latin *outline*
  source. ([SIL Andika](https://software.sil.org/andika/),
  [Andika & visually impaired](https://software.sil.org/andika/andika-and-the-visually-impaired/))
- **KanjiVG (CC-BY-SA 3.0)** — authoritative, structured **stroke-order** SVG for Han; share-alike,
  attribution required — usable for any future Han content if SA is honored and kept separable.
  ([KanjiVG](https://kanjivg.tagaini.net/), [KanjiVG GitHub](https://github.com/KanjiVG/kanjivg))
- **Non-Latin niche sites** (Kalimah, Iqra Games, TracerTutor for Arabic) — show real demand and
  good practice (positional forms, RTL arrows) but are **closed-license, single-script, single-
  vendor, unsourced**. ([Kalimah](https://kalimah-center.com/arabic-alphabet-handwriting-and-tracing/),
  [TracerTutor Arabic](https://www.tracertutor.com/arabic/handwriting-worksheet))

**Trademark note (a gap competitors live in):** "D'Nealian," "Zaner-Bloser," and "Handwriting
Without Tears" are **trademarked methods**; tools advertise "D'Nealian-*style*" because the genuine
fonts aren't licensed for redistribution. The plan's stance — cite *facts of formation*, build an
*original* overlay, never trace proprietary forms, never claim a brand — is the legally clean path
and a differentiator.

---

## 3. Gaps we can fill

1. **Truly open, redistributable output.** Every mainstream option is paywalled (Education.com),
   personal-use-only (TpT), bare-copyright (handwritingworksheets.com), or proprietary
   (StartWrite). **CC-BY PDFs + CC-BY/SA models that an NGO can fork, translate, and re-host** is
   an unoccupied niche.
2. **License-clean by construction.** A **default-deny provenance gate** that fails the build on
   any unverified font/source is unique — competitors silently ship proprietary "school fonts."
3. **Sourced, reviewed formation (not font outlines).** Per-glyph `sourceRef` + reviewer sign-off
   makes correctness *auditable* — no competitor cites stroke-order provenance at all.
4. **Multi-script done correctly,** with script-literate review for RTL/joining/conjuncts —
   versus generic tools that mangle non-Latin, and single-vendor niche sites that cover one script.
5. **Accessibility as standard output** (left-handed, low-vision, ink-saving) — essentially absent
   from free competitors.
6. **Low-resource / offline / print-faithful** A4 *and* Letter, grayscale-safe — built for
   under-resourced classrooms, not ad-supported web flows.
7. **Reusable, neutral commons** — multiple labeled national styles side-by-side instead of one
   brand's method, so any community can adopt without buying in.

---

## 4. Differentiators to win

- **The formation *commons*, not a worksheet dump.** The durable asset is cited, reviewed,
  machine-readable formation data; PDFs regenerate from it in any size/style/script. Nobody else
  ships the *data*.
- **Provenance gate = trust.** "Every glyph's outline and every stroke-order claim is open-licensed
  and sourced, or the build fails" is a credibility claim no competitor can make.
- **Correctness you can audit** (per-glyph checklist + script-literate sign-off) vs. anonymous PDFs.
- **Genuinely multi-script + accessible + free + offline** in one tool.
- **Legally fearless on method branding** — original overlays + cited facts sidestep the
  D'Nealian/ZB/HWT trademark+font trap that forces competitors into "style" hedging or payment.

---

## 5. Claude API leverage (and where Claude must NOT decide)

**High-value, low-risk uses (Claude as generator/assistant under human gates):**
1. **Practice content & word lists per script/level** — decodable, frequency-appropriate words and
   short copy-text that *only use already-taught letters*, with profanity/cultural screening; a
   natural tie to `decodable-readers`.
2. **Layout/template synthesis & QA** — propose `SheetTemplate` row recipes (trace→fade→copy→free),
   compute spacing/repetition, and **lint generated SVG/PDF** for overflow, contrast, and guideline
   alignment across A4/Letter.
3. **Multilingual instruction & UI strings** — translate sheet headers, learner/teacher
   instructions, and accessibility notes into the target language (then human-verified).
4. **Provenance/licensing triage** — draft `AssetProvenance` records, summarize a font/source
   license, flag NC/no-embed terms for the human gate (Claude proposes; gate + human decide).
5. **"Add a script" research drafts** — assemble candidate open fonts + authoritative formation
   sources + reviewer-recruitment leads for a new script, as a starting dossier.

**Where Claude must NOT be the decider (hard guardrails):**
- **Letter-formation pedagogy** — stroke order/direction/start points must come from a **cited
  authoritative source and a script-literate/qualified human reviewer**, never from Claude's
  belief about "how to write a letter." (Wrong formation taught early is hard to unlearn.)
- **Font-license verification** — Claude may *summarize*, but the **provenance gate + human**
  decide PASS/FAIL; a model assertion is not legal clearance (default-deny).
- **Stroke-order correctness per script** — especially RTL joining, Indic conjuncts/matras,
  Hangul blocks, Han — must be human-verified; no LLM-asserted contextual forms ship unreviewed.
- **No copyrighted/proprietary fonts or worksheet artwork** — Claude must refuse to reproduce
  D'Nealian/ZB/HWT branded outlines or copy proprietary trace art; only OFL/PD/CC outlines + our
  original overlay.
- **No clinical/therapy framing** — Claude must not generate dysgraphia-treatment or efficacy
  claims; practice-material framing only.

---

## 6. Ten concrete optimizations

1. **Name a complex-text-shaping engine (HarfBuzz) in §6 now.** Arabic joining, Indic conjuncts/
   matras, and Hangul blocks need real shaping (cmap+GSUB), not codepoint→path; pick an SVG→PDF
   tool that shapes (e.g. resvg/typst/weasyprint-class) and golden-test shaped output.
2. **Split "outline source" from "formation target shape" as two reviewed decisions.** Record both
   in the model; a legal outline is not automatically the shape a child should copy.
3. **Model cursive joins explicitly** (entry/exit strokes, pen-lift continuity) in the schema
   before M4, even if unused at M1 — retrofitting a join model later is expensive.
4. **Write down the OFL subset-vs-modify threshold** in the gate: subsetting ≠ derivative (no RFN
   rename); any outline edit ⇒ derivative ⇒ rename. Prevents both false fails and real breaches.
5. **Adopt KanjiVG's pattern (cited, structured stroke data) as the model for *every* script,** and
   carry a `shareAlike` flag so SA-derived models (KanjiVG) stay separable from CC-BY ones.
6. **Ship a machine-readable per-sheet provenance sidecar** (JSON: font record + source refs +
   reviewers + license) alongside the human-readable footer — enables OER re-hosting and the
   outcome ledger.
7. **Add print-fidelity automated checks**: PDF/A-ish determinism, embedded-subset verification,
   min stroke-width and contrast thresholds for low-vision, and an "ink coverage" budget for the
   ink-saving variant.
8. **Pre-empt the partner gap** by shipping a **self-serve OER bundle** (PDF pack + model +
   license + sidecar) from M1, so `verifiedNeed:false` still reaches real users while a named
   partner is pursued.
9. **Build a tiny "formation review harness"** that renders each glyph's strokes *in order* as an
   animated/numbered proof sheet for the reviewer — makes the per-glyph checklist fast and the
   sign-off auditable.
10. **Left-handed = formation, not just mirroring.** Add push/pull-stroke and paper-angle notes
    validated by a left-handed reviewer; don't auto-mirror arrows blindly.

---

## 7. Parallel & perpendicular spin-offs

- **Reusable worksheet-generation engine.** The `(model + template + variant + paper) → SVG → PDF`
  core with a provenance gate and accessibility matrix is **script-agnostic and domain-agnostic**.
  Factor it into a shared `worksheet-core` that other Hee-Lee Oss printables reuse.
- **`math-manipulatives-printables`** — number lines, grids, base-10, fraction strips, clock faces:
  same deterministic SVG→PDF engine, same A4/Letter + accessibility + CC-BY pipeline.
- **`decodable-readers` / `literacy-from-zero`** — direct content tie-in: handwriting word lists
  draw only on letters/phonemes already introduced by the reader sequence; share the
  letter-introduction order and the multilingual content pipeline.
- **`open-accessible-fonts`** — feeds this project its vetted OFL outline sources and consumes its
  "formation target shape" feedback; shared low-vision/legibility review pool.
- **Formation-data commons (standalone).** The cited, reviewed `ScriptModel` JSON is valuable
  independent of worksheets — usable by app makers, OER authors, and handwriting-instruction tools;
  publish it as a versioned open dataset with KanjiVG-style provenance.
- **MCP server.** Expose `list_scripts`, `get_glyph_formation`, `render_sheet(script, template,
  variant, paper)`, and `check_provenance(asset)` as an **MCP server** so any agent/IDE can
  generate license-clean practice sheets on demand — turns the engine into reusable infrastructure
  and a natural Claude integration surface.

---

## 8. Open questions

1. **Shaping toolchain:** which single SVG→PDF library gives deterministic output *and*
   HarfBuzz-class complex shaping (Arabic/Indic/Hangul) so we don't bolt on a second engine later?
   (Sharpens PLAN Q1.)
2. **Andika fitness:** does a reading-optimized literacy font's outline match the *formation target*
   for instruction, or do we need its stylistic alternates / a different OFL outline per region?
   (PLAN Q2.)
3. **Cursive:** is there an *open* continuous-cursive formation source + reviewer, or do we ship
   print-only until one exists — and does the schema model joins from day one regardless? (PLAN Q3.)
4. **Share-alike posture:** adopt KanjiVG/CC-BY-SA into the repo (kept separable) or keep the
   commons strictly CC-BY and defer Han? (PLAN Q5.)
5. **National-style neutrality:** default to shipping multiple labeled style variants (single/
   double-story `a`/`g`, open/closed `4`) rather than picking one? (PLAN Q6.)
6. **Partner-first vs publish-first:** hold a script until a named educator commits, or publish the
   OER bundle and pursue adoption in parallel? (Affects `verifiedNeed` timing; PLAN Q7.)
7. **Reviewer supply:** what is the recruitment pipeline (SIL-style literacy programs, language-
   community orgs) that prevents reviewer scarcity from silently blocking scripts? (PLAN Q reviewer.)
8. **Trademark line:** how close can a labeled "D'Nealian-*style*" model come before it risks the
   trademark — do we avoid brand names entirely and describe styles regionally/descriptively?
