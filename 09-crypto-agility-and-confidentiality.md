> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 09. Cryptographic Agility and Confidentiality

Answers a question raised directly (2026-09-28): can a new signature
mechanism, or encryption, be added to a document *after the fact* -
because of a technical advance, or because an existing algorithm is
compromised - without a format break? Yes, and this file specifies the
mechanism. It also surfaces a gap this question exposed: signing
(integrity) and encryption (confidentiality) had been conflated with
the declarative reveal/hide mechanism (`03-interactive-profile.md`),
which is neither - see [Confidentiality](#confidentiality-a-separate-
capability-from-reveal-hide) below.

**Scope note:** this file specifies the *mechanism* - how an algorithm
is identified, how a new one is added, how an old one is retired. It
does not choose the actual cryptographic primitives (which post-quantum
scheme, which encryption cipher) - that remains Phase 0 item 3,
correctly gated on dedicated outside cryptography review (see
further dedicated review). A mechanism that lets the right
experts plug in an answer later is exactly what "don't narrow the
capability set early" means applied to cryptography.

## The open cryptographic-algorithm registry

Every cryptographic operation in a VIDIMUS document - a signature, a
hash, an encryption operation - is identified by an explicit algorithm
identifier from an **open, versioned registry**, the same structure as
the chunk-type registry (`02-content-model-and-chunks.md`,
the same registry pattern used for chunk types, and for the same reason:
a new entry (a post-quantum signature scheme, a replacement hash
function, a new cipher) is a registry addition, not a format version
bump. No cryptographic operation anywhere in the format is ever
hard-coded to one fixed algorithm.

## Signatures: multiple, hybrid, and added-later

**Multiple simultaneous signature blocks.** A Nexa document's manifest
MAY carry more than one `signature-block` chunk over the same Merkle
root, each independently identified by its own algorithm from the
registry above. This is what makes **hybrid signing** possible during
an algorithm-migration window - e.g. a classical (Ed25519-class) and a
post-quantum (ML-DSA-class) signature applied at the same time,
already the recommended NIST/IETF transition pattern for exactly this
situation, not a VIDIMUS-specific invention.

**Counter-signatures: adding a new algorithm to a document already
sealed.** This is the mechanism for "nachschieben" - retrofitting a
stronger algorithm onto a document signed years earlier, without
breaking its seal or its history:

- A **counter-signature** is a new `signature-block` whose signed
  content is defined as *the original Merkle root plus every existing
  signature block plus a timestamp* - it wraps and re-attests the
  entire prior signed state under a new algorithm, rather than
  replacing anything.
- This reuses the existing lineage model
  (`04-signed-static-profile.md`, Section 04, "Lineage model")
  exactly: a counter-signature is a traceable addition, never a silent
  mutation, the same discipline already applied to content edits - but
  distinguished from a content edit because nothing about the visible
  content changes, only the trust layer strengthens.
- The closed-world signature-coverage rule (`04-signed-static-
  profile.md`) extends naturally: a counter-signature's own "closed
  world" includes the original signature block by construction, so a
  document can never end up with an unsigned or ambiguously-signed gap
  between the original seal and a later counter-signature.
- Applying a counter-signature requires the same authority the
  original signer had (or a defined delegate) - who exactly is allowed
  to counter-sign a given document is a policy question, not a format
  one, and is out of this file's scope.

## Algorithm deprecation: the incident-response path

A signature (or encryption) algorithm can be judged compromised after
a document using it already exists - the incident-response
case. This is handled without a format change:

- A **deprecated-algorithm list** is maintained independently of the
  format itself (the same relationship a browser's list of distrusted
  TLS cipher suites has to the TLS specification, or a CRL has to the
  X.509 format) - updatable on its own schedule, not tied to a VIDIMUS
  spec version.
- A compliant verifier MUST check a document's signature algorithm(s)
  against the current deprecated-algorithm list at verification time.
  If a document's *only* remaining valid signature uses a deprecated
  algorithm, the verifier MUST reflect a downgraded trust status in the
  mandated plain-language trust summary (see Section 09
  idea 7, e.g. "sealed under an algorithm since deprecated -
  re-signature recommended") rather than either silently trusting it or
  silently rejecting it.
- If a counter-signature under a non-deprecated algorithm is present,
  the document's trust status reflects that instead - this is precisely
  why counter-signing, not just "sign again from scratch," is the
  retrofit mechanism: it gives a document a documented recovery path
  from an algorithm incident.

## Confidentiality: a separate capability from reveal/hide

**Gap this question surfaced:** nothing in this specification currently
provides actual cryptographic confidentiality. The Interactive
profile's reveal/hide mechanism (`03-interactive-profile.md`) is
**declarative and reader-enforced** - hidden content is still physically
present in the file, and a compliant reader simply chooses not to
render it. That's the right design for progressive disclosure (an
accordion, a "click to reveal" section) but it is not encryption: a
non-compliant reader or a raw file inspector can still read
"hidden" content. This was never claimed as confidentiality elsewhere
in this project's documents, but the question of encryption makes the
distinction worth stating explicitly so it's never assumed by mistake
later.

**Proposed (not yet decided): an optional per-chunk encryption
capability**, fitting the existing capability-module tier system
(`08-capability-modules-and-feature-roadmap.md`) as a new module rather
than a baseline requirement:

- Any chunk MAY carry `encrypted: true`, an `enc_algorithm` (from the
  same open registry above), and a recipient/key-wrapping structure
  (multi-recipient key-wrapping, the same shape `age`'s recipient-
  stanza design or PGP's multi-recipient encryption already use - not
  a new invented primitive).
- A reader without a decryption key for a given chunk MUST use that
  chunk's `fallback` field (`01-container-and-manifest.md`'s existing
  capability-dependency mechanism) exactly as it would for a missing
  capability module - encryption becomes a specific case of the
  already-specified `requires_capability`/`fallback` pattern, not a
  parallel mechanism.
- This connects directly to the wallet-gated confidentiality idea (Section 09, EUDI
  Wallet-native reveal conditions): a wallet-gated *reveal* condition
  only ever hides content from a compliant reader's UI; genuine
  wallet-gated *confidentiality* would need this encryption capability
  underneath it, with the wallet credential used to release a
  decryption key, not just to evaluate a declarative condition. Whether
  wallet-gated content should require actual encryption (not just
  declarative hiding) given its typical sensitivity (age, citizenship,
  role attributes) is a real design question this file surfaces rather
  than resolves.

**Not decided here:** whether this capability ships in v0 at all, or is
correctly a later module once real demand for cryptographic (not just
declarative) confidentiality is confirmed - tracked in
`07-open-questions.md`.

## What this file deliberately does not do

It doesn't pick algorithms (still pending dedicated cryptography review), doesn't
decide whether encryption ships in v0, and doesn't specify the exact
recipient/key-wrapping byte format - it establishes that the mechanism
for all three (new signature algorithms, algorithm retirement, and
optional encryption) is a single, consistent, already-extensible
pattern: an open algorithm registry plus the capability-module
graceful-fallback rule this specification already commits to
elsewhere.
