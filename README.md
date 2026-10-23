# JolSense

## ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control

JolSense is a planned mobile application for personal vehicle owners.
It combines vehicle monitoring, ML-supported diagnostics, maintenance
guidance, weather-aware route assessment, supported remote controls,
and Blizzard Mode safety check-ins.

Status: documentation and UX prototyping.
Implementation, integrations, and experimental evaluation are pending.

## Problem Statement

Drivers may struggle to interpret diagnostic trouble codes and assess
how vehicle condition, weather, and road information affect a journey.

Relevant information is often distributed across separate applications
and services. JolSense aims to bring these inputs together and provide
understandable explanations and decision support.

A further planned function, Blizzard Mode, will support safety check-ins
during potentially hazardous stops in cold or adverse-weather conditions.

## Target Users

The intended users are ordinary owners and drivers of personal vehicles.

User needs and priorities still require validation through discussions
with potential users.

## Project Aim

To develop a mobile vehicle-assistance prototype that integrates
monitoring, ML-supported diagnostics, maintenance guidance, weather-aware
trip assessment, and supported remote-control functions.

The research evaluation will investigate whether adding vehicle-health
indicators improves route-recommendation appropriateness compared with
using weather and road information alone.

## Objectives

1. Identify suitable automotive data and available OBD-II indicators.
2. Display supported vehicle readings with units, timestamps,
   and availability status.
3. Define a diagnostic task and evaluate a Random Forest model
   against an appropriate baseline.
4. Explain recorded fault codes and model assessments in plain language.
5. Provide maintenance guidance and reminders.
6. Obtain weather information for relevant locations and travel times.
7. Compare candidate routes using available weather, road,
   and vehicle-condition information.
8. Support remote commands only through authorized, compatible interfaces.
9. Prototype Blizzard Mode check-ins and contact-notification workflows.
10. Evaluate the implemented functions using documented tests
    and reproducible scenarios.

## Planned Features

### Vehicle Monitoring

- Read supported vehicle indicators through an OBD-II adapter.
- Display available readings, units, and timestamps.
- Identify missing, stale, or unsupported measurements.
- Keep observed readings separate from inferred assessments.

Adapter selection, vehicle compatibility, and supported indicators
remain to be verified.

### ML-Supported Diagnostics

- Use Random Forest for a diagnostic task supported by the selected data.
- Keep recorded diagnostic trouble codes separate from ML predictions.
- Present model limitations and uncertainty.
- Evaluate performance using metrics appropriate to the final task.

The dataset, target labels, prediction horizon where relevant,
and final model configuration have not yet been selected.
Predictive-maintenance claims will depend on the available labels
and evaluation evidence.

### Maintenance Assistance

- Explain diagnostic information in accessible language.
- Provide suggested checks and maintenance guidance.
- Support maintenance records and reminders.
- Avoid presenting possible causes as confirmed diagnoses.

### Weather-Aware Trip Assessment

- Obtain weather information for selected route locations and times.
- Retrieve candidate routes and available road information.
- Compare route suitability using documented criteria.
- Explain the factors behind each assessment.
- Communicate missing or outdated information.

Weather and routing providers remain to be selected.
Recommendations will support driver decisions and will not guarantee
that a route is safe.

### Supported Remote Controls

Planned functions include door locking and unlocking.

Engine start, boot opening, and window control will be considered
only where a compatible, authorized integration is available.

OBD-II telemetry access alone does not establish permission or capability
to perform these commands. Unavailable functions will be identified
clearly in the interface.

### Blizzard Mode

Blizzard Mode is a planned, user-enabled safety-session feature.

The proposed workflow is:

1. Explain location sharing and obtain the required permissions.
2. Configure emergency contacts who have agreed to receive alerts.
3. Register the safety session with the backend.
4. Show server monitoring as active only after registration succeeds.
5. Assess potentially hazardous stops using available movement,
   vehicle, location, and weather information.
6. Ask the driver to confirm whether they are OK.
7. Allow the driver to confirm, pause monitoring, request help,
   or end the session.
8. Request contact notification when the configured escalation
   conditions are met.

During an active session, the application is planned to send periodic
heartbeats and the latest available location to the backend.
The backend will monitor registered sessions and overdue communication.

A missing heartbeat indicates lost communication, not a confirmed
emergency. Alerts must state this uncertainty and identify the timestamp
of the last known location.

Telegram is a proposed prototype notification channel.
SMS is a possible future extension. Neither is implemented yet.

Rules, thresholds, intervals, retries, and cancellation behavior
require definition and testing. Blizzard Mode will use documented
rules and timers rather than the diagnostic Random Forest model.

JolSense is not a rescue service and will not automatically dispatch
emergency responders.

## Technology Choices

| Area | Current choice | Remaining decisions |
|---|---|---|
| Mobile application | Flutter | Target platforms, versions, and compatible plugins |
| Backend | Node.js | Framework, hosting, authentication, and scheduled worker |
| ML diagnostics | Random Forest | Dataset, target, library, parameters, and serving approach |
| Vehicle monitoring | OBD-II | Adapter, connection type, vehicle support, and indicators |
| Weather information | External API planned | Provider, coverage, parameters, and permitted use |
| Routing | External API planned | Provider, route attributes, and coverage |
| Data storage | Not selected | Database and retention configuration |
| Contact notifications | Telegram proposed | Provider configuration and delivery verification |
| Version control | GitHub | Continued use of branches and reviewed pull requests |
| Task tracking | YouTrack | Maintain tasks, responsibilities, milestones, and risks |

Python may be used for model training or inference where appropriate.
The integration approach remains to be confirmed; it does not replace
Node.js as the selected application backend.

## Data Preparation

We plan to use a publicly available automotive dataset, subject to
checking its suitability, alongside weather and route information
from APIs.

Data preparation will include:

- Checking source accessibility, documentation, and permitted use.
- Identifying relevant variables, labels, units, and timestamps.
- Handling duplicates, missing values, and invalid records.
- Recording data provenance and retrieval information.
- Separating raw inputs from processed data and evaluation outputs.
- Preventing related observations from leaking across evaluation splits.

Independently sourced vehicle and environmental records may be combined
into explicitly labelled constructed scenarios. They will not be
presented as observations from the same real journey.

Simulation and replay data may support prototype testing.
Real vehicle integration will be evaluated only after compatibility
and access are confirmed.

## Verification and Evaluation

### Functional Verification

Tests will cover the implemented workflows, including:

- Available and missing vehicle readings.
- Diagnostic explanations and model-output presentation.
- Maintenance records and reminders.
- Weather and route requests.
- Unsupported remote controls and authorization checks.
- API failures and unavailable services.
- Blizzard Mode session registration, check-ins, pause, cancellation,
  heartbeat loss, and notification outcomes.

Mock behavior will be distinguished from real integrations.

### Diagnostic Model Evaluation

Random Forest will be compared with an appropriate simple baseline
using the same data partitions and preprocessing conditions.

Metrics will match the selected task. Classification may use precision,
recall, F1-score, and a confusion matrix; regression will require
appropriate error measures.

The final target, split strategy, metrics, and acceptance criteria
must be documented before the final evaluation.

### Route-Recommendation Comparison

Two configurations are planned:

1. Baseline: weather and road-condition inputs.
2. Combined: the same inputs plus supported vehicle-health indicators.

Both will process the same retained scenarios and candidate routes.
Reference suitability assessments will be independently reviewed
before system outputs are examined.

The planned primary metric is recommendation agreement rate with
the reference assessments. Unsuitable-route selection rate is
a secondary metric.

This comparison will evaluate recommendation appropriateness.
It will not demonstrate actual crash reduction.

### Blizzard Mode Evaluation

Testing will use safe, constructed scenarios rather than placing
participants in dangerous weather.

Scenarios will include prolonged stops, ordinary rest stops,
driver responses, lost connectivity, backend restarts, and
notification failures.

Measures will include detection and escalation delays, false alerts
within the tested scenarios, and notification submission or delivery
outcomes where supported.

A submitted notification will not be treated as proof that a contact
received or read it.

No experimental results are available yet.

## Privacy and Responsible Use

- Collect only information required for the selected functions.
- Explain location sharing before activating a safety session.
- Share alerts only with opted-in contacts.
- Restrict access to telemetry, contact details, and location records.
- Define retention and deletion rules before collecting personal data.
- Keep credentials, API keys, and identifiable logs out of Git.
- Use anonymized or synthetic examples in public documentation.
- Avoid sending sensitive data to external AI or cloud tools
  without an appropriate basis and safeguards.

## Documentation

| Material | Location |
|---|---|
| Project proposal | [Project proposal](docs/proposal/project-proposal.md) |
| Literature review | [Literature review](docs/literature-review/literature-review.md) |
| Literature matrix | [Literature matrix](docs/literature-review/literature-matrix.xlsx) |
| Existing systems comparison | [Existing systems](docs/existing-systems/existing-systems-comparison.md) |
| Technology comparison | [Technology comparison](docs/technology-comparison/technology-comparison.md) |
| Requirements | [Requirements specification](docs/requirements/requirements-specification.md) |
| Architecture | [Architecture description](docs/architecture/architecture-description.md) |
| Architecture decisions | [Architecture decisions](docs/architecture/architecture-decisions.md) |
| UX prototype materials | [UX prototype](docs/ux-prototype/README.md) |

## Diagrams

| Diagram | Editable source | Image |
|---|---|---|
| System Context | [DRAWIO](diagrams/source/system-context.drawio) | [PNG](diagrams/exports/system-context.png) |
| Use Case | [DRAWIO](diagrams/source/use-case.drawio) | [PNG](diagrams/exports/use-case.png) |
| Component Diagram — draft | [DRAWIO](diagrams/source/component-diagram.drawio) | [PNG](diagrams/exports/component-diagram.png) |

Diagrams describe the planned system. They do not demonstrate
completed integrations.

## UX Prototype

The repository contains interface screenshots with illustrative data.

They do not demonstrate working vehicle communication, model inference,
weather requests, remote commands, or emergency-message delivery.

The shared prototype link and navigation verification status
are maintained in [the UX README](docs/ux-prototype/README.md).

## Team and Responsibilities

| Team member | Main implementation responsibilities |
|---|---|
| Shurenova Karima | Vehicle monitoring, supported remote-control integration, weather-aware route assessment, and mobile functionality |
| Yerbolat Aisha | ML-supported diagnostics, diagnostic explanations, maintenance assistance, and personalized maintenance planning |

Shared work includes integration, Blizzard Mode coordination,
evaluation, documentation consistency, and preparation for defense.
Detailed tasks and ownership will be maintained in YouTrack.

Supervisor: Symbat Nurgalieva  
Group: SE-2405

## Project Management

The project uses GitHub for version control and YouTrack for task tracking.

Documentation and implementation changes should be linked to relevant
tasks where possible and reviewed through pull requests.

Planned milestones are:

1. Review requirements, literature, architecture, and UX materials.
2. Validate data suitability and integration feasibility.
3. Implement core monitoring and backend communication.
4. Develop and evaluate the diagnostic model.
5. Integrate trip assessment and prototype Blizzard Mode.
6. Complete functional tests and comparative evaluation.
7. Finalize documentation and defense materials.

Dates and individual task status will be recorded in YouTrack.

## Main Risks and Dependencies

| Risk or dependency | Planned response |
|---|---|
| Insufficient diagnostic data | Check labels and relevance before committing to a prediction task |
| Limited OBD-II compatibility | Verify supported indicators and use labelled replay data where needed |
| Restricted remote-control access | Limit commands to authorized interfaces and show unavailable capabilities |
| Incomplete weather or road coverage | Show source limitations and insufficient-information outcomes |
| Mobile background restrictions or lost connectivity | Test supported operating modes and registered-session heartbeat behavior |
| False safety alerts | Test rules, check-ins, pause, cancellation, and escalation conditions |
| Notification failure | Record status, apply controlled retries, and avoid claiming guaranteed delivery |

## Repository Structure

- `docs/` — proposal, reviews, requirements, architecture, and UX materials.
- `diagrams/source/` — editable diagram files.
- `diagrams/exports/` — readable diagram images.
- `src/mobile/` — planned Flutter application.
- `src/backend/` — planned Node.js backend.
- `src/ml/` — planned model development and inference materials.
- `tests/` — testing materials.
- `config/` — configuration templates.

## Setup and Execution

Runnable application setup is not available yet.

Installation commands, dependency versions, environment variables,
API configuration, and test instructions will be documented
when implementation is available.

Use configuration templates for required settings.
Never commit real secrets or credentials.

## Current Status and Next Steps

The repository contains project documentation, UX screenshots,
and draft architectural diagrams.

Implementation and evaluation remain pending.
Before submission, the team must check:

- Consistency of technology choices across documents.
- A populated literature matrix aligned with the review and references.
- Removal of excluded sources.
- Prototype access and working documentation links.
- A complete verification plan and traceable project milestones.
- Evidence of user needs and actual supervisor feedback.

## Expected Outcomes

- A Flutter mobile application prototype with a Node.js backend.
- Monitoring of supported vehicle indicators.
- A reproducible Random Forest diagnostic experiment.
- Understandable diagnostic and maintenance guidance.
- Weather-aware route assessment and a paired configuration comparison.
- Authorized remote-control workflows where feasible.
- A tested Blizzard Mode prototype under documented scenarios.
- Reproducible documentation of methods, limitations, and results.
