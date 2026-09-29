> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 04. Signed/Static Profile (working name: Nexa)

## Purpose

The sealed, immutable-once-signed profile aiming for eIDAS/TSE-grade
legal and financial durability.

## Signature model: Merkle-tree over chunks

Rather than a single flat hash over the whole file (PDF's model), a
Nexa document's signature is computed over a Merkle tree of chunk
hashes. This enables per-chunk
provability - a specific chunk (or its absence, in a redaction) can be
proven consistent with the signed tree without needing the whole
document, directly supporting the redaction-with-proof use case
(Section 4d).

## Closed-world signature coverage (hard requirement)

A compliant verifier MUST reject any byte, chunk, or manifest entry not
covered by the signed Merkle tree, rather than silently rendering
unsigned trailing content. "Signed" means every byte a compliant
reader will ever render is accounted for in the signature - not "some
prefix of the file is." This closes the same vulnerability class as
PDF's 2019 "shadow attack" research (Ruhr-Universität Bochum), where
content appended after a validly signed PDF was still rendered by
permissive viewers. This is a verifier
requirement, not a recommendation.

## Sealed vs. detachable content

At signing time, the signer categorises chunks as sealed (hard to
remove without invalidating the signature, short of a screenshot) or
detachable - modelled loosely on PDF's
`DocMDP` permission mechanism, adapted to the chunk-level granularity
this profile has that PDF does not.

## Lineage model for edits

Editing a signed Nexa document does not silently mutate it. An edit
produces a new, linked document via a `derived-from` pointer back to
the original - lineage is explicit and traceable, never a silent
overwrite.

## Crypto-agility and post-quantum readiness (launch-day requirement)

The signature scheme MUST be
crypto-agile (support algorithm migration) and MUST have a credible
post-quantum path in place by launch, not added later - the real
durability window this profile needs to survive is roughly 2030-2050,
and NIST/BSI/ANSSI-class guidance already treats that window as when
classical-only signatures stop being a safe long-term choice.

**Mechanism decided in outline, 2026-09-28** (`09-crypto-agility-and-
confidentiality.md`): every algorithm is identified from an open,
versioned registry; a document MAY carry multiple simultaneous
signature blocks (enabling hybrid classical + post-quantum signing
during a migration window); a new algorithm can be added to an
already-sealed document later via a **counter-signature** - a new
signature block signing over the original Merkle root, every existing
signature block, and a timestamp, reusing this profile's lineage model
so the addition is traceable, never a silent mutation; and an
independently-maintained algorithm-deprecation list feeds into the
mandated plain-language trust summary (see Section 09
idea 7) as the incident-response path when an algorithm is later judged
compromised. This replaces the earlier vague "e.g. via timestamp
chaining per PAdES-style precedent" phrasing with a concrete mechanism.
**Still needs dedicated outside cryptography and legal review before
being finalised:** the actual signature/hash/post-quantum algorithms
themselves, and the exact counter-signature byte format, remain open.
