# Runtime Conflict Management Direction

This note records the current intended architectural story for participant
runtime conflict after policy acknowledgment.

It is separate from the baseline protocol note because the architectural point
is about system meaning and management boundaries, not only object shapes.

## 2026-05-29

### Current Direction

The intended architecture is:

- a participant may be currently associated because it received and
  acknowledged the applicable policy instance;
- that same participant may also report, or be observed as having, runtime
  behavior that conflicts with that acknowledged policy; and
- the household or deployment may then choose from optional remediation paths,
  but the architecture does not require one universal response.

### Architectural Boundary

The architecture draft already says that:

- acknowledgment and recordkeeping do not prove compliance;
- enforcement is out of scope; and
- future work may define optional participant status reporting and
  deconfliction strategies.

What should be made more explicit is that runtime conflict is compatible with
continued current association.

That means:

- current association and runtime conflict are not mutually exclusive;
- runtime conflict should be understood as an informational or management
  signal, not as automatic disassociation; and
- deployment-local responses such as notification, segregation, blocking, or
  manual disassociation remain optional choices rather than architectural
  requirements.

### Draft Follow-Up To Capture

The architecture draft should make the following point explicit in its future
work or management discussion:

"A participant can remain currently associated for signaling and recordkeeping
purposes while separately reporting runtime behavior that conflicts with the
acknowledged policy instance. Detection, presentation, and remediation of that
runtime conflict are optional deployment behaviors and do not, by themselves,
redefine baseline association."
