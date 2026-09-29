> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 00. Scope and Conformance

## What VIDIMUS is

An interactive, machine-readable document format aiming for PDF-grade
archival fidelity, structured for both human and machine (LLM) reading,
with a declarative reveal/hide interaction layer and a Signed/Static
profile designed to work toward eIDAS/TSE-grade assurance (not itself
a certification or compliance claim). Developed as an open, European-governed
document standard, not a single vendor's file format.

## Two conformance profiles, not two formats

VIDIMUS defines exactly two conformance profiles sharing one container
and content model:

- **Interactive (working name: Vertex, extension `.vtx`).** Mutable,
  editable, the everyday working profile. Supports the reveal/hide
  layer. Does not carry a signature covering the whole document in the
  way the Signed/Static profile does.
- **Signed/Static (working name: Nexa, extension `.nxa`).** Sealed,
  immutable once signed, aiming for eIDAS/TSE-grade legal/financial
  durability (a design goal, not a current certification).
  MUST satisfy the signature-coverage requirements in
  `04-signed-static-profile.md`.

Naming is not finalised - see `07-open-questions.md` for the full
naming/trademark status. This spec uses Vertex/Nexa as working names
throughout.

## Normative language

This and subsequent spec files use MUST / MUST NOT / SHOULD / SHOULD
NOT / MAY per RFC 2119, introduced now to start shaping design work into
checkable requirements. A requirement stated with MUST here is not
necessarily final - it reflects the current state of the design and
remains open to revision until the architecture questions in
`07-open-questions.md` actually close.

## Status of this document

This file is a working draft, not a ratified specification - see the
status note at the top of this file. Promoting it to a stable,
versioned specification is itself tracked as an open item in
`07-open-questions.md`.
