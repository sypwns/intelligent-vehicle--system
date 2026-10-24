# Architecture Description

Project: ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control  

Team: Shurenova Karima and Yerbolat Aisha  

Supervisor: Symbat Nurgalieva  

Status: Initial proposed architecture — not an implemented or tested system  

Repository path: `docs/architecture/architecture-description.md`

## 1. Architectural purpose and boundaries

The diploma project proposes a driver-facing mobile application for vehicle status monitoring, machine-learning-assisted diagnostics, maintenance guidance, weather- and road-aware trip assessment, supported remote functions, and Blizzard Survival Mode emergency check-ins for potentially stranded drivers. The architecture separates the user interface, application services, data adapters, diagnostic inference, and external providers so that unavailable integrations can be replaced by explicitly labeled sample/replay or sandbox implementations.

The application provides decision support, not certified vehicle safety assessment, guaranteed fault detection, vehicle repair, autonomous driving, emergency dispatch, or proof of crash reduction. Blizzard Survival Mode does not guarantee detection, connectivity, message delivery, or rescue. The broader diploma product covers all five feature areas. A distinct scenario-based comparison of weather/road-only recommendations against vehicle-health-aware recommendations may be used for research evaluation, but it is not a substitute for application functional testing.

Architecture status: The structure below is a design proposal. Flutter is the planned mobile approach and Node.js is the selected backend runtime. The Node.js framework, database, diagnostic ML integration protocol, external API providers, vehicle compatibility, hosting, and scheduler are not yet finalized or implemented.

## 2. System context and external actors

| External actor/system | Interaction | Dependency and boundary |

|---|---|---|

| Driver / vehicle owner | Manages vehicle profile, views monitoring and diagnostics, records maintenance, reviews trips, requests supported controls | Driver must not mistake advisory information for verified mechanical diagnoses |

| Vehicle telemetry source | Provides sensor values, diagnostic trouble codes (DTCs), units, timestamps, and source metadata | Initially documented sample/replay records; actual OBD-II/telematics only after compatibility verification |

| Weather information provider | Supplies forecast values for route locations and travel times | Provider, coverage, license, rate limits, and forecast freshness are unconfirmed |

| Mapping/routing provider | Provides route candidates, distance, duration, and road attributes when available | Do not invent a second route when only one is returned |

| Authorized vehicle-control provider | Identifies supported commands and may submit confirmed commands through an approved interface | No real actuation in the initial mandatory prototype; ordinary OBD-II access is not remote-control authorization |

| Opted-in emergency contact | Accepts an invitation and may receive a possible-emergency alert containing last known location and time | Consent/opt-in and revocation required; alert is not proof of emergency |
| Emergency notification provider (planned) | Sends server-originated Telegram notification in prototype; SMS gateway remains an alternative | Delivery depends on provider and network; do not confuse acceptance with confirmed delivery |
| Phone location / battery services | Supplies location, battery state and local check-in notifications during a user-enabled safety session | Foreground/background limits, permissions and sensor availability vary by OS |
| Future reviewer/tester | Reviews requirements, scenario/rubric definitions, and evidence of test outcomes | Reviews and evidence must actually occur before they are reported as completed |

Planned diagram: `diagrams/source/system-context.drawio` with readable export under `diagrams/exports/`. No diagram is claimed to be complete by this document.

## 3. Proposed logical components

Engineering ownership (planned): Yerbolat Aisha leads Node.js backend integration and coordination with the mobile application; Shurenova Karima leads the diagnostic ML service and model interface. Both review and integrate Blizzard Survival Mode backend behavior. Ownership does not imply these components are implemented.

| Component | Responsibility | Main requirement references |

|---|---|---|

| Mobile UI (Flutter — planned) | Vehicle selection, status dashboard, alerts, maintenance history, trip form/results, remote-control availability, understandable error states | FR-01–02, FR-05, FR-08, FR-10–11, FR-13, FR-18–20, FR-22, FR-26; NFR-01, NFR-11 |

| Node.js application/API layer (framework TBD) | Validates requests, applies access rules, coordinates services and data sources, returns consistent responses | FR-03, FR-19, FR-25; NFR-02, NFR-05, NFR-07 |

| Vehicle profile and status service | Stores vehicle metadata, validates telemetry, tracks units and last-update timestamps, distinguishes sample/replay/connected data | FR-01–04; NFR-03 |

| Diagnostics service / ML adapter | Validates model input schema; runs a documented classifier/anomaly detector when implemented; returns label/score, version, and uncertainty; keeps diagnostic codes separate from predicted faults | FR-05–07; NFR-03 |

| Maintenance service | Stores service events and derives next-service suggestions from declared intervals, mileage/history, and supported warnings | FR-08–09 |

| Weather and routing integration adapters | Retrieve/normalize weather and route data; preserve provider and observation/forecast times; detect missing/stale data | FR-10–11, FR-25; NFR-07 |

| Travel-assessment service | Applies documented weather/road advice and optional vehicle-health constraints; explains selected alternatives and permits “postpone/insufficient information” | FR-18–20; NFR-03 |

| Blizzard Survival Mode detector and mobile check-in | Evaluates prolonged stationary states, relevant vehicle/temperature/location signals, configurable exclusions and user check-ins; supports help, safe confirmation, and snooze | Blizzard-related FRs (IDs to reconcile with requirements) |
| Safety session and heartbeat monitor (backend) | Records authorized active sessions, last accepted heartbeat and last reported location; detects overdue check-ins/heartbeats for cautious escalation | FR-12–17; NFR-13–15 |
| Emergency contact and notification service | Stores consent/opt-in status, dispatches alerts through approved provider, tracks queued/sent/failed/confirmed statuses when supported | FR-12–17; NFR-13–15 |
| Remote-control capability adapter | Reports Available/Unavailable/Not configured; supports clearly labeled mock commands; real commands only with an approved provider, authentication, confirmation and status verification | FR-22–24; NFR-05 |

| Data storage (technology TBD) | Persists profiles, maintenance records, documented scenario inputs, result provenance, and authorized local settings | FR-01, 08; NFR-03, 06 |

| Configuration/logging layer | Holds nonsecret settings, environment-variable references, trace IDs, application error logs without exposing private data | NFR-03–04, 08, 12 |

Component diagram draft: `diagrams/source/component-diagram.drawio` and corresponding export. These are expected files, not yet verified outputs.

## 4. Key user interactions and data flows

### 4.1 Vehicle monitoring and diagnostics

1. Driver selects a stored vehicle profile.

2. Status service receives a documented sample or replay record initially; a connected telemetry adapter may later be supported.

3. Validation checks identifier, field type, units, expected ranges, timestamps, and provenance. Invalid records do not silently become healthy readings.

4. Mobile dashboard displays values with units and “last updated”; unavailable/stale readings are distinguished.

5. Diagnostics adapter evaluates supported data or interprets known DTCs. The output identifies code/condition, whether it is a prediction or observed DTC, supported explanation, and limits.

6. Driver sees a cautious recommended next action rather than an unverified repair instruction.

### 4.2 Maintenance recommendations

1. Driver enters dated maintenance events and mileage where available.

2. Maintenance service combines declared service intervals with available history and warnings.

3. Response identifies the suggested task, relevant trigger, and uncertainty or missing data.

4. Changes are saved and recoverable after restart; the user can edit/delete records.

### 4.3 Weather-aware trip advice

1. Driver specifies start, destination and intended departure time.

2. Routing adapter obtains available candidate routes; weather adapter obtains associated forecast values.

3. Inputs retain route IDs, segments where supported, forecast issue/valid times, provider, units and error flags.

4. Travel-assessment service generates a documented weather/road advisory for each route.

5. Where trustworthy vehicle-health values are present, the health-aware configuration additionally considers them; where absent, the output explicitly states environmental-only assessment.

6. UI shows candidate options, reasons, warnings and possibly “postpone” or “insufficient information.” It does not claim a recommended route is guaranteed safe.

The planned evaluation uses the same scenario IDs and candidate routes for both configurations, changes only the availability of vehicle-health inputs, and compares outputs to independently reviewed reference suitability assessments. Constructed scenarios must be labeled as constructed, not real journeys.

### 4.4 Remote-control capability and demonstration

1. UI queries the capability adapter for the vehicle/provider.

2. Each function is marked Available, Unavailable, or Not configured.

3. In the initial prototype, any demonstration is clearly marked Mock/Sandbox and cannot actuate a real vehicle.

4. Later real integration would require verified ownership/authorization, provider-approved interface, command-specific safety checks, explicit confirmation, and status reconciliation. A submitted command is never automatically labeled executed.

### 4.5 Blizzard Survival Mode — detection, check-in and escalation

1. A driver explicitly enables a safety-monitoring session and configures emergency contacts. Contacts must accept an invitation before alerts may be sent to them. A session must clearly show whether monitoring is active or limited by permissions, power or connectivity.
2. The mobile detector evaluates *available* speed, time stationary (initial example threshold: 20 minutes), engine running/recently running when known, outside temperature (OBD-II when supported or weather API), location outside known safe places/city areas, and optional severe-weather context. These conditions are proposed and must be validated; missing signals are not silently treated as proof of danger.
3. Potential danger moves the session from `Normal` to `Suspected` and triggers a check-in with audible/visual cues: I'm fine (temporarily snooze), I need help (immediate escalation), or no response (repeat after a configurable interval, initially 2–3 minutes). While active, periodic follow-up check-ins are planned (e.g., 30–60 minutes), subject to platform constraints.
4. The mobile client sends timestamped heartbeats and last-known status/location while connectivity is available. The backend monitors the last *received* heartbeat and authorization/session state. An overdue heartbeat is treated only as possible loss of contact, not confirmation that the driver is stranded.
5. If escalation is warranted, the backend attempts a provider notification to opted-in contacts containing a map link, observation/alert time, last movement time when known, outdoor temperature and phone battery level when known, and clear wording such as No response to a safety check-in. The alert identifies readings as last-known rather than live when stale.
6. Outgoing events use explicit statuses such as `queued`, `sent_to_provider`, `failed`, and `delivered` only when provider acknowledgment truly supports delivery confirmation. With no connectivity, the mobile client may queue alerts locally for later retry; server-side escalation can use only data and heartbeats previously received by the server.
7. The app provides a manual Call 112 action; it never automatically calls emergency services. Location sharing is limited to authorized safety alerts; retention, encryption and deletion settings require final policy definition.

Prototype boundaries: Detection and check-ins may be demonstrated with clearly labeled simulated OBD/GPS/weather inputs and test opt-in contacts through a test Telegram bot. SMS gateway integration, unattended native SMS, iOS continuous background operation, and real-life rescue performance are not asserted. Android foreground service behavior and iOS restrictions must be investigated and documented. Safety sessions must not claim to remain active if the OS has stopped monitoring.

## 5. Planned interface and data contracts

These are design-level example fields, not running endpoints or confirmed external API schemas.

| Data object | Minimum planned fields | Validation/provenance requirements |

|---|---|---|

| VehicleProfile | `vehicle_id`, `make`, `model`, `model_year`, `display_name` | Stable local ID; required-field checks; avoid publishing real VINs |

| TelemetryRecord | `record_id`, `vehicle_id`, `observed_at`, `source_type`, `measurements`, `units`, optional `dtcs` | Source is `sample`, `replay`, or verified `connected`; units and timestamps explicit; unsupported readings marked missing |

| DiagnosticAssessment | `assessment_id`, `record_id`, `model_or_rule_version`, `label_or_score`, `explanation`, `limitations` | Distinguish observed code, derived threshold, and ML prediction |

| MaintenanceEvent | `event_id`, `vehicle_id`, `date`, `service_type`, `mileage_or_reason` | Editable, removable, input validated |

| TripRequest | `scenario_id`, `origin`, `destination`, `departure_time`, optional `vehicle_id` | Precise date/time and scenario origin; minimize retained location data |

| RouteAssessment | `scenario_id`, `route_id`, `configuration`, `risk_factors`, `recommendation`, `reason`, `source_timestamps` | Configuration is environmental-only or vehicle-aware; record insufficient inputs |

| SafetySession | `session_id`, `user_id`, `state`, `started_at`, `last_heartbeat_received_at`, `last_known_location`, `last_movement_at`, `last_temperature`, `monitoring_limits` | States and source freshness explicit; only store necessary sensitive data with access controls |
| EmergencyContact / Alert | `contact_id`, `opt_in_status`, `alert_id`, `session_id`, `reason`, `created_at`, `delivery_status`, `last_location_timestamp` | Do not transmit to non-consenting contacts; provider delivery vs successful request clearly differentiated |
| ControlCapability / Command | `vehicle_id`, `command_type`, `capability_state`, `execution_mode`, `confirmation`, `provider_status` | Mode clearly `mock` or verified `authorized`; no unsupported commands |

The Node.js runtime is selected. The finalized API endpoints and persistence schema will be documented after choosing the Node.js framework, database, and provider integrations.

## 6. Data storage and confidentiality

- Protect precise location, contacts, opt-in history and safety-session data with access controls and appropriate retention/deletion rules. A contact invitation alone does not constitute opt-in.
- Separate user-facing records from technical logs. Do not include access tokens, credentials, personal trip histories, or raw identifiers in committed fixtures or screenshots.

- Store only required profile/maintenance data and input provenance; document retention and deletion behavior before implementation.

- Keep sample/replay records explicitly distinguishable from actual telemetry.

- Keep raw input data apart from processed evaluation outputs so scenarios and recommendations remain traceable.

- Place secrets outside Git and use `config/.env.example` with placeholder values. `.gitignore` is necessary but not sufficient to protect already committed secrets.

## 7. Quality attributes and failure behavior

| Concern | Planned architectural response | Verification basis |

|---|---|---|

| Safety and honest output | Unknown codes and missing/old data remain unknown; advisory language, reasons and limitations included | FR-05, FR-17–20, FR-25 |

| Security | Authorized provider boundary, confirmation before actions, mock-only initial implementation, least privilege | FR-22–24; NFR-04–05 |

| Emergency escalation and false positives | Missing mobile heartbeat is ambiguous; check-in/snooze, configurable timers, opt-in, explicit statuses, offline queue and server-side stale-state handling | Blizzard-related FR/NFR; simulation with failure cases |
| Reliability | Explicit network/provider error, retry where appropriate, stale indicators, no fabricated fallback weather | FR-17, FR-25; NFR-07 |

| Traceability | Record scenario ID, provenance, decision configuration, rule/model version and evaluation output | NFR-03 |

| Testability | Provider adapters may be replaced by deterministic fixtures; units/services tested independently | NFR-08, NFR-12 |

| Performance | Proposed response-time targets measured later with defined devices, datasets and network conditions | NFR-09 |

| Maintainability | Distinct UI, service, adapter and model responsibilities; change decisions recorded in ADRs | NFR-12 |

## 8. Intended deployment structure and repository alignment

```text

src/

  mobile/       # Flutter app: planned, currently may contain README placeholder

  backend/      # Node.js API and application services; framework not yet finalized

  ml/           # Diagnostic training/inference assets: not implemented yet

docs/

  requirements/requirements-specification.md

  architecture/architecture-description.md

  architecture/architecture-decisions.md

diagrams/

  source/       # editable .drawio files

  exports/      # matching .png/.svg exports

tests/          # test plan, fixtures, automated checks when implemented

config/         # public examples only; live credentials never committed

```

This is a logical mapping, not evidence that the services, diagrams or tests already exist. Whether backend and model inference run on one server or separate processes remains an open deployment decision.

## 9. Dependencies, open issues, and next verification steps

| Item | Current status / required next decision |

|---|---|

| Mobile application | Flutter is planned; supported OS/device target must be documented |

| Backend and persistence | Node.js runtime selected; compare framework, hosting, scheduler, and persistence alternatives |

| Automotive data | Verify dataset license, input features and vehicle compatibility; do not claim live OBD-II access |

| Model | Define exact diagnostic target, dataset, baseline, held-out evaluation and explainability strategy |

| Weather and routing APIs | Verify coverage, quota, permission and freshness; choose providers |

| Blizzard Survival Mode | Define candidate thresholds, opt-in flow, session-state machine, heartbeat cadence, provider selection, offline behavior, OS/background limitations and test scenarios |
| Remote commands | Keep sandbox mode unless authorized OEM/service integration can be verified and safely tested |

| Independent route reference | Define rubric and obtain independent review before measuring agreement |

| UX and diagrams | Create/edit/export the three required diagrams and cross-check them against requirement IDs |

| Implementation evidence | Add test IDs, screenshots, measured outputs and review approvals only after actual execution |

## 10. Traceability to architectural decisions

See [`architecture-decisions.md`](architecture-decisions.md) for proposed choices, unresolved alternatives, consequences and required confirmations. Architectural revisions must reference affected requirement IDs and be tracked through a reviewed Git pull request and YouTrack task.

Current completion statement: This document defines an initial, reviewable architecture; it does not assert that code, external integrations, model accuracy, production security, or practical safety have been verified.
