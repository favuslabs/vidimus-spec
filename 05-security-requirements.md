> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 05. Security Requirements

Consolidated from restated as checkable
requirements. Applies to both profiles unless noted otherwise. Framed
honestly, per Section 3a: these requirements make malicious embedded
code hard to impossible by removing attack-surface categories by
construction - they cannot guarantee a buggy reader implementation is
unexploitable.

## No execution model (the single biggest lever)

The specification MUST NOT define any execution model: no scripting
language, no macro system, no embedded WASM, no expression language
capable of growing into a general-purpose one. This directly avoids
the PDF-JavaScript and Office-macro attack-surface category.

## Container and parser hardening

A compliant reader MUST:

- Enforce a manifest-declared maximum decompression ratio and absolute
  size (zip-bomb protection).
- Reject path-traversal entry names, absolute paths, symlink entries,
  and duplicate/case-only-differing entry names (the Janus/ZIP-polyglot
  ambiguity class).
- Validate the manifest/chunk index against a strict, versioned schema,
  rejecting unexpected or malformed fields outright rather than
  skipping and continuing.
- Disable DTD processing and external entity resolution outright for
  any XML-family content that ever enters the format (XXE prevention).
- Verify that a chunk's or asset's declared content type matches its
  actual byte signature (magic number) before processing it.
- Only accept content types from the closed whitelist in
  `02-content-model-and-chunks.md`; reject anything else.

## Font and media decoding (implementation guideline, not a format rule)

Compliant readers SHOULD decode untrusted embedded media (fonts,
images) through process-isolated or memory-safe decoders, not through
whatever general-purpose, memory-unsafe library happens to be
installed on the host. Named explicitly because this is real,
historically-exploited attack surface that the container format alone
cannot remove.

## No implicit network access

Opening or rendering a VIDIMUS file MUST NOT itself trigger any network
request. Every external reference is either bundled in the container,
or a chunk-level convention followed only after the person's own
explicit action - never an automatic fetch triggered by parsing.

## Nexa-specific: closed-world signature coverage

See `04-signed-static-profile.md` - a verifier MUST reject any content
not covered by the signed Merkle tree.

## Adapters are a separate trust boundary

`vidimus-adapters` converters (LaTeX, and future formats) parse
untrusted external input and are not covered by any requirement above.
They need their own hardening statement once real adapter work begins.

## Testing requirement

The conformance test corpus (`vidimus-spec`'s eventual test suite)
MUST include a deliberately malformed/adversarial counterpart to the
valid-file corpus: zip bombs, path-traversal entries, magic-byte/
declared-type mismatches, truncated or tampered signature trees.

## Process requirement

A `SECURITY.md` with a defined disclosure channel and response-time
expectation MUST exist from the point real code exists in any VIDIMUS
repository.
