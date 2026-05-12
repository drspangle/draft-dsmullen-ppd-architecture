# Relationship to Existing Work

Date: 2026-05-12

## Decision

- Put the substantive comparison to adjacent work in the architecture draft.
- Keep the protocol draft to a short cross-reference back to that discussion.

## Why

The architecture draft is the conceptual starting point for learning PPD.
It already carries the broader problem framing and the existing DNT/P3P discussion.
That makes it the right place to explain where PPD is intentionally borrowing from, and where it is solving a different problem.

## Comparisons To Keep

- `DNT` / `P3P`: earlier privacy-preference signaling efforts, but not a fit for participant-specific home-network lifecycle handling or household-side policy-instance recordkeeping.
- `MUD`: the closest device-to-home-network signaling precedent, but focused on manufacturer-defined network intent rather than household-defined privacy-policy signaling.
- `DPV` / `ODRL`: useful semantic substrate for vocabulary and policy-expression, but closer to the content layer than to the participant-facing signaling architecture.

## Drafting Position

The architecture draft should carry a concise relationship-to-existing-work section.
The protocol draft should reference that section and stay focused on the participant-facing wire behavior.
