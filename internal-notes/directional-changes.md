# Directional Changes Summary

This note captures the major architectural direction changes that have emerged
from draft review and discussion. It is intended to be short enough to reuse in
leadership or stakeholder updates.

## 2026-05-06

- The participant-facing architectural contract now centers on a `PPD service
  endpoint` rather than on direct discovery of a repository.
- A gateway-hosted PPD service is now treated as a common home deployment model
  rather than as the only deployment model.
- The architecture now uses `policy authority` as the stronger term for the
  source of truth behind household policy state and any resulting effective
  policy calculations.
- The draft now explicitly leaves room for the policy authority to be local or
  remote, while keeping that topology out of the baseline participant-facing
  contract.
- The architecture now recognizes `effective policy derivation` as a logical
  function performed by or on behalf of the policy authority.
- Reassociation is now framed as a condition-based lifecycle event: it occurs
  when current association can no longer be confirmed, rather than being tied
  to any one network or transport event.
- Current association is no longer treated as indefinite. The draft now treats
  association freshness as part of current association and leaves room for
  bounded renewal intervals or deadlines enforced by the participant-facing
  service.
- The draft now distinguishes stale association from needs-reassociation
  states caused by policy or participant-state changes.
- The previous consumer-product-style vision and use-case text has been replaced
  with operational scenarios focused on discovery, association, reassociation,
  and mixed PPD/non-PPD home-network visibility.
- Future-work and security sections now align with the service-endpoint and
  policy-authority model instead of the earlier repository-centric wording.

## Why These Changes Matter

- They align the draft more closely with the concerns of IETF IOTOPS, where
  onboarding, lifecycle behavior, operational visibility, and trust boundaries
  are more important than product or marketing framing.
- They preserve compatibility with the current gateway-oriented prototype while
  creating architectural space for more distributed or cloud-backed policy
  deployments later.
- They make it clearer which parts of the system are interoperable protocol
  surfaces and which parts are deployment-local implementation details.
