> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 03. Interactive Profile (working name: Vertex)

## Purpose

The mutable, everyday working profile.
Supports ordinary editing and the reveal/hide interaction layer. Does
not carry the whole-document signature coverage the Signed/Static
profile requires.

## The reveal/hide mechanism

The Interactive profile's core interactivity is a **declarative**
manifest describing which chunks are shown or hidden under which named
conditions. This is not a scripting
mechanism - see the no-execution-model rule below, which is the single
most load-bearing constraint on this profile's design.

## No execution model (hard constraint, inherited from Section 3a)

A compliant Vertex reader MUST NOT provide any general-purpose
execution environment to document content: no embedded scripting
language, no macro system, no embedded WASM module, no expression
language with enough power to become a general one. This is a
permanent specification constraint, not a reader-configurable setting
. Full rationale and the PDF-JavaScript/
Office-macro precedent this avoids is in `05-security-requirements.md`.

## Context-adaptivity (provisional extension point, not yet specified)

A related idea under consideration proposes extending reveal/hide
from "person clicks to reveal" to "reader-evaluated declarative
conditions" (viewport size, accessibility preference, etc.), modelled
on the relationship CSS media queries have to a browser - the document
states a condition, the reader's own trusted logic evaluates it, and
nothing is ever executed. This is a hypothesis under the same
no-execution-model constraint above, not a specified mechanism.

## Collaborative editing (provisional, tracked for later)

Track-changes-as-reveal-regions is named as
an authoring scenario; Section 3b (idea D) speculates about CRDT-based,
local-first collaborative editing as a future direction. Neither is
specified here.
