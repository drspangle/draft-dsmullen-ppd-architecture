# Security Mechanism Options

This note tracks future work on choosing a concrete mechanism, or a small set of
profiled mechanisms, for secure PPD policy acknowledgment.

The draft should not remain permanently generic here. The architecture needs to
support a strong record that a particular PPD participant acknowledged a
particular policy instance from the home network, while still remaining
implementable by constrained home-network and IoT devices.

## Required Security Properties

Any selected mechanism needs to provide:

- participant authentication sufficient to bind an acknowledgment to the device
  or backend service that made it
- policy-instance integrity so the acknowledged policy can be identified
  unambiguously
- freshness or sequencing so an old acknowledgment cannot be replayed as
  evidence of current association
- a record format that can distinguish local-device acknowledgment from
  backend-service acknowledgment on behalf of a device
- a way to retain or export the acknowledgment record without exposing more
  household metadata than necessary

## Candidate Mechanism Families

### Local Onboarding-Bound Key

The device establishes a local key during onboarding and uses it to authenticate
future policy retrieval and acknowledgment exchanges.

Pros:

- strong fit for a home-network scoped protocol
- can be simpler than a global PKI dependency
- supports policy-instance signatures or MACs

Open issues:

- key provisioning and reset behavior
- device replacement and factory-reset flows
- whether backend services can participate cleanly

### Mutually Authenticated TLS

The device and PPD service authenticate each other at the transport layer.

Pros:

- well-understood security model
- can bind transport identity to acknowledgment exchanges
- good fit for HTTP-based protocol work

Open issues:

- certificate provisioning for low-cost devices
- local trust-anchor management in the home
- backend-on-behalf-of-device attribution

### Signed Acknowledgment Object

The acknowledgment payload is signed by the participant or by a backend service
authorized to act for the device.

Pros:

- creates a portable record independent of transport logs
- can bind participant identity, policy hash, policy version, and timestamp or
  sequence
- clear fit for recordkeeping

Open issues:

- signing-key provisioning
- device-side crypto cost
- format and canonicalization rules
- how the home network verifies backend authority

### Token Or Proof-of-Possession Model

The device obtains a token during onboarding or association and uses a proof of
possession for acknowledgment.

Pros:

- can support constrained compatibility profiles
- can separate authorization from long-lived device certificates
- may work for backend-mediated device ecosystems

Open issues:

- token issuer role
- token lifetime and replay handling
- local versus cloud trust boundary
- complexity relative to prototype goals

### Manufacturer Or Device-Certificate Identity

The device presents a manufacturer-provisioned identity or certificate chain.

Pros:

- strong device identity when available
- may align with existing device manufacturing flows
- useful for higher-assurance deployments

Open issues:

- not all devices have usable manufacturer identities
- privacy risk from stable global identifiers
- manufacturer trust and revocation handling
- may be too heavy as a baseline requirement

### Staged Compatibility Mode

The protocol defines a lower-assurance compatibility mode for devices that
cannot support the stronger mechanism, while making the limitations explicit.

Pros:

- lowers adoption barrier
- useful for legacy or constrained devices
- allows the architecture to distinguish receipt records by assurance level

Open issues:

- risk of becoming the de facto default
- how to present assurance differences without implying enforcement
- what minimum security floor is still acceptable

## Drafting Direction

Likely architecture direction:

- identify the required security properties in the architecture draft
- avoid choosing the mechanism in the architecture draft until discovery and
  trust-boundary decisions are clearer
- choose or profile the concrete mechanism in the protocol specification
- make assurance level explicit in the acknowledgment record if multiple
  profiles are allowed

## Open Decision

The next substantive security decision is whether the baseline protocol should
use:

- a local onboarding-bound key with signed acknowledgment records
- mutually authenticated TLS plus an application-level acknowledgment record
- a signed acknowledgment object independent of the transport
- a two-profile model with a stronger baseline and a clearly labeled
  compatibility mode
