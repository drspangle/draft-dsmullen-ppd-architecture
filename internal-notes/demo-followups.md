# Demo And Proof-Of-Concept Follow-Ups

This note tracks changes that may need to go back into the existing demo or
proof of concept so the implementation stays aligned with the evolving draft.

These are not all required immediately. They are earmarked so they can be
scheduled and implemented deliberately.

## Participant-Facing Service Model

- The current gateway implementation uses `/ppd/v1` as its participant-facing
  API path. Treat that as prototype deployment behavior, not as an architecture
  or protocol requirement for a fixed origin-root path.
- The protocol baseline obtains the API mount point from `service_base_uri` in
  metadata retrieved at `GET /.well-known/ppd` on a candidate service origin.
  Operation references are resolved relative to that advertised base URI.
- Avoid presenting the internal repository surface as the baseline client-facing
  contract.
- Keep repository topology and operator-facing APIs clearly separate from
  participant-facing protocol behavior.

## Discovery Automation

- The current demo still relies on manually pointing the device at the gateway.
- Prototype a lower-level discovery mechanism that can provide a candidate PPD
  service origin without requiring manual user configuration during
  association. The protocol's well-known metadata bootstrap then identifies the
  API base URI on that origin.
- Candidate classes to explore later:
  - configured endpoint plus persisted state
  - local DNS name
  - mDNS or DNS-SD
  - DHCP-delivered configuration
  - default-gateway probing
- Preserve the principle that discovery yields candidate origins, not trust by
  itself.

## Metadata Confirmation

- Implement the protocol's lightweight metadata confirmation at
  `GET /.well-known/ppd` on the candidate origin. The response advertises a
  same-origin `service_base_uri`, which may include a deployment-selected path
  prefix and version segment.
- The prototype's existing `/ppd/v1/meta` route may remain during migration,
  but should not be presented as the protocol's required discovery path.
- Ensure this metadata stays small and avoids leaking household policy contents
  or device inventory.

## Policy Authority And Derivation

- The current demo is weaker than the new draft direction because it does not
  yet clearly separate the participant-facing service from the authoritative
  source of policy state.
- Consider whether demo code or documentation should more clearly distinguish:
  - participant-facing PPD service behavior
  - policy storage
  - effective-policy derivation logic
- Leave room for more complex effective-policy calculations to move elsewhere in
  the future without changing the baseline client-facing contract.
- If the participant-facing service and authoritative policy logic split apart,
  preserve an unambiguous binding between:
  - the participant-facing service endpoint
  - the policy-instance identifier presented to the participant
  - the freshness or renewal metadata associated with that policy instance
  - the underlying authoritative policy state

## Association Lifecycle

- The demo should continue moving toward explicit lifecycle handling for:
  - first association
  - bounded association freshness and periodic renewal
  - stale association when renewal does not occur in time
  - reassociation when current association can no longer be confirmed
  - participant state changes that affect effective policy
  - loss of state requiring resynchronization
- The participant-facing service should be treated as the source of truth for
  whether association is current, stale, or in a needs-reassociation state.
- Dashboard or management views should make it clear when a participant is:
  - currently associated
  - stale because renewal expired
  - in needs-reassociation because policy or relevant participant state changed
- Stale devices should not count as current in any summary or operator view.
- Stale association should remain distinct from policy-change reassociation in
  local status and diagnostics.
- Clients likely need to track:
  - last successful renewal
  - next refresh or renewal deadline
  - whether the last known state is current, stale, or needs reassociation
- Clients likely need to renew periodically even when policy content is
  unchanged.

## Local Management Versus Interoperable Protocol

- Keep dashboard, service-health, and operator-only views clearly labeled as
  local management surfaces.
- Avoid letting demo-only controls or diagnostics imply baseline protocol
  requirements in public materials.

## Security Roadmap

- The draft still leaves room for a staged security story.
- The demo should keep a path from simple compatibility mode toward stronger
  authenticated participation, likely including:
  - better service authentication
  - a clearer separation between discovery of candidate endpoints and trust in
    the selected endpoint
  - participant identity binding
  - a concrete protected-receipt mechanism for acknowledgments, such as a
    signature or equivalent integrity-protected record that can support later
    review
  - stronger acknowledgment integrity and replay protection
  - clearer distinction between local-device and backend-on-behalf-of-device
    acknowledgment
