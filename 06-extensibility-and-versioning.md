> **Status: working draft, not a ratified specification.** Extracted
> Many points remain explicitly open; see
> `07-open-questions.md`. Normative language (MUST/SHOULD/MAY) follows
> RFC 2119 convention, used here to start shaping prose into checkable
> requirements, not to claim anything below is finalised.

# 06. Extensibility, Versioning, and Target Timeline

## Forward- and backward-compatible extensibility

A reader built against an earlier version of this spec MUST degrade
gracefully on a file using a later version's content type it doesn't
recognise (show what it can, flag what it can't), rather than failing
outright. A file written against an earlier version MUST continue to
open correctly in a later-version-compliant reader
.

## Target timeline

The format is targeted to be
finished and ready for broad use before the start of 2030, and to
remain genuinely advantageous through at least 2050. As of this draft
(September 2026), this project is concept-stage only - this timeline is
a target informing priority, not a claim about current progress.

## A dated durability window, not a floating one

Because of the 2030 target, this format's real "20-year horizon"
(Section 1e) is effectively **2030-2050**, not 2026-2046. This is why
crypto-agility and post-quantum readiness (`04-signed-static-profile.md`)
are launch-day requirements rather than later additions.

## Planned major-edition boundary (~2050)

Rather than assuming a single version must remain unmodified forever,
a deliberate second edition around 2050 is treated as an acceptable,
precedented way to handle changes too large for the extension
mechanism above to absorb gracefully - the same reasoning behind TLS
1.3's clean break from TLS 1.0, and PDF's own move to PDF 2.0 (ISO
32000-2) as a distinct edition.
