# Contributing to the VIDIMUS specification

Thanks for taking an interest in the VIDIMUS specification. This document
covers how to propose a change, what a good issue or pull request looks
like, and how design-review items get resolved.

## Before opening anything

Read the [Code of Conduct](CODE_OF_CONDUCT.md) - it applies to every
issue, pull request, and discussion in this repository.

Skim `07-open-questions.md` first. A number of design questions are
already tracked there with context on what's been considered; if your
proposal touches one of them, reply on that thread instead of opening a
new one so the discussion stays in one place.

## What a good contribution looks like

- **Propose a change to the container or content model** - open an issue
  describing the concrete problem it solves (a real interoperability gap,
  a security requirement it doesn't currently cover, an ambiguity an
  implementer hit), not just a preference.
- **Comment on a security requirement** - `05-security-requirements.md`
  is the consolidated list every implementation must meet; if you find a
  requirement that's ambiguous, insufficient, or missing, say so with the
  attack or failure case in mind.
- **Weigh in on an open design-review item** - `07-open-questions.md` is
  the current agenda. These aren't starter tasks; resolving one is
  meaningful, substantive work on the specification itself.
- **Report an implementer-facing problem** - if you're building against
  this spec (in the reference implementations or independently) and hit
  something underspecified or contradictory, that's exactly the kind of
  issue this repository wants.

## What doesn't belong here

Implementation bugs, feature requests for the reference parser/writer, or
adapter requests belong in the implementation repositories once they're
public (see the [Repository map](README.md#repository-map) in the main
README) - this repository is the specification only.

## Pull requests

- Keep a pull request scoped to one change to one document where
  possible; a PR that touches the container model, the security
  requirements, and the versioning section at once is harder to review
  and more likely to stall.
- Reference the relevant open-questions item or issue in the PR
  description.
- Normative language (MUST/SHOULD/MAY) follows the convention defined in
  `00-scope-and-conformance.md` - keep new or changed requirements
  consistent with it.

## How contributions get resolved

Every issue and pull request is read directly by the team. Design-review
items get resolved deliberately, not on a fixed schedule - a specification
meant to remain implementable for decades is worth getting right over
getting merged quickly. You'll get a substantive response, even if the
answer is "not yet" or "no, and here's why."
