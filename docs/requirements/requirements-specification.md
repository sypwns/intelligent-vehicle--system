# Requirements Specification

Project: ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control  
Team: Shurenova Karima; Yerbolat Aisha  
Supervisor: Symbat Nurgalieva  
Document status: Initial specification — proposed requirements, not verified implementation  
Repository location: `docs/requirements/requirements-specification.md`

## 1. Purpose and scope

This document defines the planned requirements for a driver-facing mobile application that combines vehicle status monitoring, machine-learning-based fault assessment, understandable diagnostic and maintenance guidance, weather-aware travel recommendations, and authorized remote vehicle functions where a compatible integration is available.

The principal users are vehicle owners and drivers. The application is intended as decision support, not a replacement for a mechanic, official weather warnings, vehicle safety systems, or a driver's judgment. No live vehicle connection, dataset availability, supplier integration, or completed functionality is claimed at this stage.

The broader diploma product includes all five functions. The separately defined research comparison—weather/road-only route recommendations versus recommendations also considering vehicle-health indicators—can inform later evaluation but does not substitute for verification of the application requirements in this document.

## 2. Requirements management

Priority legend

- M — Mandatory (target MVP): Required for a demonstrable application prototype, using documented test or simulated inputs if real integration is unavailable.
- S — Should have: Important planned functionality, conditional on data, time, or access.
- C — Could have: Additional functionality after mandatory work is verified.

Evidence legend

- Functional test: Run defined user actions and check observable outputs.
- Integration test: Verify a documented interface, API response, and failure handling.
- Dataset/model test: Evaluate with a documented dataset, baseline, metrics, and held-out records.
- Inspection/review: Examine design, configuration, trace logs, or documented procedures.
- Security test: Verify authorization, input validation, and secret-handling controls.

All requirements below are planned. Passing a criterion requires stored evidence (test result, screenshot of an actual implementation, test log, or review record); these have not yet been produced.

## 3. Functional requirements

| ID | Priority | Requirement | Verifiable acceptance criteria | Verification |
|---|---|---|---|---|
| FR-01 | M | Vehicle profile management | A user can add, view, edit, and remove a vehicle profile with at least make, model, model year, and a locally assigned vehicle label. Invalid required values generate a clear error. | Functional test |
| FR-02 | M | Vehicle status dashboard | The dashboard displays supported readings, their units, and last-update timestamps; unavailable values display Unavailable, not zero or a normal status. | Functional test |
| FR-03 | M | Vehicle data input and provenance | The prototype accepts documented sample/replay vehicle-health records through a defined input format and labels each record's origin (sample, replay, or connected source). Invalid records are rejected with a reason. | Functional test; inspection |
| FR-04 | S | Compatible OBD-II/telemetry integration | For a documented supported adapter or source, the system reads available standardized parameters and diagnostic trouble codes (DTCs). Unsupported parameters are identified explicitly; disconnects are handled without falsely displaying current data. | Integration test |
| FR-05 | M | Diagnostic warning presentation | A recognized DTC or abnormal input produces a warning with the affected parameter/code, detection time, severity or uncertainty label, and a plain-language description. Unknown codes are shown as unknown rather than assigned an invented cause. | Functional test |
| FR-06 | M | ML diagnostic prototype | A documented ML pipeline produces a fault/anomaly label or score for a defined input schema; the application can display the output. A valid test record and invalid/missing-data record yield reproducible and distinguishable responses. | Dataset/model test; functional test |
| FR-07 | M | Explainable diagnostic advice | For supported warnings, the application shows an explanation of the observed signal or code, what may be wrong, and a cautious recommended next step. The text distinguishes a model prediction from a confirmed mechanical diagnosis. | Functional test; content review |
| FR-08 | M | Maintenance record management | A user can create, view, edit, and delete dated maintenance records containing service type and mileage (or a recorded reason if mileage is unavailable). Records are retained correctly across application restarts. | Functional test |
| FR-09 | M | Personalized maintenance plan | The application generates a next-service suggestion from documented service intervals plus available mileage, service history, or vehicle-health alerts. It states the triggering information and indicates when information is insufficient. | Functional test |
| FR-10 | M | Weather information retrieval/display | For a selected location and planned travel time, the system presents supported forecast values (e.g., temperature, precipitation, wind) with forecast time, provider, and update time. Failed or stale retrievals are visibly identified. | Integration or replay test |
| FR-11 | M | Route candidate presentation | For a specified origin and destination, the application displays at least two route candidates when the routing source provides them, including distance and expected duration; if fewer exist, it explains the limitation. | Functional/integration test |
| FR-12 | M | Potentially stranded detection | The application detects potentially dangerous vehicle stops using available speed, engine status, location, temperature, and weather information according to predefined thresholds. | Scenario-based functional test |
| FR-13 | M | Driver safety check-in | When a potentially dangerous stop is detected, the application displays an audible and visual notification with “I'm fine” and “I need help” options. The driver can also temporarily pause monitoring. | Functional/UI test |
| FR-14 | M | Emergency contact alerts | If the driver requests help or does not respond within the configured timeout, the system attempts to notify opted-in emergency contacts with the last known location, time, and available environmental information. | Integration test |
| FR-15 | M | Server-side heartbeat monitoring | During an active Blizzard Mode session, the application periodically sends heartbeats to the backend, which detects missed heartbeats and initiates the defined escalation procedure. | Heartbeat interruption test |
| FR-16 | M | False-alarm prevention | The system distinguishes predefined traffic jams, planned stops, and normal parking scenarios from potentially dangerous stops using configurable detection rules. | Scenario-based test |
| FR-17 | M | Connectivity failure handling | When GPS, vehicle data, or internet connectivity is unavailable, the system displays the limitation, preserves pending alerts where possible, and retries transmission when connectivity returns. | Failure simulation test |
| FR-18 | M | Weather/road safety assessment | Each candidate route receives an interpretable advisory or risk category based on explicitly defined weather and road-condition rules or model outputs. The factors leading to the advisory are shown. | Functional test |
| FR-19 | M | Vehicle-aware travel advice | Where valid health indicators exist, the user can see how these inputs affect the route assessment. When health information is missing, the system labels advice as environmental-only rather than implying a healthy vehicle. | Functional test |
| FR-20 | M | Comparison and recommendation transparency | The user can inspect the suggested route, key reasons, and relevant limitations; the system can recommend postponing a trip when defined conditions indicate no suitable route, without claiming to guarantee safety. | Functional test |
| FR-21 | S | User notifications | User-visible notifications are generated for configured critical diagnostic warnings and significant trip advisories, with a way to open the underlying information. Duplicate notifications for unchanged data are limited. | Functional test |
| FR-22 | M | Remote-control capability discovery | For each requested control (e.g., lock/unlock, climate, start), the interface shows Available, Unavailable, or Not configured according to a documented compatible integration. No action control is offered as working without a supported provider. | Functional test; inspection |
| FR-23 | S | Authorized remote commands | Where an approved vehicle-provider interface is available, the application submits only supported commands after authenticated authorization and explicit user confirmation; it reports provider success/failure/unknown status without assuming execution from request submission alone. | Integration/security test |
| FR-24 | M | Safe remote-control prototype | Without a compatible authorized vehicle integration, any remote-control demonstration operates only in a clearly labeled mock/sandbox mode and must never transmit actual actuator commands. | Functional/security test |
| FR-25 | M | Error and insufficient-information handling | Network loss, invalid telemetry, unavailable vehicle features, and missing weather/routing data produce explicit recoverable messages; no missing reading is silently treated as safe. | Functional test |
| FR-26 | M | Basic information access and navigation | From the main screen, a user can access vehicle status, diagnostics, maintenance, trip planning, and remote-control capability status, and return without losing unsaved work without warning. | Functional test |

### Remote-control safety boundary

Ordinary OBD-II data access does not provide general-purpose authorization to remotely start a vehicle, unlock doors, operate windows, or control climate. Actual commands require an approved, compatible OEM or service-provider interface and compliance with provider policies. For the mandatory MVP, a clearly labeled mock demonstration is acceptable. Remote engine start and other potentially hazardous actions will not be implemented against a real vehicle until compatibility, ownership authorization, safety interlocks, and supervisor-approved testing arrangements are confirmed.

## 4. Non-functional requirements

| ID | Priority | Requirement | Verifiable acceptance criteria | Verification |
|---|---|---|---|---|
| NFR-01 | M | Usability and clarity | All five core sections are reachable from the home navigation in no more than three user actions; technical alerts state their meaning and next step in readable language. | Manual task walkthrough |
| NFR-02 | M | Input validation and data integrity | Required fields and allowed ranges/units are documented; invalid or malformed values do not change stored valid records and produce actionable validation messages. | Functional test |
| NFR-03 | M | Traceability of recommendations | Each saved demonstration result can be linked to input record/scenario ID, data source, timestamp, decision configuration, and rule/model version. | Inspection; functional test |
| NFR-04 | M | Security of sensitive settings | No live passwords, API keys, tokens, or personal vehicle/location histories are committed to Git; example configuration files use placeholders. | Repository inspection |
| NFR-05 | M | Remote-command authorization | An unauthenticated or unauthorized request cannot invoke a real remote command; a mock-mode request must remain local/sandboxed. | Security test |
| NFR-06 | M | Privacy/data minimization | Only data needed for selected features is collected; stored vehicle and trip information is documented, and users can remove locally stored vehicle profiles and maintenance records in the prototype. | Design review; functional test |
| NFR-07 | M | Fault tolerance and transparency | If an external provider is unavailable, the app communicates the failure and last valid update time instead of displaying stale data as live or reporting an unconfirmed control result as successful. | Failure-injection test |
| NFR-08 | M | Reproducibility | The repository documents dependencies, configuration placeholders, sample input schemas, and steps to run the prototype and reproduce at least one deterministic scenario result once implementation is available. | Clean-environment setup test |
| NFR-09 | S | Performance | In a documented test environment with locally available inputs, the status/dashboard screen and saved scenario assessment load within 3 seconds in at least 95% of 20 repeated trials. Network latency is measured separately. | Timed performance test |
| NFR-10 | S | Compatibility | The mobile prototype runs on a documented supported Android version/device or emulator, with unsupported platforms clearly listed; actual supported vehicle integrations are listed separately. | Device/emulator test |
| NFR-11 | S | Accessibility | Essential text remains readable under increased font scaling, interactive controls have meaningful labels, and status is not communicated by color alone. | Accessibility inspection |
| NFR-12 | M | Maintainability and testability | Mobile, backend, and ML code are organized by component; configuration is separate from source code; every mandatory feature has at least one documented verification case, even before test automation exists. | Repository/test-plan review |
| NFR-13 | M | Emergency monitoring reliability | Blizzard Mode supports configurable detection thresholds and check-in timeouts, handles interrupted monitoring sessions, and clearly indicates when continuous monitoring cannot be guaranteed. | Reliability and interruption tests |
| NFR-14 | M | Emergency contact privacy | Emergency contacts must opt in before receiving alerts. Location information is shared only according to the user's authorized alert settings, and the user can revoke contact permissions. | Privacy and permission tests |
| NFR-15 | M | Alert status transparency | The application distinguishes pending, sent, failed, and confirmed-delivered alerts when delivery confirmation is available. It explicitly states that JolSense does not guarantee emergency rescue. | Functional/UI test |
Performance thresholds are preliminary engineering targets, not measured performance or formal acceptance by an external client. Revise them only with a documented rationale and review.

## 5. Functional coverage and priorities

| Area | Mandatory prototype scope | Conditional or later scope |
|---|---|---|
| Monitoring | Profile, status UI, timestamped sample/replay inputs | Real OBD-II or telematics connectivity |
| Diagnostics | ML diagnostic prototype, warnings, cautious explanations | Broader vehicle coverage and more fault categories |
| Maintenance | Record management and personalized rule-based planning | Advanced predictive service scheduling |
| Weather safety | Weather/road advisories, candidate routes, health-aware reasoning | Expanded real-time data and additional route-risk models |
| Remote control | Capability display and explicit mock mode | Actual approved OEM/provider integration |

## 6. Verification and acceptance process

1. Define test material: Use permitted, documented sample inputs and constructed test scenarios; label them as such. Do not represent them as field measurements or completed user studies.
2. Verify mandatory functions: For each `FR` and `NFR` marked M, create at least one test case listing inputs, expected result, actual result, and evidence location.
3. Test failures explicitly: Include missing/invalid telemetry, stale weather, unavailable routes, unsupported remote functions, and unauthorized command attempts.
4. Evaluate ML separately: Document dataset provenance, preprocessing, baseline, leakage prevention, class balance, held-out test split, and appropriate task metrics (e.g., precision, recall, F1, confusion matrix for fault classification). No performance metric is claimed before running experiments.
5. Review outstanding work: Mark a requirement Pass, Fail, Blocked, or Not tested only from actual verification evidence. Unimplemented requirements remain Not tested.

Minimum verification record template: `Requirement ID | Test case ID | Preconditions/input | Expected result | Actual result | Evidence link | Status | Reviewer | Date`.

## 7. Requirement-level dependencies, assumptions, and risks

The `REQ-D` series is local to requirements. Project-wide dependencies use the separate `D-01`–`D-09` series in `docs/project-management/risks-and-dependencies.md`.

| ID | Dependency or risk | Planned handling |
|---|---|---|
| REQ-D-01 | Vehicle telemetry formats and supported parameters vary by vehicle and adapter. | Use a documented input schema and sample/replay fallback; validate an adapter before announcing compatibility. |
| REQ-D-02 | Public diagnostic datasets may not match a consumer vehicle or include enough labeled failures. | Check provenance, license, vehicle coverage, labels, and feature availability before choosing data or model tasks. |
| REQ-D-03 | Weather and routing APIs have coverage, quotas, latency, and licensing constraints. | Document provider terms, requested fields, failure modes, and reproducible example responses. |
| REQ-D-04 | Real vehicle remote control requires compatible authorized services and careful safety testing. | Keep real controls out of the mandatory MVP; use sandbox demonstrations until approvals/integration exist. |
| REQ-D-05 | Incorrect warnings or route advice could lead users to unsafe decisions. | Display evidence, uncertainty, limitations, and clear advisories; do not claim certified safety or medical/mechanical certainty. |
| REQ-D-06 | Initial driver interviews have been conducted, but the number, recruitment, consent record, and representativeness of participants still require complete documentation, and further validation may be needed. | Preserve anonymized interview evidence with consent safeguards; distinguish observed interview themes from unvalidated design assumptions and review priorities as evidence grows. |

## 8. Scope exclusions and change control

The initial project does not include autonomous driving, guaranteed fault prediction, repair execution, emergency response dispatch, certified route safety, real-world accident reduction claims, or unsupported vehicle actuation. iOS support, production deployment, and additional OEM integration are not mandatory until approved as feasible.

Changes to mandatory features, priorities, safety constraints, or acceptance criteria should be recorded in the requirements history and linked to a YouTrack task and reviewed Git pull request. Architectural decisions should reference the affected requirement IDs.

## 9. Traceability to diploma deliverables

| Diploma deliverable | Requirements evidence |
|---|---|
| Proposal and scope | Section 1; Sections 5 and 8 |
| Technology comparison | Requirements dependencies REQ-D-01 through REQ-D-04 and requirements NFR-08, NFR-10 |
| Existing system comparison | Functional coverage areas in Section 5 |
| System context, use case, component diagrams | External connections and functional groups FR-01–FR-26 |
| UX prototype | User flows FR-02, FR-05, FR-08, FR-11, FR-13–14, FR-18–20, FR-26 |
| Verification and evaluation plan | Section 6; measurable FR/NFR acceptance criteria |
| Project management | Requirement IDs, priorities, risks, and change control |

Current verification state: All requirements are proposed / not yet tested. The specification provides acceptance criteria that can be verified during subsequent implementation and testing; it does not imply that the application or integrations already exist.
