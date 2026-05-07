# Draft Work Log

This file tracks draft-content work separately from the functional repository
contents. It is not part of the rendered Internet-Draft.

## 2026-05-05

- Added `internal-notes/` as the place for operational and draft-progress
  notes.
- Current draft source: `draft-dsmullen-ppd-architecture.md`.
- Current submitted tags present locally: `draft-dsmullen-ppd-architecture-00`
  and `draft-dsmullen-ppd-architecture-04`.
- Tested local environmental dependencies without running any Datatracker
  submission command. Local rendering prerequisites are not currently
  satisfied on native PowerShell because `make`, Python, Ruby, and WSL Ubuntu
  are missing.
- Attempted to start WSL setup with `wsl --install -d Ubuntu`; Windows still
  reported that WSL is not installed. This likely needs machine-level setup
  outside this repo/session.
- Enabled the Windows Subsystem for Linux and Virtual Machine Platform optional
  features from elevated PowerShell.
- Installed WSL package version 2.6.3 with `wsl.exe --install -d Ubuntu
  --no-launch`. Windows reported that changes will not be effective until the
  system is rebooted.
- Current blocker: reboot Windows, then install/initialize Ubuntu and run the
  apt dependency installation.
- Prepared post-reboot scripts under `internal-notes/scripts/` for checking WSL
  state, installing Ubuntu package dependencies, checking the template toolchain,
  and rendering the local editor's copy without Datatracker submission.

## 2026-05-06

- After reboot, confirmed WSL version `2.6.3.0` and default WSL version `2`.
- Installed the Ubuntu WSL distribution and confirmed it runs as WSL2.
- Installed Ubuntu package dependencies needed for local rendering.
- Confirmed `internal-notes/scripts/check-template-env.sh` passes.
- Ran local rendering only. No `make upload`, `make publish`, `make next`, git
  tag, workflow dispatch, or Datatracker API command was run.
- First render failed at `kramdown-rfc` because Bundler used a relative local
  gem path incorrectly from the Windows-mounted WSL repo path.
- Retried with an absolute Ubuntu `BUNDLE_PATH` and rendered successfully.
- Updated `internal-notes/scripts/render-local-editor-copy.sh` to export the
  absolute Bundler path automatically.
- Current local review artifacts:
  `draft-dsmullen-ppd-architecture.html` and
  `draft-dsmullen-ppd-architecture.txt`.
- Collapsed the short `Privacy`, `Transparency`, and `User Control`
  subsections into the terminology list and moved the RFC6973/RFC8280 framing
  into the Introduction so the definitions section better matches IETF style.
- Reframed the participant-facing architecture around a `PPD service endpoint`
  rather than around direct repository discovery.
- Introduced `policy authority` and `effective policy derivation` as explicit
  architectural concepts so the source of truth and policy-computation function
  can be distinct from the participant-facing service.
- Replaced the previous product-style vision and use cases with operational
  scenarios focused on discovery, association, reassociation, participant state
  change, and mixed PPD/non-PPD visibility.
- Changed reassociation wording to be condition-based: it now occurs when
  current association can no longer be confirmed, rather than being tied to any
  one network event.
- Strengthened the lifecycle model so current association now includes
  freshness, can expire without policy-content change, and distinguishes stale
  association from broader needs-reassociation states.
- Added a dedicated architecture subsection that explains association state and
  freshness in one place, including the role of the PPD service endpoint as the
  source of truth for whether a participant is current, stale, or in needs
  reassociation.
- Added explicit discovery/trust language so the draft now treats discovery as
  yielding candidate PPD service endpoints, with trust in a selected endpoint
  established separately by the applicable protocol and security mechanisms.
- Added an explicit policy-authority boundary model so the draft now says the
  participant-facing contract ends at the PPD service endpoint even when policy
  storage or effective-policy derivation live elsewhere.
- Tightened the introduction and goals so the front half of the draft reads
  more like an IETF architecture document and less like product or advocacy
  framing.
- Made the reciprocal-acknowledgment point more explicit so the draft now says
  household-controlled acknowledgment records can support later accountability
  or review without asserting compliance or defining enforcement behavior.
- Cleaned up the architecture draft for IETF-style review by adding proper
  informative references, removing the stream metadata mismatch with the
  current Datatracker record, and tightening sections that had become too
  speculative or product-oriented.
- Updated the architecture draft's companion-protocol wording so it now refers
  coherently to the new protocol-draft work under the expected name
  `draft-dsmullen-ppd-protocol`.
- Removed explicit prototype-derived wording from the JSON baseline discussion
  so the draft no longer cites prototype experience directly.
- Updated future-work and security text to align with the service-endpoint and
  policy-authority model.
- Added `directional-changes.md` for concise leadership reporting.
- Added `demo-followups.md` to earmark implementation changes that may need to
  go back into the demo or proof of concept.

## Open Checks

- Confirm intended IETF venue metadata before the next formal submission.
- Add a real acknowledgments section before publication if there are people,
  reviewers, prototype contributors, or organizations that should be credited.
  The placeholder `TODO acknowledge.` was removed from the draft source so the
  Internet-Draft does not carry placeholder text.
- Decide how much of the bootstrap discovery story should remain in this
  architecture draft versus being deferred to the companion protocol
  specification.
- Decide whether the introduction still needs a further tone pass to remove any
  remaining non-IETF-style advocacy language.
- Decide how much effective-policy derivation detail belongs in the
  architecture draft before it starts to pre-empt companion protocol work.

## Completed Checks

- Replaced placeholder YAML keywords with PPD-related terms.
- Changed `consensus` from `true` to `false` for the current individual draft
  state.
- Removed CBOR/RFC 8949 as a baseline encoding suggestion. Current architecture
  text treats JSON as the practical starting point and reserves more compact
  encodings for deployment profiles that demonstrate a need.
- Completed an initial tone pass to keep the draft focused on PPD as a
  signaling and acknowledgment architecture, not a mechanism that modifies or
  constrains device behavior.
- Replaced `accountability` terminology with `recordkeeping` to keep the
  architecture neutral and manufacturer-friendly.

## Security Discussion Notes

- The draft should preserve a strong record that a particular participant
  acknowledged a particular policy instance from the home network.
- The architecture should avoid selecting a concrete security mechanism before
  considering constrained devices, backend-on-behalf-of-device acknowledgment,
  provisioning constraints, and deployability.
- Candidate mechanism families to discuss later include device certificates or
  manufacturer identities, local onboarding-bound keys, signed acknowledgments,
  mutually authenticated TLS, token-based approaches, and staged compatibility
  modes. These are discussion candidates, not draft commitments.
- Future draft work likely needs to choose a specific security mechanism or
  small set of profiled mechanisms for policy acknowledgment. A purely generic
  statement that acknowledgments need protection is probably not specific enough
  for the eventual protocol architecture.
- Added `security-mechanism-options.md` to track candidate mechanism families,
  required properties, and the open decision without committing the current
  architecture draft to one mechanism prematurely.
