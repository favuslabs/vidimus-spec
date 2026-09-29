> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 08. Capability Modules and Feature Roadmap

This file promotes every idea gathered from this project's design process, Sections
3b, 3c, and 3d from open-ended hypothesis to a planned item: which
profile(s) it belongs to, whether it is baseline (every conformant
decoder MUST implement it) or an optional capability module (a decoder
MAY implement it, declares support, and every other decoder MUST
degrade gracefully when it doesn't), and how any tension between ideas
- or between an idea and an already-committed rule - is resolved. It
does not replace this project's own internal design notes, which are kept in full as
the rationale record - consistent with this project's practice of marking earlier text superseded rather than deleting it;
this file is the concrete planning layer sitting on top of it.

## The conformance-tier mechanism that makes "modular" precise

Two tiers, not a loose "optional or not" judgment call per feature:

- **Baseline conformance** (MUST, both profiles unless stated
  otherwise): every feature a decoder must implement to call itself a
  VIDIMUS reader at all. A file that only uses baseline features MUST
  render fully and correctly in any conformant reader.
- **Capability modules** (MAY, declared): a named, versioned capability
  a decoder can implement or not. The manifest (`01-container-and-
  manifest.md`) carries, per chunk or per document, which module (if
  any) that content depends on. A decoder that lacks a module MUST NOT
  fail, crash, or silently show nothing without explanation - it MUST
  apply the **declared fallback** for that content: either an
  author-supplied fallback chunk (plain content shown in place of the
  module-dependent one) or, if none was supplied, a short, plain-
  language notice naming the missing capability ("this section
  requires wallet-verification support not available in this reader"),
  per the plain-language-comprehensibility rule already established
  above.

This single mechanism - baseline vs. declared module vs. mandatory
fallback - is what answers "does every decoder need every capability?"
for every idea below without deciding it feature-by-feature from
scratch.

## Section 3b (decentralization and adaptivity) - promoted

| Idea | Profile(s) | Tier | Resolution |
|---|---|---|---|
| A. Content-addressed chunks | Vertex + Nexa | **Baseline.** | Both profiles already hash chunks for the Merkle-tree/manifest model (Section 3); content-addressing is that same hash used as the chunk's storage key. No new mechanism, no opt-out - it's a property of the manifest format itself, not a reader-visible capability. |
| B. Gossiped, CT-style revocation log | Nexa only | **Capability module, opt-in per lookup.** | Directly in tension with Section 3a's no-implicit-network-access rule if done automatically. Resolved by making revocation checking an explicit, user-initiated action only - never triggered on open, never silent - exactly like a browser's OCSP-stapling-vs-live-check distinction. A reader without this module simply cannot check revocation and MUST say so in its trust summary (Section 3d's mandated plain-language line: "revocation status: not checked by this reader") rather than implying a clean check happened. |
| C. Declarative, reader-evaluated context-adaptivity | Vertex + Nexa | **Split.** Device/print-vs-screen conditions: **baseline**. Credential-gated and other advanced condition types: **capability module** (see 3c-2 below, same module). | Splitting avoids forcing every reader to integrate with external systems (wallets) just to support ordinary responsive-layout conditions. |
| D. CRDT-based local-first collaborative editing | **Vertex only** - out of scope for Nexa by definition (sealed/immutable content cannot be collaboratively mutated; a Nexa document is what a Vertex document becomes once finalized). | **Capability module.** | A Vertex reader without this module MUST still open and display a CRDT-authored document correctly (it just can't merge concurrent local edits) - the CRDT state resolves to a normal, readable chunk sequence for any reader that doesn't participate in the merge. |

## Section 3c (2040-perspective requirements) - promoted

| Idea | Profile(s) | Tier | Resolution |
|---|---|---|---|
| 1. AI-authorship/delegation provenance | Vertex + Nexa | **Baseline schema, optional population.** | The metadata *field* is part of the baseline schema (every decoder MUST be able to read and display it if present, and MUST NOT choke on it if absent) per the forward-compatibility rule in `06-extensibility-and-versioning.md`. Whether an authoring tool *populates* it is a separate, non-technical question the spec doesn't mandate. |
| 2. EUDI Wallet-native reveal conditions | Vertex + Nexa | **Capability module** (same module family as 3b-C's advanced conditions). | Safe default on missing module: content stays **hidden**, not shown - the reverse of 3b-B's fallback, because a credential-gated chunk is confidentiality-driven; showing it by default on missing capability would be a security regression, while a revocation check failing closed only degrades trust confidence, not confidentiality. |
| 3. Zero-knowledge redaction proofs | **Nexa only** | **Reserved extension point, not a launch feature.** | Does not replace the Merkle-tree reveal model (`03a`, `04-signed-static-profile.md`), which remains the mandatory baseline signature scheme. ZK-proof support is filed under the crypto-agility mechanism (`04-signed-static-profile.md`, `06-extensibility-and-versioning.md`) as an additional, optional proof type a future version could add without a format break - explicitly not required for the pre-2030 launch. |
| 4. Modality-agnostic/ambient consumption | Vertex + Nexa | **Baseline design principle, not a toggle.** | Not a reader feature at all - it's a property the content model (`02-content-model-and-chunks.md`) already must have (structured, unambiguous chunks) to serve AI-native reading (Section 1e). Voice/AR/ambient rendering is then just another renderer built on the same baseline content, same as a screen renderer is. |
| 5. Verification-cost/energy metadata | Vertex + Nexa | **Baseline schema, optional population, capability module to render.** | Same schema-vs-population split as idea 1. Displaying it prominently (vs. just tolerating its presence) is a capability module, since not every reader UI will have a place for it. |
| 6. Digital custody transfer | Vertex + Nexa | **Reserved extension point, not a launch feature.** | Explicitly flagged as intersecting real, jurisdiction-specific inheritance law this project cannot resolve. Treated the same way as idea 3: a named gap the crypto-agility/versioning mechanism must leave room for, a candidate for the ~2050 second edition (`06-extensibility-and-versioning.md`) rather than a pre-2030 requirement. |

## Section 3d (further-perspectives round) - promoted

| Idea | Profile(s) | Tier | Resolution |
|---|---|---|---|
| 1a. Chunk-aligned parallel translations | Vertex + Nexa | **Baseline.** | A direct extension of the chunk model (`02-content-model-and-chunks.md`) already committed to both profiles. |
| 1b. "Legally authoritative version" marker | **Nexa only.** | **Baseline field, but only meaningful/settable on Nexa.** | A Vertex document is mutable and not legally sealed, so an "authoritative version" marker on one would be misleading; the field MUST be accepted by readers on any profile (forward-compat) but authoring tools SHOULD only expose it for Nexa documents. |
| 2. Script/font/bidi whitelist for all EU languages | Vertex + Nexa | **Baseline.** | Folded directly into the existing content-type whitelist (`05-security-requirements.md`) - not a separate feature, a scope correction to one already committed. |
| 3. Language-keyed reveal/hide conditions | Vertex + Nexa | **Baseline** (not a module). | Unlike wallet-credential conditions, this needs no external integration - every reader already has a locale setting - so it belongs in the same tier as 3b-C's basic device/print conditions, not the advanced-conditions module. |
| 4. Self-describing "Representation Information" (OAIS) | Vertex + Nexa | **MUST for Nexa, SHOULD for Vertex.** | Nexa is this format's archival/legal-grade profile - self-sufficiency against the spec disappearing is exactly its purpose, so this is mandatory there. Vertex documents benefit but are not the profile this project stakes its 20-year archival claim on, hence SHOULD not MUST. |
| 5. Fixity checking | Vertex + Nexa | **Free consequence of baseline, not a new feature.** | Requires no format change once 3b-A's content-addressed hashing is baseline - fixity checking is just re-hashing and comparing, a verification-tool behaviour, not a decoder capability that needs a toggle. |
| 6. Migration-vs-emulation framing for the ~2050 edition | n/a | **Governance decision, not a decoder feature.** | Recorded in `06-extensibility-and-versioning.md`'s existing major-edition-boundary section; no module or baseline entry needed here. |
| 7. Mandated plain-language trust summary | **MUST for Nexa.** Vertex equivalent (edit-status summary): **SHOULD.** | Baseline rendering requirement, not optional - this is the padlock-icon fix and the spec-mandated half of Section 5c's honeycomb-icon identity element; a reader cannot claim Nexa conformance without rendering it. |
| 8. Plain-language content layer | Vertex + Nexa | **Baseline to render if present; optional to author.** | Trivial for any decoder to support (it is plain text) - no reason to gate it behind a module. Whether an authoring tool produces one is a content decision, not a decoder one. |
| 9. Shared visual vocabulary for sealed/revealed/detachable states | **MUST for any decoder rendering Nexa content**, SHOULD for Vertex's editable/sealed-chunk distinction (Section 1d). | Baseline rendering requirement, paired with idea 7 above and with `05-security-requirements.md`'s trust-comprehensibility goals. |

## Section 5c (identity/attractiveness framings) - not a feature list

Section 5c is explicitly public-messaging framing of mechanisms
already covered above (or already baseline in Sections 3-4d) - it adds
no new decoder-facing feature, module, or profile assignment, and
nothing in this file changes because of it. Recorded here only so a
future reader of this roadmap doesn't go looking for a "5c module" that
doesn't exist.

## Crypto-agility infrastructure (now specified) and a proposed new module

**Not a Section 3b/3c/3d idea being promoted, but infrastructure the
existing Nexa baseline requirement (crypto-agility, `04-signed-static-
profile.md`) now has a concrete mechanism for** - see
`09-crypto-agility-and-confidentiality.md` (2026-09-28): the open
cryptographic-algorithm registry, multi-signature/hybrid signing, and
counter-signatures are all **baseline mechanism for Nexa** (a
conformant Nexa decoder MUST be able to verify multiple signature
blocks and a counter-signature), independent of which specific
algorithms end up in the registry.

| Idea | Profile(s) | Tier | Resolution |
|---|---|---|---|
| Multi-recipient chunk encryption (confidentiality, distinct from reveal/hide) | Vertex + Nexa | **Proposed capability module, not yet decided whether it ships in v0** (`07-open-questions.md`). | Would reuse this file's own `requires_capability`/`fallback` mechanism exactly as any other capability module: a reader without a decryption key uses the chunk's declared fallback. Directly relevant to Section 3c idea 2 (EUDI Wallet-native reveal conditions) - true wallet-gated confidentiality (vs. wallet-gated declarative hiding) would need this module underneath it. |
| Algorithm deprecation list | Nexa (verification requirement) | **Baseline for Nexa verifiers.** | Not a decoder-optional feature - a compliant Nexa verifier MUST check signature algorithm(s) against the current deprecated-algorithm list and reflect a downgraded trust status in the mandated plain-language trust summary (idea 7 above) when only a deprecated algorithm remains valid. |

## Net effect on `07-open-questions.md`

The "Speculative, explicitly not decided (Section 3b)" list in
`07-open-questions.md` is superseded by this file's tables, not
deleted - that list now carries a note pointing here; what
was an open "is this in scope at all" question for ideas A-D is
resolved to "yes, at the tier and with the resolution shown above,"
though the *exact encoding* of each module (manifest flag syntax,
condition-language grammar, etc.) remains genuinely open engineering
work, tracked as new entries below.

## New open questions this promotion surfaces

- Exact manifest syntax for declaring a chunk's capability-module
  dependency and its fallback chunk/notice.
- Exact condition-language grammar covering both the baseline tier
  (device/print/language) and the advanced-conditions module
  (wallet-credential, future condition types) from one consistent
  grammar, per Section 3b idea C / Section 3d idea 3.
- Exact wording and required fields of the mandated plain-language
  trust summary (idea 7) and its Vertex-equivalent edit-status summary.
- Exact schema for the AI-authorship/delegation and verification-cost
  metadata fields (3c ideas 1 and 5).
