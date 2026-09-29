# VIDIMUS - Specification & Design

## Contents

- [Why the specification is published on its own](#why-the-specification-is-published-on-its-own-ahead-of-any-implementation)
- [What's in here](#whats-in-here)
- [Contributing](#contributing)
- [Repository map](#repository-map)
- [Naming](#naming)
- [License](#license)

**Working title**, published now under this name while a permanent
public name clears trademark review (see [Naming](#naming) below) -
the specification and everything built against it will carry forward
under the final name without disruption.

The specification behind **VIDIMUS**: an open, interactive,
machine-readable document format designed for both everyday editable
documents and legally-sealed, signature-verifiable records, built to
remain a genuine, secure archival standard for decades, not years. Two
conformance profiles from one specification - **Vertex** (interactive,
mutable, everyday use) and **Nexa** (signed, sealed, legal/financial
grade) - the same relationship PDF has to PDF/A.

This repository defines the specification: the container and chunk
manifest model, the content model, both conformance profiles, security
requirements, and the extensibility and capability-module model every
implementation conforms to. Reference implementations - a core parser/
writer library and format adapters - are developed against this
specification and published separately as they reach a usable state.

## Why the specification is published on its own, ahead of any implementation

A document format meant to remain a genuine, implementable standard for
decades needs to be able to outlive, and be implemented independently
of, any one codebase - the same reason formats like EPUB and protocols
like Matrix and ActivityPub keep their specification in its own
repository, so no implementation is structurally privileged over a
second or later one. Publishing the specification and its design review
openly, before a reference implementation exists, is standard practice
for a format meant to be implemented by more than one party - the same
model IETF and W3C specifications, and Rust's own pre-1.0 RFC process,
use to gather implementation-shaping feedback before any code commits
the design to a particular shape. `tessera-spec` (our infrastructure-
monitoring project) follows the same model.

## What's in here

1. **`00-scope-and-conformance.md`** - what VIDIMUS is, the two
   conformance profiles, and the normative-language convention.
2. **`01-container-and-manifest.md`** - the container format, the
   chunk manifest/index, and container-level hardening requirements.
3. **`02-content-model-and-chunks.md`** - chunk types, the media-type
   model, and accessibility metadata.
4. **`03-interactive-profile.md`** - the Vertex profile: mutability,
   the reveal/hide mechanism, and the no-execution-model guarantee.
5. **`04-signed-static-profile.md`** - the Nexa profile: the
   Merkle-tree signature model, closed-world signature coverage, and
   the lineage model for edits to a signed document.
6. **`05-security-requirements.md`** - the consolidated security
   requirements every implementation must meet.
7. **`06-extensibility-and-versioning.md`** - forward/backward
   compatibility and the format's long-term versioning model.
8. **`07-open-questions.md`** - the current design-review agenda.
9. **`08-capability-modules-and-feature-roadmap.md`** - the capability-
   module model: which features are baseline (every implementation
   supports them) versus optional modules (declared, with a defined
   fallback for implementations that don't), by conformance profile.
10. **`09-crypto-agility-and-confidentiality.md`** - the open
    cryptographic-algorithm registry; hybrid and counter-signatures for
    adding a new algorithm after a document is already sealed;
    algorithm deprecation as an incident-response path; and the
    proposed (not yet decided) optional per-chunk encryption capability.

## Contributing

Contributions are welcome on the specification and, once published, on
the reference implementations:

- **Specification and design discussion** happens here - propose a
  change to the container model, comment on a security requirement,
  weigh in on an open design-review item in `07-open-questions.md`.
- **Implementation contributions** will land in the reference
  implementation repositories once they are public (see
  [Repository map](#repository-map)).

Every issue and pull request is read directly by the team; changes
worth adopting are merged deliberately. See
[CONTRIBUTING.md](CONTRIBUTING.md) for the mechanics, the
[Code of Conduct](CODE_OF_CONDUCT.md) that applies to all of it, and the
[AI Policy](AI_POLICY.md) before using an AI tool to help with a
contribution.

## Repository map

| Repository | Purpose | Status |
|---|---|---|
| **vidimus-spec** (this one) | Specification: container model, content model, both conformance profiles, security requirements | Published |
| Core library (name pending) | Reference implementation: parser/writer | In development |
| Format adapters (name pending) | Import/export adapters (LaTeX first) | Planned |

## Naming

Published under the working title **VIDIMUS** while **Vertex**
(interactive profile) and **Nexa** (signed/static profile) - the
intended public names - complete trademark and domain clearance. The
specification's technical content is unaffected by the outcome; only
labels change.

## License

The specification text in this repository is licensed under
[**CC BY 4.0**](LICENSE) (Creative Commons Attribution 4.0 International) -
anyone can read, implement against, and redistribute it, including
commercially, as long as Favus is credited. This is the same model
IETF/W3C specifications and formats like EPUB use: the specification
itself stays freely implementable by any party. The reference
implementations (`vidimus-core`, `vidimus-adapters`) are licensed
separately under AGPLv3 - see their own repositories.

---

Built by Favus.

*Céad míle fáilte* - "a hundred thousand welcomes." A specification
means something once other people read it, argue with it, and
implement against it: fáilte romhat.
