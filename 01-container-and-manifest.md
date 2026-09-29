> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 01. Container and Manifest

## Container format

A VIDIMUS file MUST be a ZIP-based container, in the same family as
`.docx`/`.epub`/`.pptx`. This choice
trades a novel container format for one with decades of existing
tooling and familiarity.

## Manifest / chunk index

**Decided (2026-09-28):**
a VIDIMUS container MUST include a manifest encoded as **CBOR**
(RFC 8949). Every compliant toolchain MUST also be able to produce and
consume a defined, lossless JSON representation of the manifest (byte
strings as base64url) for debugging and tooling that doesn't carry a
CBOR dependency - the on-disk format is always CBOR, but no
implementation is ever CBOR-only-or-nothing. The manifest:

- Declares the conformance profile (Vertex/Interactive or Nexa/
  Signed-Static).
- Provides a chunk index: an ordered or addressable list of content
  chunks, each with a type from the open chunk-type registry (see
  `02-content-model-and-chunks.md`).
- Supports random access to individual chunks without requiring the
  whole file to be parsed first - directly serving both fast in-reader
  search and giving an LLM/retrieval system a pre-extracted
  representation to work from.
- Declares the maximum decompression ratio and absolute size a
  compliant reader should expect, per the container-hardening
  requirement in `05-security-requirements.md`.

## Progressive / streaming load

A compliant container SHOULD place the manifest and early chunk(s)
early in the container's byte layout, analogous to PDF's "linearized"/
fast-web-view convention, so a reader can begin rendering before the
whole file has downloaded.

## Container-level hardening (normative summary; full detail in `05-security-requirements.md`)

A compliant reader MUST:

- Refuse to decompress beyond the manifest-declared maximum ratio and
  absolute size.
- Reject entry names containing path traversal sequences (`../`) or
  absolute paths.
- Reject symlink entries.
- Reject duplicate or case-only-differing entry names within the same
  container.
- Validate the manifest against a strict, versioned schema, rejecting
  unexpected or malformed fields rather than skipping them.

## Capability-module dependency and fallback (decided, 2026-09-28)

**Decided:** any chunk
MAY carry `requires_capability` (a capability-module identifier plus
minimum version, from the tiers in `08-capability-modules-and-feature-
roadmap.md`) and `fallback` (another chunk ID to render instead, or an
inline plain-text notice). A reader lacking the named capability MUST
use `fallback` if present, or otherwise render a plain-language notice
naming the missing capability - the mechanical expression of the
graceful-fallback rule Section 08 already requires in prose.

## Content addressing (provisional, not yet part of this spec)

One open idea is using each chunk's
existing signing hash as its retrieval address as well, enabling
decentralized mirroring. This is a hypothesis, not a requirement -
flagged here as a design point to evaluate once the manifest format
itself is settled, not specified now.
