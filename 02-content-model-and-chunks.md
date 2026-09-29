> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 02. Content Model and Chunks

## Chunk types: an open registry, not a fixed enum

**Decided (2026-09-28):**
content is divided into typed chunks,
and chunk types are **an open, versioned registry** - the same
structure as MIME media types - not a closed list fixed to a format
version. A reader that doesn't recognise a chunk type degrades per
`06-extensibility-and-versioning.md`'s forward-compatibility rule
(show what it can, flag what it can't) rather than failing outright.
This is deliberate: it means a capability promised elsewhere in this
project's documents can get a reserved type identifier now, without
having to retrofit the registry later once that capability is
actually built.

**v0 baseline (a compliant reader MUST support these):**

| Type ID | Purpose |
|---|---|
| `heading` | Section/document heading, with level |
| `body-text` | Ordinary text content |
| `table` | Tabular data |
| `footnote` | Footnote/endnote content |
| `citation` | Citation/reference |
| `image-ref` | Referenced (not embedded) raster/vector image |
| `math` | Mathematical notation |
| `reveal-region` | Interactive-profile reveal/hide-governed content (`03-interactive-profile.md`) |
| `signature-block` | Signed/Static-profile signature/seal metadata (`04-signed-static-profile.md`) |
| `accessibility-meta` | Alt-text/reading-order metadata |

**v0-reserved (identifier locked in, implementation not required in v0):**

| Type ID | Reserved for |
|---|---|
| `translation-link` | Chunk-aligned parallel translations, legal-authority marking (Section 3d idea 1) |
| `plain-language-summary` | The mandated plain-language content layer (Section 3d idea 8) |
| `provenance-meta` | AI-authorship/delegation provenance (Section 3c idea 1) |
| `verification-cost-meta` | Verification-cost/energy metadata (Section 3c idea 5) |
| `representation-info` | OAIS self-describing spec-excerpt reference (Section 3d idea 4) |

Adding a further chunk type later - v0-reserved or genuinely new - is a
registry addition, not a format break, per the same forward-
compatibility rule above.

## Media/content-type inventory

Evaluation:

**Clearly in scope:** structured text, tables, mathematical notation
(the LaTeX requirement), vector diagrams/illustrations, still images/
photos, the reveal/hide interactive layer itself, embedded metadata
(authorship, revision lineage, accessibility metadata such as alt-text
and reading order for screen readers).

**Explicitly not yet decided whether native or referenced-only:**
embedded audio/video, interactive/executable content beyond reveal/
hide, 3D/CAD content, geospatial/map data. Per Section 1e, the working
lean is toward *referencing* rather than natively embedding these,
keeping the core format smaller and more durable - but this is not
decided.

**Whitelist requirement (security-relevant, see
`05-security-requirements.md`):** whatever the final inventory, a
compliant container MUST only contain content types from an explicit,
closed whitelist. A reader MUST reject any content type outside that
whitelist rather than attempting to decode it.

## Accessibility metadata

Named explicitly in Section 1e given the government/Behörden use case,
where accessibility is often a legal requirement, not a nice-to-have.
A compliant container SHOULD carry alt-text and reading-order metadata
for chunks that need it. Exact schema TBD.

## Per-chunk encryption metadata (proposed, not yet decided)

**Not a new chunk type.** Per `09-crypto-agility-and-confidentiality.md`
(2026-09-28), a proposed optional per-chunk encryption capability would
add metadata fields (`encrypted`, `enc_algorithm`, a recipient/key-
wrapping structure) applicable to **any** chunk type above via the
chunk's existing metadata, not a new `encrypted-chunk` type of its own
- an encrypted `table` is still a `table` for typing purposes, just
with its content unreadable without a key. This keeps the encryption
question orthogonal to the chunk-type registry rather than doubling
its size. Whether this capability ships at all remains open
(`07-open-questions.md`).

## Boundary: a document format, not a general container format

Every content type added as natively embeddable is a 20+ year support
commitment; every type left as an
externally-referenced asset keeps the core format smaller at the cost
of reveal/hide-style interactivity for that content. This tension is
tracked, not resolved, here.
