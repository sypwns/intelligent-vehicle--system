# JolSense — Verification and Evaluation Plan

Project: ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control  
Status: Planned tests only; no execution results are claimed in this file.  
Related: `docs/requirements/requirements-specification.md`, `docs/architecture/architecture-description.md`, `docs/project-management/risks-and-dependencies.md`

## 1. Purpose and boundaries

The objective is to verify that the JolSense prototype behaves according to its functional and non-functional requirements, and that failures are visible rather than reported as successful operations. The product verification below is separate from the formal research question about comparing weather/road-only and vehicle-health-aware route advice. Neither type of testing alone proves fewer real-world crashes.

No real vehicle actuation or intentionally dangerous driving is required. Use sanitized, explicitly labeled simulated/sample/replay inputs until authorized integrations can be safely evaluated.

## 2. Environments, instruments and recorded evidence

- Mobile: document Flutter version, target Android emulator/device, OS version, permissions and build identifier when available.
- Backend: Node.js is selected. Record the chosen framework, Node.js version, service URL, authentication, configuration example, and scheduler settings once implemented. Aisha leads backend/mobile integration; Karima leads the ML inference service and model interface; Blizzard Mode backend behavior is jointly reviewed.
- Test data: deterministic vehicle records with units, timestamps, IDs, DTCs or health states; weather/route fixture responses with provider/time provenance; nonpersonal simulated geographic coordinates.
- Emergency tests: opted-in test contacts and a test notification provider; simulated heartbeat loss, no-response, acknowledgement and offline events.
- Record each executed case with case ID, date, build/commit, environment, input fixture, expected output, actual output, pass/fail, defects and evidence link. Leave these blank until a run is performed.

## 3. Functional test cases

| Test ID | Requirements | Planned case | Expected acceptance outcome | Approach |
|---|---|---|---|---|
| T-01 | FR-01–03 | Create/edit/delete vehicle; load valid and malformed telemetry | Profile changes persist; telemetry source/units/time visible; invalid input rejected | UI/unit test and sample replay |
| T-02 | FR-02–05, FR-25 | Missing and stale readings, unknown DTC | Shows unavailable/stale/unknown without implying healthy status | Negative input fixtures |
| T-03 | FR-06–07 | Defined fault/anomaly input and unsupported data | Model output and uncertainty/version are distinguishable from confirmed diagnosis; invalid input handled | ML contract and UI tests |
| T-04 | FR-08–09 | Create/change maintenance event; assess next-service reminder | Saved history and explained trigger; insufficient data clearly reported | Functional test |
| T-05 | FR-10–11 | Weather/route providers return two candidates, one candidate or failure | Correct candidate count, distance/duration, provider times; no invented alternatives | Provider adapter tests |
| T-06 | FR-18–20 | Compare weather/road-only vs vehicle-aware assessment on same routes | Reasons and changed/unchanged outputs visible; health absence labeled; postpone/insufficient option where applicable | Paired fixtures and content review |
| T-07 | FR-22–24, NFR-05 | Mock control and unauthorized request | Mock commands do not actuate; unsupported actions disabled; unauthorized request blocked | Sandbox and security test |
| T-08 | FR-26, NFR-01, NFR-11 | Navigate core sections, scale text, inspect status labels | Features accessible, understandable, readable, not color-only | Manual accessibility walkthrough |
| T-09 | FR-12–13, FR-16 | Simulate 20-minute stationary interval under low temperature away from known places | Defined thresholds lead to suspected-stop state and check-in; inputs and rules logged | Scenario/state-transition test |
| T-10 | FR-12, FR-16–17, NFR-13 | Traffic jam, urban parking, rest stop, missing speed or location | No unjustified confirmed-emergency claim; no-response only where specified; missing data visible | False-positive/negative scenarios |
| T-11 | FR-13–14, NFR-13 | Select “I'm fine”, snooze, or “I need help”; let check-in time out | Pause/recheck behavior and escalation follow configured states | Mobile UI + timer fixtures |
| T-12 | FR-14, NFR-14–15 | Opt-in contact, revoke contact, trigger alert | Only authorized test recipient receives permitted last-known location/time and contextual details; opt-out respected | Integration/privacy tests |
| T-13 | FR-15, FR-17, NFR-13, NFR-15 | Stop heartbeats; lose network; restore connectivity | Server identifies timeout as possible loss of contact, not proof of stranding; retry/status recorded; duplicate alerts limited | Clock-controlled backend/integration tests |

All IDs above correspond to the current requirements specification. Review mappings whenever a requirement ID or scope changes.

## 4. Non-functional and security checks

| Check | Measure / conditions | Acceptance or reporting rule |
|---|---|---|
| NFR-02, NFR-03 — integrity/traceability | Invalid units, duplicate/missing IDs; inspect scenario source, timestamp and rule/model version | No silent corruption; outputs traceable to source and version |
| NFR-04, NFR-06 — privacy/secrets | Inspect repository, config examples, logs and contact/location retention design | No committed live secrets or personal trip history; only placeholders in tracked examples |
| NFR-07 — fault tolerance | Simulated API/network interruption and restart | Errors and last-update states visible; no false delivery/control success |
| NFR-08, NFR-12 — reproducibility | Clean checkout/setup with documented dependencies and fixtures | Another team member can reproduce an identified deterministic case when runnable implementation exists |
| NFR-09 — performance | Locally available dashboard and saved scenario, 20 trials in documented environment | Target: within 3 seconds in at least 95% of trials; external network delays reported separately |
| NFR-10, NFR-11 — compatibility and accessibility | Declared Android target, font scaling, control labels | Explicit device/OS support list; meaningful accessible labels |
| NFR-13–15 — Blizzard reliability/privacy/status | Consent, heartbeat loss, interrupted/background mobile execution, retry and status transitions | Clearly states actual status (pending/sent/confirmed if receipt known), platform limits and inability to guarantee rescue |

## 5. Metrics and baseline definitions

| Feature | Baseline or reference | Planned metric | Interpretation |
|---|---|---|---|
| Vehicle diagnostics | Simple documented DTC/threshold or majority-class baseline appropriate to dataset | Precision, recall, macro F1 and confusion matrix where labeled data supports them; otherwise anomaly error/false-alarm analysis | Report class distribution, held-out set, false positives and false negatives; no invented accuracy |
| Route recommendation research comparison | Configuration A: weather + road conditions alone; B: identical candidate routes and environmental inputs plus valid vehicle-health indicators | Agreement rate against independently reviewed suitability rubric; unsuitable-route selection rate; paired difference | Same scenarios and candidate routes; scenario construction labeled; independent references required before claims |
| Blizzard suspected-stop detection | Documented rule set including time/speed/location/temperature | Detection delay, false-alert fraction across predefined normal-stop cases, missed detections across predefined hazard scenarios | Report performance on simulated cases, not real emergency sensitivity |
| Blizzard check-in and alerting | Configured timeout and heartbeat grace period | Time from trigger to check-in, from timeout to send attempt, delivered/failed/pending proportion if delivery data exists | Server receipt/send attempt is not proof a person read the alert; measure delivered only with provider confirmation |
| Core mobile/backend | Expected functional behavior from requirements | Passed/failed cases; API error handling; timed latency | Report test environment and unresolved defects |

## 6. Fairness, leakage prevention and interpretation

For ML datasets, split at an appropriate vehicle, session or time boundary when records are correlated; prevent duplicates or overlapping windows crossing splits. Keep preprocessing fitted only to training data. When feasible, evaluate held-out records and compare an appropriate baseline under matched input and metrics. No ML training is required merely to write this plan.

For the route evaluation, keep the route set, weather, road conditions and departure time constant across both configurations; only the vehicle-health input differs. Define the reference suitability rubric before inspecting outputs and obtain independent review. Use paired descriptive comparisons and failure-case analysis; inferential tests only if sample size and outcome structure support them.

For Blizzard Mode, test traffic queues, city stops, valid safe rests, loss of GPS, low phone battery, missing OBD data, no network and missing heartbeat. A possible loss-of-contact warning must never be represented as a verified emergency. A successful test-bot alert does not establish production emergency availability.

## 7. Execution stages and exit criteria

1. Planning/readiness: verify existing requirement IDs against the requirements specification; define test fixtures, providers, mock contact consent, config templates and expected outputs.
2. Component verification: run unit tests and state-machine tests against deterministic inputs; retain run logs and defects.
3. Integration verification: exercise Flutter ↔ Node.js ↔ mock/provider interfaces, persistence, retries and failure status.
4. End-to-end walkthrough: demonstrate monitoring, diagnosis, maintenance, trip planning, mock controls and Blizzard Mode with only permitted test materials.
5. Review: reconcile defects and requirement gaps, obtain teammate review, and document known limitations before assigning a release tag.

A mandatory requirement is considered verified only after its mapped case has an executed record with evidence and no unresolved blocking failure. Until then, state is Planned or Not tested; this document does not assert completion.
