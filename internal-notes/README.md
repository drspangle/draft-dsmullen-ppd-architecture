# Internal Notes

This directory is for working notes only. It is intentionally separate from the
Internet-Draft source, generated artifacts, template files, and CI-managed
publication outputs.

Use this README to decide where draft-adjacent local notes belong before adding
another file here.

Implementation-backed cross-draft implications should not start here. Capture
them first in the gateway repository's standards-facing handoff note
[draft-notes/ppd-internet-draft-notes.md](https://code.cablelabs.com/cablelabs/security-evolution/federated-identity-authentication-and-privacy/user-centric-privacy/habanero-ppd-gateway/-/blob/main/draft-notes/ppd-internet-draft-notes.md),
then copy only the architecture-specific normalized work into this directory
when draft-local editorial notes are still needed.

## Start Here

Use these files as follows:

- `template-operations.md`: build, render, GitHub Pages, and Datatracker
  submission workflow notes.
- `draft-work-log.md`: draft-content TODOs, decisions, open questions, and
  progress notes.
- `directional-changes.md`: concise summary of major draft-direction changes
  suitable for leadership or stakeholder reporting.
- `demo-followups.md`: implementation-facing follow-up items that may need to
  go back into the demo or proof of concept for alignment with the evolving
  draft.
- `security-mechanism-options.md`: working notes for selecting the future
  policy-acknowledgment security mechanism.
- `relationship-to-existing-work.md`: working notes for positioning PPD
  against adjacent work such as DNT/P3P, MUD, and privacy vocabulary or
  policy-expression efforts.
- `taxonomy-protocol-coordination.md`: architecture-side note on how to align
  later with the evolving taxonomy/protocol semantic model

Rules for this directory:

- Do not store passwords, tokens, session cookies, or Datatracker credentials.
- Do not include these files from the draft markdown source.
- Do not link these notes from the public-facing README unless that is a
  deliberate project decision.
- Keep dated status entries so old observations can be identified later.
