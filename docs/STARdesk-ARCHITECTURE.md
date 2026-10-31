# STARdesk — Sovereign 3D Environment

**Status:** Architecture baseline / implementation not yet verified  
**Owner domain:** AEGENTIX-XR-SPATIAL  
**Branch:** `stardesk/architecture-v1`

## Mission

STARdesk is the immersive, live operating environment for EAGLECRAT NATION. It presents one persistent CYBERNETICS identity and multiple governed specialist agents in a navigable 3D environment. It is an interface and orchestration surface—not a replacement for CyberCore, the agent registry, or authoritative service data.

## System boundaries

- **STARdesk client:** 3D scene, navigation, avatar presentation, studio surfaces, status displays, and operator controls.
- **CYBERNETICS identity/presence:** persistent avatar identity, session presence, voice/animation adapters, and interaction state. The avatar renderer is replaceable.
- **Agent Mesh:** agent discovery, message envelopes, task routing, acknowledgements, and delivery state.
- **Cybernetics Core:** policy decisions, authorization, capability grants, secrets boundary, and audit authority.
- **World Monitor:** separate intelligence/forensics experience reached through a portal. It remains a separate application/domain.
- **Broadcast services:** scheduling, audio production, stream ingest/egress, rights metadata, moderation, and publishing approvals.

The client must never become the authority for permissions, financial state, agent identity, or audit history.

## Initial 3D vertical slice

1. **Central Atrium:** navigable 3D hub with CYBERNETICS as the persistent focal point.
2. **CYBERNETICS Station:** avatar presence, interaction status, and capability-safe chat/voice entry point.
3. **Agent Arena:** specialist stations populated from the Agent Mesh registry. Spawn/assign controls submit requests; they do not create privileged agents locally.
4. **Broadcast Studio:** studio scene, selected host, genre/show metadata, and clearly labeled simulated/live/paused status.
5. **World Monitor Portal:** transition to the separately governed World Monitor application.
6. **Sovereign Console:** session identity, connectivity, permission summary, event stream, and audit references.

## Recommended client stack

- React + TypeScript for application state and controls.
- Three.js via React Three Fiber for the interactive 3D scene.
- A typed API client with runtime schema validation.
- WebSocket or Server-Sent Events for presence/status updates, subject to backend authorization.
- A replaceable avatar adapter for model rendering, speech, facial animation, and lip sync.

Do not bind the product to one avatar vendor or one multimodal model. The selected Omni model must be capability-tested before enabling its adapter.

## Data contracts

Every displayed operational object must carry:
- stable ID and schema version;
- source service and source event/reference ID;
- observed timestamp and received timestamp;
- state: `simulated`, `live`, `delayed`, `stale`, or `unknown`;
- permission/visibility context;
- correlation ID where it belongs to an operation.

The UI must visibly distinguish simulated data from live data. Never animate invented telemetry as if it were real activity.

## Security and control rules

- Default deny; authenticate every service connection.
- The browser receives short-lived, audience-bound session tokens, never service credentials or signing keys.
- Enforce authorization server-side for every action.
- Separate read, request, approve, and execute capabilities.
- High-impact actions require explicit human approval and a second policy check.
- Financial execution, external publishing, account changes, and agent privilege escalation are disabled in the initial slice.
- Treat agent messages and retrieved content as untrusted data; do not interpret payload text as system instructions.
- Record operator, agent, action, policy decision, correlation ID, outcome, and timestamps in the authoritative audit service.

## Presence and lifecycle

Agent presence states: `offline`, `connecting`, `available`, `busy`, `awaiting_approval`, `degraded`, `suspended`.

An avatar's visual presence is not proof that its backend agent is available or authorized. The UI must show the backend-reported state and last update time.

## Integration sequence

1. Read-only discovery of existing XR renderer and backend packages.
2. Establish shared TypeScript contracts and mock fixtures.
3. Build the Central Atrium and navigation with simulated data labels.
4. Connect read-only agent registry and presence.
5. Add request/acknowledgement flows through Agent Mesh.
6. Add CYBERNETICS avatar adapter.
7. Add Broadcast Studio status integration.
8. Add World Monitor deep link/hand-off.
9. Add policy-gated operator actions only after backend tests and approval design.

## Acceptance criteria for the first vertical slice

- Runs locally in a supported desktop browser.
- User can navigate the 3D hub and open all four stations/portals.
- CYBERNETICS has a stable identity anchor and visible connection state.
- Specialist agents are represented from typed fixtures or registry data; fixtures are labeled simulated.
- No browser-side secrets or direct privileged service calls.
- Every operational status has source and freshness metadata.
- Unit/build checks and a documented manual smoke test pass before calling the slice verified.

## Non-goals for the first slice

No autonomous financial actions, wallet signing, public broadcasting, social publishing, unrestricted agent-to-agent calls, or claims of live telemetry without a verified source.
