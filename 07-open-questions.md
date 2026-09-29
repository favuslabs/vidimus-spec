> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 07. Open Questions

A running checklist of everything genuinely unresolved, gathered from
across this project's design work so future spec work has one place to check
rather than scattered flags. Nothing below should be assumed decided
just because it appears in one of the other files above using
normative language drawn from a current, revisable decision.

## Architecture (Section 3)

- ~~Exact chunk-index encoding: JSON vs. CBOR vs. something else.~~
  **Decided 2026-09-28: CBOR, with a mandatory JSON equivalence for
  tooling - see
  `01-container-and-manifest.md`.**
- ~~Exact chunk-type vocabulary beyond the illustrative list in
  `02-content-model-and-chunks.md`.~~ **Decided 2026-09-28: an open,
  versioned registry (not a fixed enum), with a v0 baseline list and a
  set of v0-reserved identifiers for already-promised capabilities -
  see
  `02-content-model-and-chunks.md`. Still open: the exact field schema
  *within* each chunk type beyond its type ID.**
- Exact Merkle-tree construction and signature algorithm(s) for the
  Nexa profile. **The crypto-agility mechanism itself is decided in
  outline, 2026-09-28** (`09-crypto-agility-and-confidentiality.md`):
  an open algorithm registry, hybrid/multi-signature support, and
  counter-signatures as the after-the-fact retrofit path. **Still
  needing dedicated outside cryptography and legal review** (see
  further engineering work): the actual
  signature/hash/post-quantum algorithms, and the exact counter-
  signature byte format.
- Whether the proposed optional per-chunk encryption capability
  (`09-crypto-agility-and-confidentiality.md`) ships in v0 at all, or
  is correctly a later module once real demand for cryptographic (not
  just declarative reveal/hide) confidentiality is confirmed. Not
  decided as of 2026-09-28.
- Media-type resolution for the archival-vs-efficient-codec tension
  (e.g. dual representations for archivally important images).
- Version/revision-history storage as deltas vs. full copies.
- Manifest syntax for a chunk's capability-module dependency and
  fallback: **decided in outline 2026-09-28 (`requires_capability` +
  `fallback` fields (see `01-container-and-manifest.md`),
  `01-container-and-manifest.md`); the exact field-value grammar
  (capability-ID/version format) remains open.**

## Content-model boundary (Section 1e / `02-content-model-and-chunks.md`)

- Whether audio/video, 3D/CAD, and geospatial data are natively
  embeddable or referenced-only for v0 - **explicitly deferred, not
  decided, as of 2026-09-28**: staying referenced-only for v0 is a default, not a
  ceiling, since the chunk-type registry (above) and the versioned
  content-type whitelist (`05-security-requirements.md`) both allow
  adding a native embed type later without a format break.
- Accessibility metadata's exact schema.

## Naming and identity (Section 4)

- Whether "Vertex" and "Nexa" survive domain/trademark checks, or a
  reserve candidate is needed instead (candidates tracked internally, not listed publicly to avoid pre-emptive domain/trademark squatting).
- Nice-class trademark filing scope and cost, pending real counsel
  review.

## Speculative, explicitly not decided (Section 3b) - superseded by 08

**Superseded:** whether ideas A-D below are in scope at all is now
resolved in `08-capability-modules-and-feature-roadmap.md` (baseline,
capability module, or reserved extension point, per idea). Kept here,
not deleted, because the *exact encoding* of each remains open
engineering work - see "New open questions this promotion surfaces" at
the end of `08-capability-modules-and-feature-roadmap.md` for what is
still genuinely undecided.

- Content-addressed chunks for decentralized storage/retrieval (idea
  A) - now baseline, per `08-capability-modules-and-feature-roadmap.md`.
- A Certificate-Transparency-style decentralized trust/revocation log
  (idea B) - now a capability module, explicit-user-action-only, per
  `08-capability-modules-and-feature-roadmap.md`.
- Declarative, reader-evaluated context-adaptivity beyond basic
  reveal/hide (idea C) - now split baseline/module, per
  `08-capability-modules-and-feature-roadmap.md`.
- CRDT-based local-first collaborative editing (idea D) - now a
  Vertex-only capability module, per
  `08-capability-modules-and-feature-roadmap.md`.

## Repository and process (Section 5a)

- Exact point at which `vidimus-adapters`, a standalone verifier repo,
  and a viewer application get created, per the "no scaffolding ahead
  of what's real" sequencing rule.
- Whether this spec skeleton itself gets promoted to canonical status
  over an existing spec section, and when.

## Governance (Section 1a / 5a)

- Final decision on trademark ownership structure (an individual
  founder vs. a future association), independent of repository hosting
  (already decided: Favus Labs organisation).
- Which path, if any, VIDIMUS eventually takes toward formal
  standardization or broad establishment - options range from staying
  a de facto standard driven by adoption, to a lightweight national
  specification process, to a full national or European standard, to
  submission to an international standards body, to an IETF/W3C-style
  process, to a standard becoming load-bearing through regulatory
  reference rather than formal standard status at all, to holding the
  specification through a dedicated steward organization rather than a
  single company. Not decided, not mutually exclusive, and not
  expected to be resolved before the specification itself has real
  adoption to build on.
