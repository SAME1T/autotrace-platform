# ADR 0001: Architecture choice (Modular Monolith)

## Status
Accepted

## Context
AutoTrace will evolve across multiple domains:
- Traceability (part/lot/station/measurements)
- Quality (defect, NCR/8D, CAPA)
- Telemetry/Diagnostics (DTC, signals)
- Analytics (models, scoring)
- Dashboard

A microservices-first approach adds operational complexity early (multiple deployments, networking, auth, observability),
which is risky for a single-developer student project.

## Decision
We will start with a **Modular Monolith**:
- Single backend service and database
- Internal modular structure by domain (traceability, quality, telemetry, analytics)
- Clear boundaries via packages/modules, DTOs, and service interfaces

## Consequences
### Positive
- Faster development and easier debugging
- Lower operational overhead
- Easier to keep data consistency in early phases

### Trade-offs
- Requires discipline to keep module boundaries clean
- Some scalability concerns if everything stays in one service forever

### Future evolution
If needed, a module can be extracted into a separate service later by:
- introducing an internal API boundary
- moving the module data to a dedicated schema/database
- using messaging for events (e.g., TelemetryEvent)
