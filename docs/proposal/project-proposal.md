Copy this into `docs/proposal/project-proposal.md`:

```markdown
# Project Proposal

**Project title:** ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control

**Application name:** JolSense  
**Team members:** Shurenova Karima and Yerbolat Aisha  
**Group:** SE-2405  
**Supervisor:** Symbat Nurgalieva  
**Status:** Draft for supervisor review

## 1. Problem Statement

Owners of personal vehicles need to understand their vehicle's condition,
respond to diagnostic warnings, and consider weather and road conditions
when planning a journey. Sensor readings and diagnostic trouble codes
may be difficult to interpret without technical knowledge. Drivers must
also decide whether the available vehicle and environmental information
requires action before or during a trip.

A further concern is a prolonged stop in a remote area during cold or
adverse weather. Such a stop does not necessarily indicate an emergency,
but an unanswered safety check-in may justify notifying a trusted contact.

JolSense is a proposed mobile application that combines OBD-II monitoring,
ML-based diagnostic assistance, understandable explanations, maintenance
guidance, weather-aware route assessment, supported remote commands,
and a safety check-in feature called Blizzard Mode.

These functions represent the proposed scope. User needs and priorities
will be validated through discussions with potential users. No completed
user discussions or measured safety improvements are claimed here.

## 2. Target Users

The primary users are owners and drivers of personal vehicles who want to:

- Monitor available vehicle-state indicators.
- Understand diagnostic warnings.
- Prepare for maintenance.
- Review weather-related journey risks.
- Compare the suitability of candidate routes.
- Use supported remote vehicle commands.
- Enable safety check-ins during potentially difficult journeys.

Blizzard Mode will also involve emergency contacts who explicitly
agree to receive notifications.

## 3. Project Aim

To develop and evaluate a mobile application that brings vehicle
monitoring, ML-based diagnostics, weather-aware journey assessment,
supported remote control, and safety check-ins into one interface
for owners of personal vehicles.

## 4. Objectives

1. Review relevant scientific literature and compare existing automotive
   systems using consistent criteria.

2. Validate user needs and define functional and non-functional
   requirements with verifiable acceptance criteria.

3. Establish an OBD-II data-acquisition workflow and identify the
   parameters supported by the selected vehicle and adapter.

4. Select a documented automotive dataset suitable for a clearly
   defined diagnostic or future-fault prediction task.

5. Train and evaluate a Random Forest model against an appropriate
   baseline while preventing data leakage.

6. Present diagnostic findings and maintenance guidance in
   understandable language.

7. Integrate weather and routing information to provide journey
   warnings and compare candidate routes.

8. Evaluate whether vehicle-health information improves recommendation
   suitability compared with weather and road-condition information alone.

9. Implement supported remote commands where a compatible,
   authorized control interface is available.

10. Develop Blizzard Mode with safety check-ins, server-side heartbeat
    monitoring, and notifications to opted-in emergency contacts.

11. Test the implemented functions and document compatibility,
    communication failures, uncertainty, and other limitations.

## 5. Proposed Solution

JolSense will consist of a Flutter mobile application and a Node.js
backend. Random Forest is the selected initial model for the
vehicle-diagnostics experiment.

The application will obtain supported vehicle readings through an
OBD-II adapter. External services will provide weather forecasts and
candidate routes. A separate authorized integration will be required
for remote vehicle commands.

Blizzard Mode will initially use documented rules and timers rather
than the diagnostic ML model. The backend will monitor registered
safety sessions and initiate contact notifications when the configured
conditions are met.

Specific hardware, API providers, ML libraries, deployment services,
and operating-system coverage remain to be selected or validated.

## 6. Planned Functions

### 6.1 Vehicle Monitoring

The application will display supported vehicle readings, their units,
timestamps, and connection status.

Candidate indicators include engine speed, vehicle speed, coolant
temperature, and battery-related readings where available. The final
set will depend on the selected vehicle and adapter.

Unavailable or outdated readings will be identified explicitly rather
than interpreted as normal conditions. Battery voltage alone will not
be presented as a complete assessment of battery health.

### 6.2 Diagnostic Assistance and ML

The application will read supported diagnostic trouble codes and
provide understandable explanations of their meaning.

A Random Forest model will be developed for a defined task supported
by the selected dataset. Future-fault prediction will require earlier
measurements linked to later fault outcomes and a specified prediction
horizon.

If the available data support only current-fault classification, the
component will be described as diagnosis rather than future prediction.
Any resulting scope change will be documented.

Diagnostic codes and model outputs will support decisions; they will
not establish a confirmed root cause or replace professional inspection.

### 6.3 Maintenance Assistant

The application will provide maintenance reminders and guidance based
on available vehicle information and documented maintenance inputs.

Guidance will distinguish recorded information, model estimates, and
recommended checks. It will avoid presenting uncertain findings as
confirmed mechanical failures.

### 6.4 Weather-Aware Journey Assessment

Users will specify a destination and departure time. With permission,
the application will obtain the starting location from the phone.

The routing service will return candidate routes. Weather forecasts
will be requested for selected locations along those routes and,
where supported, their estimated passage times.

The application will present relevant weather warnings and compare
route suitability using documented criteria and available vehicle
information.

Weather forecasts will not be treated as direct measurements of road
surface conditions. Road closures, surface conditions, and traffic
events will be used only when a suitable source provides them.

Route recommendations will express estimated suitability rather than
guarantee that a route is safe.

### 6.5 Supported Remote Control

Door locking and unlocking are planned. Engine start, boot opening,
and window control will depend on authorized access and compatibility.

OBD-II monitoring will not be assumed to provide these capabilities.
Remote commands will require a separate supported control interface.

The application will distinguish a requested command, a confirmed
result, an unsuccessful command, and an unknown outcome.

If integration access is unavailable, clearly labelled simulated
responses may demonstrate the interface. These demonstrations will
not be reported as successful control of a real vehicle.

### 6.6 Blizzard Mode

Blizzard Mode is a planned, user-enabled safety check-in feature for
potentially concerning prolonged stops in cold or adverse weather.

During an enabled session, the application will assess available
information such as:

- Duration of the stop.
- Recent vehicle movement and available engine-state information.
- Outside temperature from a supported vehicle reading or weather API.
- Location relative to user-saved places.
- Available settlement-boundary information.
- Relevant weather warnings, where provided.

A prolonged stop is only a possible concern, not proof that the user
is stranded. Missing or outdated inputs will not be treated as
confirmed hazardous conditions.

Detection rules, thresholds, geographic coverage, and timing values
will be defined and tested before being finalized.

#### Safety Check-In

When the configured conditions are met, the application will request
a response using a notification, sound, and vibration where permitted.

The user will be able to:

- Confirm “I'm fine.”
- Pause further checks for a selected period.
- Select “I need help.”
- End the safety session.

If checks remain unanswered after the configured repeat and response
periods, the backend will initiate an alert to opted-in contacts.
Selecting “I need help” will initiate the alert without waiting for
the unanswered-check-in deadline.

#### Server-Side Heartbeat

While the safety session is active, the application will send periodic
status updates to the backend.

Server monitoring will be shown as active only after the backend
acknowledges session registration. The server will retain the latest
received status and location timestamp.

A missed heartbeat will indicate a loss of communication, not a
confirmed emergency. Following the configured escalation policy,
the server may notify contacts that communication or check-ins were
missed and provide the last known information.

If the phone loses connectivity before the session is registered,
server-side monitoring cannot be assumed to be active.

#### Contact Notifications

The initial proposed delivery channel is a Telegram bot. SMS is a
possible later extension and is not part of the confirmed integration.

An alert may contain:

- The reason for the notification.
- Last known coordinates and a map link.
- The coordinate timestamp.
- Last recorded movement time.
- Available outside-temperature information and its timestamp.
- Phone battery level, if available.

The system will distinguish pending, submitted, and failed delivery
states. Provider acceptance will not be treated as proof that a
person has read the message.

The application will not automatically contact emergency services
or claim that assistance has been dispatched.

## 7. Data Sources and Preparation

### 7.1 Vehicle Data

Operational monitoring is planned through an OBD-II adapter. The team
will record the test vehicle, adapter, connection method, supported
parameters, units, and sampling configuration.

A publicly available automotive dataset will support initial ML
development, subject to checking documentation, permitted use,
labels, data quality, and task suitability.

### 7.2 Environmental and Route Data

Weather and routing APIs will provide information for selected
locations and times. Providers have not yet been selected.

Retrieved records will retain their source, retrieval time, relevant
forecast or observation time, and units.

### 7.3 Constructed Scenarios

Independently sourced automotive and environmental records may be
combined into experimental scenarios.

These scenarios will be labelled as constructed combinations rather
than observations from the same real journey.

Blizzard Mode will be tested using simulated stops, communication
failures, and test contacts. Participants will not be required to
become stranded or drive in hazardous conditions.

## 8. Scope and Boundaries

### Included

- Mobile application prototype.
- OBD-II monitoring for a documented supported configuration.
- A defined Random Forest diagnostic experiment.
- Diagnostic explanations and maintenance guidance.
- Weather-related journey warnings.
- Candidate-route suitability comparison.
- Supported remote-control integration or labelled demonstration.
- Blizzard Mode prototype and controlled scenario testing.
- Documentation, diagrams, interface mockups, and test materials.

### Outside the Current Scope

- Universal compatibility with all vehicles and adapters.
- Autonomous driving or automatic steering and braking.
- Guaranteed fault detection, safe travel, or rescue.
- Automatic emergency-service dispatch.
- Fuel-based survival-time estimates.
- Camera-based driver drowsiness detection.
- Unsupported remote commands or bypassing vehicle access controls.

## 9. Technical Approach

| Component | Selected approach | Pending details |
|---|---|---|
| Mobile application | Flutter | Target platforms, packages, and device testing. |
| Backend | Node.js | Framework, storage, hosting, and scheduled worker. |
| ML diagnostics | Random Forest | Dataset, target, training library, and serving method. |
| Vehicle connection | OBD-II adapter | Adapter, connection protocol, and supported vehicle. |
| Weather and routing | External APIs | Providers, coverage, limits, and terms of use. |
| Blizzard Mode | Rules, check-ins, and server timers | Thresholds, background support, and escalation policy. |
| Contact notifications | Telegram proposed for the prototype | Bot integration and consent workflow. |
| Remote commands | Authorized compatible interface | Provider access and command coverage. |

If the ML model is developed in Python, a separate prediction service
is a possible integration approach. This decision remains pending.

Background execution and notification behavior must be tested on the
selected operating systems. Server monitoring does not remove mobile
platform restrictions.

## 10. Evaluation Plan

### 10.1 Functional Testing

Test cases will verify mandatory requirements and their acceptance
criteria, including:

- Supported and unsupported vehicle readings.
- Invalid inputs and outdated data.
- OBD-II connection loss and recovery.
- External API failures.
- Remote-command outcomes.
- Safety-session registration and cancellation.
- Contact-consent handling.
- Notification retries and duplicate prevention.

### 10.2 ML Evaluation

The Random Forest model will be compared with a suitable simple
baseline.

Related records will remain within the same data partition, and
temporal order will be preserved when evaluating future prediction.
Preprocessing will be fitted using training data only.

Metrics will match the final task. Classification evaluation may
include precision, recall, F1-score, PR-AUC, and a confusion matrix.
Regression evaluation will use appropriate error measures.

Errors, class imbalance, dataset limitations, and generalization
constraints will be reported.

### 10.3 Journey-Recommendation Evaluation

Two configurations will process the same retained scenarios:

- Baseline: weather and road-condition information.
- Combined configuration: the same information plus vehicle-health
  indicators.

Candidate routes and shared settings will remain unchanged.

Outputs will be compared with independently reviewed reference
assessments established before examining system recommendations.
The evaluation will assess recommendation suitability, not actual
crash reduction.

### 10.4 Blizzard Mode Evaluation

Controlled scenarios will include:

- A qualifying prolonged stop in cold weather.
- An ordinary rest stop.
- A stop at a saved place.
- Missing or outdated location and weather inputs.
- A successful “I'm fine” response.
- A manual help request.
- Unanswered check-ins.
- Heartbeat loss after successful session registration.
- A pause or cancellation near an escalation deadline.
- Network restoration and notification retry.

Measures will include detection delay, false alerts and missed
qualifying cases within the constructed scenarios, escalation timing,
and notification delivery outcomes.

All results will distinguish simulated inputs from real integration
tests. No claim of real-world accident reduction will be made from
these tests alone.

## 11. Privacy and Responsible Data Use

Location collection and safety-session monitoring will require clear
user permission. Emergency contacts must opt in and be able to
withdraw.

During an active safety session, location may be transmitted to the
backend as part of status updates. Disclosure to emergency contacts
will occur only under the configured alert conditions.

The application will explain what is collected, why it is needed,
and who may receive it. Storage access will be restricted, and
credentials will not be committed to the repository.

Retention periods, deletion procedures, and external-service data
handling will be documented before real-user testing.

## 12. Expected Outcomes

- A Flutter mobile application prototype.
- A Node.js backend supporting the implemented functions.
- OBD-II monitoring for a documented configuration.
- A reproducible Random Forest diagnostic experiment.
- Understandable diagnostic and maintenance information.
- Weather-aware journey warnings and route comparison.
- Supported remote commands or labelled demonstrations.
- A Blizzard Mode prototype with check-ins, server monitoring,
  and test-contact notifications.
- Requirements, architecture diagrams, UX mockups, and evaluation
  documentation.
- A version-controlled repository and task tracker recording
  individual contributions.

## 13. Risks and Open Decisions

| Risk or open decision | Planned response |
|---|---|
| Limited OBD-II support | Publish tested parameters and identify unavailable readings. |
| Dataset does not support future prediction | Define an evidence-supported task and document any scope change. |
| No authorized remote-control access | Use a labelled demonstration and report the integration limitation. |
| Weather or road-data gaps | Show missing information and avoid unsupported safety conclusions. |
| False Blizzard Mode alerts | Test ordinary stops, user pauses, and configurable detection rules. |
| Background restrictions or connectivity loss | Test supported operating modes and clearly display monitoring status. |
| Notification failure | Record delivery state, retry appropriately, and prevent duplicates. |
| Growing implementation scope | Prioritize requirements and agree on the deliverable boundary with the supervisor. |

## 14. Current Status

The intended users, application name, OBD-II monitoring approach,
and initial technology choices have been identified.

Blizzard Mode has been added to the proposed scope. Camera-based
driver monitoring is not included.

Dataset validation, integration testing, user discussions, final
acceptance thresholds, and supervisor review remain pending.
```