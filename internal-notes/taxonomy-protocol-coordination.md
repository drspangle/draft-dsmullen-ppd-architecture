# Taxonomy and Protocol Coordination

This note records the current architecture-side coordination view for the
taxonomy and protocol drafts.

## Current Assessment

The architecture draft is not currently in direct semantic conflict with the
newer taxonomy/protocol direction under discussion.

The architecture already supports:

- governance of privacy-relevant data flows;
- optional participant declarations distinct from the minimal retrieval and
  acknowledgment path;
- diagnostic-only comparison outcomes at the participant-facing boundary; and
- separation between architecture, taxonomy, and protocol concerns.

## Likely Future Alignment Edits

Once the taxonomy and protocol drafts settle their next round of semantic work,
the architecture draft will likely need only a lighter wording-alignment pass.

The most likely future alignment items are:

- describe the taxonomy as defining the semantic dimensions of atomic
  privacy-relevant dataflows rather than only a broad vocabulary and category
  list;
- make clear that household/device comparison is grounded in those atomic
  dataflow structures, without turning this architecture draft into the place
  where matching or conflict procedure is standardized; and
- align terminology if the protocol keeps the wire object name `constraints`
  while the taxonomy work treats the underlying concept as qualifiers.

## Current Rule

Do not let the architecture draft race ahead of the taxonomy/protocol semantic
work.

The architecture draft should:

- keep the high-level framing coherent;
- avoid overcommitting to immature qualifier details; and
- absorb terminology cleanup only after the taxonomy/protocol layer is ready.
