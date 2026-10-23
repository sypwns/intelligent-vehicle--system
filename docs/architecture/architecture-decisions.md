# Architecture Decisions

Project: ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control  

Status: Initial architecture decision log — planning stage  

Repository path: `docs/architecture/architecture-decisions.md`  

Related documents: [`architecture-description.md`](architecture-description.md), [`../requirements/requirements-specification.md`](../requirements/requirements-specification.md)

## 1. Decision-log conventions

This file records proposed and confirmed architectural choices for the main diploma application. It does not claim a technical comparison, implementation, testing, or supervisor approval has occurred when it has not.

- Proposed: Team's current direction; alternatives and feasibility require review.

- Pending: No selected option; evidence or access is needed.

- Accepted: Adopted after a documented team/supervisor review. None of the entries below are asserted to have this status yet.

- Superseded: A later decision replaces this entry; preserve history and link the replacement.

For each decision, record the date, responsible reviewers, technical comparison evidence and YouTrack ID when available; do not invent them.

## ADR-001 — Separate user interface, business services, and external adapters

- Status: Proposed

- Context: The application has vehicle monitoring, diagnostics, maintenance, routing/weather and optional control providers. Any particular provider may be unavailable or unsupported (FR-03, FR-04, FR-10–11, FR-16–19; NFR-07, NFR-12).

- Proposed choice: Isolate mobile UI from service logic, and isolate each external provider behind a defined adapter/interface. Support sample/replay adapters independently from live integrations.

- Alternatives: All processing inside the mobile application; one combined server with provider calls embedded directly in features.

- Rationale: Replaceable adapters allow safe prototype demonstrations, clearer error handling and deterministic tests when APIs or real vehicles are inaccessible.

- Trade-offs: More interfaces and validation; cross-component integration must be tested.

- Decision needed: Confirm practical process boundaries after backend and deployment comparison.

## ADR-002 — Flutter for the mobile interface

- Status: Proposed (team's stated preference)

- Context: One driver-facing mobile application should present all planned features (FR-01, 02, 05, 08, 10–14, 16, 20).

- Proposed choice: Use Flutter to implement the mobile client and a consistent interface for status, diagnostics, maintenance, trips and control availability.

- Alternatives to compare: Native Android (Kotlin); React Native.

- Rationale: A single UI codebase is attractive for a student team and keeps mobile feature development consistent. This is a design preference, not a measured performance finding.

- Trade-offs: Learning/build complexity; vehicle/provider SDK support and target platform compatibility must be checked.

- Decision needed: Confirm supported Android devices/versions, SDK compatibility and technical comparison results.

## ADR-003 — Backend framework and data storage

- Status: Pending

- Context: The system needs input validation, services, persistence, API coordination, and secure configuration (FR-01, 03, 08–14; NFR-02–04, 06, 08).

- Candidates: Python/FastAPI or an alternative backend; PostgreSQL, SQLite or another appropriate database. No option is declared selected solely by appearing here.

- Selection criteria: Student familiarity, deployment complexity, API/model interoperability, persistence needs, cost, security support, and testability.

- Trade-offs: Server/database services add setup and operational dependencies; fully local storage simplifies demonstrations but may limit future synchronization.

- Decision needed: Record at least 2–3 alternatives per key technology and a justified selection in `docs/technology-comparison/` before setting up dependencies.

## ADR-004 — Diagnostic ML as a bounded, versioned component

- Status: Proposed

- Context: The diploma includes ML-based fault/anomaly assessments and understandable warnings, but compatible labeled vehicle data and evaluated accuracy are not yet established (FR-05–07; NFR-03).

- Proposed choice: Define an explicit diagnostic input/output schema and treat trained model inference as a separate logical component. The response must preserve model/rule version, source record, score/label where supported, and limitations.

- Alternatives: Hard-coded DTC descriptions/thresholds alone; tightly embedded prediction logic in the UI; a separate inference service.

- Rationale: Keeps observed DTCs separate from inferred anomalies and permits fair evaluation of the chosen ML task later.

- Trade-offs: Requires data qualification, version management, validation of false warnings, and honest communication of uncertainty.

- Decision needed: Choose supported diagnostic targets, licensed data, baseline, model family and evaluation metrics; do not state model results before testing.

## ADR-005 — Environment-only baseline and vehicle-health-aware travel advice

- Status: Proposed

- Context: Route recommendations must account for weather/road risks, while additional health indicators may affect suitability (FR-10–14; NFR-03).

- Proposed choice: Keep two logically comparable assessment configurations: (A) weather/road inputs only; (B) those same inputs plus validated vehicle-health information. Preserve routes and environmental inputs in comparisons; report missing health data as missing.

- Alternatives: One combined assessment only; travel advice that ignores vehicle health entirely.

- Rationale: Offers an interpretable application behavior and permits the defined scenario-based research evaluation without changing the product scope.

- Trade-offs: Needs credible scenario combinations and an independent route-suitability rubric; results cannot demonstrate real-world crash reduction.

- Decision needed: Define risk/suitability criteria, reference-assessment review, scenario provenance and permitted weather/routing data.

## ADR-006 — No real vehicle remote actuation in the mandatory prototype

- Status: Proposed safety constraint, required by initial requirements

- Context: OBD-II telemetry access does not grant remote-control capability. Real operations require a compatible authorized provider and safety measures (FR-16–18; NFR-05).

- Proposed choice: Demonstrate capability availability and any commands only through clearly labeled local/mock/sandbox mode. Real commands are outside the mandatory initial prototype.

- Alternatives: Integrate an OEM/provider interface later after a separate approval and safety review; omit control demonstrations altogether if even mock flows cannot be validated.

- Rationale: Avoids unsupported and potentially hazardous operations while allowing interface/workflow verification.

- Trade-offs: Mock demonstrations do not prove actual remote command availability or reliability.

- Conditions for reconsideration: Verified legal access/ownership, compatible provider, authenticated authorization, documented command safety checks, explicit confirmation, safe test setup, and truthful success/failure reporting.

## ADR-007 — Traceable data provenance and explicit failure states

- Status: Proposed

- Context: Separate telemetry, weather, routing and diagnostic sources can be stale, inconsistent or absent; output must remain interpretable (FR-02–03, 05, 10, 19; NFR-02–03, 07).

- Proposed choice: Each displayed result retains input source/type, timestamps, units and applicable rule/model version. Missing or invalid measurements yield “Unavailable” or “Insufficient information,” not a fabricated safe state. Separate raw inputs from processed results.

- Alternatives: Silent default values; replacing missing measurements with nominal/healthy values.

- Rationale: Supports reproducing failures, explaining recommendations and limiting misleading output.

- Trade-offs: Extra schema and validation work; fewer confident outputs when source quality is poor.

- Decision needed: Define freshness limits, acceptable field ranges and audit fields after choosing actual providers.

## ADR-008 — Repository security and prototype privacy by design

- Status: Proposed

- Context: Vehicle profiles, driving locations and integration credentials may be sensitive (NFR-04–06).

- Proposed choice: Commit only safe examples with dummy identifiers and placeholder environment values; exclude secret files, API credentials and personal trip data from version control. Design deletion for stored vehicle and maintenance records. Minimize logging of private fields.

- Alternatives: Hard-coded credentials or real captured records in repository fixtures — rejected as unsafe.

- Rationale: Reduces accidental data exposure and supports committee inspection of safe development practices.

- Trade-offs: Requires environment setup guidance, sanitized fixtures and explicit data handling procedures.

- Decision needed: Define actual data locations, retention/deletion policy and provider-specific security requirements during implementation.

## ADR-009 — Testable initial release using sample/replay inputs

- Status: Proposed

- Context: Full real-vehicle and external integration may be unavailable during pre-defense, but mandatory functionality needs measurable acceptance criteria (FR-03, 06, 18; NFR-08, 12).

- Proposed choice: Organize `src/mobile/`, `src/backend/`, `src/ml/`, `tests/`, and `config/` from the beginning. Initial component placeholders are acceptable; later tests will include valid, missing and malformed inputs, API failures and mock-control checks.

- Alternatives: Wait to structure code/tests until the complete app exists; depend entirely on real vehicle access for demonstration.

- Rationale: Enables incremental implementation with traceable, deterministic evidence and reduces dependence on external services.

- Trade-offs: Sample/replay behavior is not proof of real-vehicle interoperability; evidence must be labeled accurately.

- Decision needed: Define test fixtures, execution instructions, module integration cases and baseline verification conditions.

## ADR-010 — Blizzard Survival Mode uses a hybrid mobile/backend safety workflow

- Status: Proposed
- Context: JolSense plans a distinctive support feature for drivers who may be stranded in low temperatures outside familiar urban areas. A stationary vehicle alone does not establish danger; a powered-off phone, background OS restriction, missing GPS or network outage can prevent on-device checks.
- Proposed choice: Implement an explicitly enabled `Normal → Suspected → Check-in → Escalation` flow, with I'm fine, I need help, configurable snooze and repeated check-ins. Use speed/stationary duration, optional engine data, temperature, weather and approximate location/safe-place context. Store unavailable inputs as unknown and make thresholds adjustable. Do not infer emergency automatically from one weak signal.
- Monitoring responsibility: The phone attempts periodic heartbeats while monitoring can run. A backend tracks active sessions, last-received heartbeat and last-known location; missing heartbeats can trigger a possible loss of contact alert after a configurable grace period, not a confirmed-stranding message.
- Alert responsibility: Notify only contacts who explicitly opted in. For the prototype, use a test Telegram bot with test contacts; consider an authorized SMS gateway later. Clearly differentiate queued, sent-to-provider, failed and provider-confirmed delivery, where possible. If no connection exists, queue locally when possible and retry; the server cannot retrieve unsent phone data after disconnection.
- Privacy and safety: Require user enablement, contact consent, minimal retention and a way to revoke sharing. Do not automatically call 112; offer a manual emergency-call action. Clearly show limited monitoring coverage and that alerts or rescue cannot be guaranteed.
- Alternatives: Phone-only alerts; server-only monitoring; direct phone SMS. Phone-only cannot reliably operate after power loss, server-only lacks fresh sensor readings during disconnection, and background/direct SMS behavior differs materially across Android and iOS.
- Rationale: Hybrid detection and heartbeat escalation offers a testable safety workflow while exposing uncertainty and platform limitations.
- Trade-offs: False positives from rest stops/traffic, false negatives from missing inputs, notification fatigue, server/provider cost and availability, sensitive location handling, permission/OS constraints and delayed delivery.
- Prototype validation: Simulated stationary/cold/remote scenarios plus counterexamples (traffic jam, planned stop, city parking); timed check-in/no-response flows, missed heartbeat tests, offline/queue/retry tests, permission denial and test-contact delivery logging. Measure detection delay, false alarms within the defined constructed scenarios and end-to-end alert timing; do not represent simulated outcomes as field evidence.
- Decision needed: Confirm requirements IDs, background behavior for each target OS, backend/scheduler and notification provider, safeguards against duplicate alerts, heartbeat/check-in cadence, storage/retention policy, and independent review of the escalation language.

## 2. Open decisions and review checklist

| Decision | Current state | Evidence needed to finalize |

|---|---|---|

| Mobile framework and target platform | Flutter proposed | Platform support, alternatives and stakeholder/team review |

| Backend technology | Pending | Alternatives, team capacity, API/ML integration and deployment plan |

| Storage solution | Pending | Data retention/security requirements, local vs server needs, alternatives |

| Diagnostic method/data | Pending | Target fault classes, dataset provenance, licensing, baseline and test plan |

| Weather/routing providers | Pending | Geography, accuracy/freshness, provider terms, quotas and failure states |

| Actual telemetry adapter | Pending | Supported vehicle/OBD-II parameters, hardware, authorization, tests |

| Real remote-control provider | Not in mandatory initial scope | Written compatibility/authorization and safe test plan |

| Blizzard Survival Mode | Proposed | Threshold scenarios, consent procedure, heartbeat and escalation timings, test delivery channel, offline behavior, OS restrictions and privacy policy |
| System hosting | Pending | Cost, availability, network/privacy constraints and deployment simplicity |

### Before marking any ADR accepted

1. Record the decision date, reviewers and relevant YouTrack task.

2. Link technology comparison or prototype evidence when the decision depends on it.

3. Check consistency with requirement IDs and with System Context, Use Case and Component diagrams.

4. Document changed assumptions, consequences and any superseded decisions.

5. Submit modifications through a teammate-reviewed pull request.

Current completion statement: These entries document architectural intent and unresolved decisions. They are not claims of finished implementation, completed comparison, validated integrations, or supervisor approval.
