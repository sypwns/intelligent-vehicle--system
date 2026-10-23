# Technology Comparison

**Project:** JolSense  
**Status:** Initial technology selection; implementation validation pending

## 1. Purpose

This document compares alternatives for three key engineering decisions:

1. Mobile application development.
2. Backend development.
3. Machine-learning-based vehicle diagnostics.

The team has selected Flutter, Node.js, and Random Forest.

The comparison explains the intended role, advantages, limitations,
and validation requirements of each selection. It does not report
project-specific performance measurements.

Blizzard Mode is included in the proposed architecture. It will
initially use documented rules and timers, separately from the
Random Forest diagnostic model.

## 2. Selection Criteria

Technologies are assessed against the following criteria:

- Fit with the planned functions.
- Mobile and hardware integration.
- Integration with external services and ML.
- Development and maintenance effort.
- Testing and reproducibility.
- Operational limitations.

No numerical ranking is provided because project-specific benchmarks
and team proficiency measurements have not yet been collected.

## 3. Mobile Application Development

### Alternatives

| Criterion | Flutter | React Native | Native Android with Kotlin |
|---|---|---|---|
| Main language | Dart. | JavaScript or TypeScript. | Kotlin. |
| Development approach | Shared application code with platform-specific integration where needed. | Shared application code with native modules where needed. | Android-specific application. |
| Interface development | Flutter widgets. | React components and native platform integration. | Android UI APIs or Jetpack Compose. |
| OBD-II communication | Requires a compatible plugin or platform-specific implementation. | Requires a compatible library or native module. | Can use Android Bluetooth APIs directly. |
| Backend integration | HTTP requests and other supported communication methods. | HTTP requests and other supported communication methods. | HTTP requests and other supported communication methods. |
| Background monitoring | Subject to operating-system restrictions. | Subject to operating-system restrictions. | Subject to Android restrictions, with direct access to platform mechanisms. |
| Main trade-off | Plugins must be checked against the selected adapter and target platforms. | Native dependencies and adapter compatibility require testing. | An iOS version requires a separate implementation. |

### Decision: Flutter

Flutter is selected for the JolSense mobile application.

It supports a shared interface for vehicle monitoring, diagnostics,
maintenance, route assessment, remote commands, and Blizzard Mode.
It also allows platform-specific implementation where a required
integration is not covered by a suitable plugin.

This choice does not establish that every planned function will work
on both Android and iOS.

### Validation Required

Before confirming platform coverage, the team will test:

- Communication with the selected OBD-II adapter.
- Bluetooth permissions and connection recovery.
- Location acquisition and permission handling.
- Notification presentation and user responses.
- Application lifecycle and background behavior.
- Network interruption and recovery.

The adapter's Bluetooth protocol must match the selected integration.
A Bluetooth Low Energy package must not be assumed to support
Bluetooth Classic adapters.

### Alternatives Not Selected

React Native remains a feasible alternative. It was not selected
because Flutter is the team's chosen mobile framework; no experiment
has established a performance advantage over React Native.

Native Kotlin is appropriate for an Android-only implementation.
It was not selected as the primary approach because the team chose
Flutter's shared-code development model.

## 4. Backend Development

### Alternatives

Node.js is a runtime rather than a complete web framework.
For comparison, Express represents a possible Node.js implementation.
The specific Node.js framework remains undecided.

| Criterion | Node.js with Express | Python with FastAPI | Python with Django |
|---|---|---|---|
| Main language | JavaScript or TypeScript. | Python. | Python. |
| Primary approach | API assembled using Express and selected libraries. | API framework with typed request validation and OpenAPI support. | Web framework with ORM, authentication facilities, and administration tools. |
| External integrations | Can coordinate weather, routing, notification, and vehicle-control services. | Can coordinate the same services. | Can coordinate the same services. |
| Python ML integration | Requires a defined bridge or separate prediction service if the model uses Python. | Can integrate a compatible Python model directly or through a separate service. | Can integrate a compatible Python model directly or through a separate service. |
| Blizzard Mode scheduling | Requires a scheduled worker or job-processing mechanism. | Requires a scheduled worker or job-processing mechanism. | Requires a scheduled worker or job-processing mechanism. |
| Main trade-off | Python-based ML may require an additional service. | Would introduce Python as the main backend language instead of the selected Node.js runtime. | Some built-in facilities may exceed the initial API requirements. |

### Decision: Node.js

Node.js is selected for the JolSense backend.

Its planned responsibilities are:

- Provide APIs to the Flutter application.
- Validate incoming requests.
- Coordinate weather and routing requests.
- Exchange data with the diagnostic model.
- Manage safety sessions and emergency-contact consent.
- Process supported remote-command requests.
- Initiate and record notification attempts.

Express is a candidate framework, not a confirmed selection.

### ML Integration

The Random Forest training library has not yet been selected.

If the model is trained in Python, a proposed approach is a separate
Python prediction service called by the Node.js backend.

The interface must define:

- Required input features and units.
- Missing-value handling.
- Preprocessing version.
- Model version.
- Prediction output and uncertainty information where supported.
- Error and timeout responses.

The backend must not report a diagnostic prediction when the
required inputs or prediction service are unavailable.

### Blizzard Mode Scheduling

Registered safety sessions require persistent server-side state.

A scheduled worker will evaluate response and heartbeat deadlines.
The implementation must support:

- Session registration and acknowledgement.
- Last received status and location timestamps.
- Pause, cancellation, and session expiry.
- Configured escalation deadlines.
- Duplicate-alert prevention.
- Notification retries and failure logging.

A temporary in-memory timer alone is insufficient for sessions that
must survive a backend restart.

### Alternatives Not Selected

FastAPI could simplify integration with a Python ML model. It remains
a possible choice for a separate prediction service.

Django provides useful integrated application facilities. It was not
selected as the primary backend because Node.js is the team's
chosen runtime.

These decisions do not imply measured superiority over the alternatives.

## 5. Machine Learning for Vehicle Diagnostics

### Alternatives

| Criterion | Random Forest | XGBoost | Neural Network |
|---|---|---|---|
| Model family | Ensemble of decision trees. | Gradient-boosted decision trees. | Trainable layers; architecture depends on the task. |
| Potential input | Structured vehicle indicators. | Structured vehicle indicators. | Structured indicators or sequences, depending on architecture. |
| Preparation | Consistent feature definitions, encoding, labels, and missing-value handling. | Consistent feature definitions, encoding, labels, and missing-value handling. | Consistent encoding and usually numerical scaling; sequence preparation where applicable. |
| Main tuning choices | Tree count, depth, leaf size, feature sampling, and related settings. | Learning rate, boosting rounds, tree complexity, and related settings. | Architecture, optimizer, learning rate, batch size, and training duration. |
| Explainability | Feature importance and additional explanation methods can be applied. | Feature importance and additional explanation methods can be applied. | Explanation requires methods appropriate to the architecture. |
| Project evidence | Selected initial model; performance not yet measured. | Alternative candidate; performance not yet measured. | Alternative candidate; performance not yet measured. |

### Decision: Random Forest

Random Forest is selected as the initial diagnostic model.

It provides a tree-based approach for examining relationships among
structured vehicle indicators and supports classification or
regression, depending on the final task.

The choice is provisional with respect to dataset suitability.
It does not establish that Random Forest will outperform XGBoost
or a neural network.

### Prediction Task and Data Requirements

The dataset and target must be defined before training.

Future-fault prediction requires measurements linked to later fault
outcomes and a specified prediction horizon.

If the dataset supports only the classification of existing faults,
the result will be described as diagnosis rather than future prediction.

An anonymized benchmark must not be assumed to use the same features
as live OBD-II readings. Deployment requires a verified mapping
between training inputs and available operational inputs.

### Evaluation Plan

- Compare against a suitable simple baseline.
- Keep related vehicle records within the same partition.
- Preserve temporal order for future-prediction evaluation.
- Fit preprocessing using training data only.
- Tune using training and validation data.
- Evaluate the final configuration on held-out test data.
- Analyse errors and limitations.

For classification, candidate metrics include precision, recall,
F1-score, PR-AUC, and a confusion matrix.

For regression, suitable error metrics will be selected after the
target and units are defined.

Feature importance will not be presented as proof of causation.

### Alternatives Not Selected

XGBoost remains a relevant tree-based alternative and may be used
as a benchmark if resources permit.

A neural network remains a candidate if the selected dataset and
task justify its additional architecture and training decisions.

Neither alternative has been rejected on measured performance grounds.

## 6. Blizzard Mode: Detection Approach

### Alternatives

| Approach | Advantages for the proposed prototype | Limitations |
|---|---|---|
| Rules and timers | Explicit conditions, inspectable decisions, and straightforward scenario testing. | Thresholds require validation and may produce false alerts. |
| Supervised ML | Could learn patterns if representative labelled examples are available. | No suitable labelled stranded-event dataset has been selected. |
| User-scheduled check-ins only | Does not require automatic interpretation of vehicle and weather conditions. | Depends on the user setting up checks and does not identify contextual stop patterns. |

### Proposed Decision: Rules and Timers

The initial Blizzard Mode prototype will use documented rules and
timers, together with explicit user control.

Potential inputs include prolonged stopping, recent movement,
available engine-state information, location, and cold or adverse
weather conditions.

Detection thresholds and timing values remain preliminary until tested.

The system will identify a possible concern, not diagnose an emergency.
Missing inputs will be handled explicitly.

The backend must acknowledge session registration before the
application displays server monitoring as active.

A missed heartbeat indicates communication loss. Escalation messages
must distinguish this from an unanswered check-in or manual help request.

## 7. Blizzard Mode: Notification Channel

### Alternatives

| Channel | Potential use | Dependencies and limitations |
|---|---|---|
| Telegram bot | Proposed initial prototype channel. | Contact must join or authorize the bot; delivery requires service connectivity. |
| SMS gateway | Possible later extension. | Provider integration, charges, sender configuration, and delivery coverage must be checked. |
| Mobile push notification | Possible contact-app workflow. | Contact requires an app installation, permission, and a registered notification token. |

### Proposed Decision: Telegram Prototype

Telegram is proposed for initial testing with opted-in test contacts.

It has not yet been integrated. SMS and push notifications remain
alternatives or future extensions.

The application will distinguish:

- Alert pending.
- Submission accepted by the provider.
- Submission failed.
- Recipient acknowledgement, if separately implemented.

Provider acceptance will not be described as proof that a person
has read the alert.

Retries must avoid sending duplicate notifications. Recovery after
network loss and service failure will be included in testing.

## 8. Integration and Privacy Requirements

The selected stack does not remove hardware, platform, or service
constraints.

The implementation must address:

- Explicit permission for location collection.
- Consent and revocation for emergency contacts.
- Restricted access to active safety-session information.
- Location timestamps and data freshness.
- Secure management of credentials.
- Defined retention and deletion procedures.
- Clear separation between real and simulated inputs.

Location may be sent to the backend during an active safety session.
Sharing with emergency contacts occurs under the configured alert
conditions.

OBD-II monitoring and remote commands are separate integrations.
Access to vehicle readings does not establish permission or technical
support for locking doors or starting the engine.

## 9. Selection Summary

| Component | Decision | Status |
|---|---|---|
| Mobile application | Flutter. | Selected by the team; integration testing pending. |
| Backend runtime | Node.js. | Selected by the team; framework undecided. |
| Diagnostic model | Random Forest. | Selected initial model; dataset and task validation pending. |
| Blizzard Mode detection | Rules and timers. | Proposed prototype approach. |
| Heartbeat monitoring | Persistent session state and scheduled backend worker. | Planned; implementation technology undecided. |
| Contact alerts | Telegram bot. | Proposed initial channel; integration pending. |
| SMS | Gateway-based integration. | Possible future extension. |
| Vehicle monitoring | OBD-II adapter. | Adapter and supported configuration undecided. |
| Weather and routing | External APIs. | Providers undecided. |
| Remote commands | Separate authorized interface. | Access and supported commands undecided. |

## 10. Validation Before Finalizing the Stack

The team will perform small integration checks before treating
the selected configuration as operational:

1. Read supported values from the selected OBD-II adapter.
2. Send validated data between Flutter and Node.js.
3. Obtain a model response using a documented input schema.
4. Retrieve weather and candidate-route information.
5. Register, pause, cancel, and expire a safety session.
6. Detect a missed deadline after a backend restart.
7. Submit a test alert and record its actual delivery status.
8. Test background behavior and connection recovery on target devices.

Versions, configuration settings, test conditions, and observed
limitations will be recorded.

Any technology change will be reflected in the architecture,
requirements, dependency files, and decision log.

## 11. Official Documentation

The following documentation supports further implementation review.
Listing a tool does not imply that it is installed or finally selected.

1. Flutter Documentation.  
   https://docs.flutter.dev/

2. Flutter Platform Channels.  
   https://docs.flutter.dev/platform-integration/platform-channels

3. React Native Documentation.  
   https://reactnative.dev/docs/getting-started

4. Android Kotlin Documentation.  
   https://developer.android.com/kotlin

5. Node.js Documentation.  
   https://nodejs.org/en/learn

6. Express Documentation.  
   https://expressjs.com/

7. FastAPI Documentation.  
   https://fastapi.tiangolo.com/

8. Django Documentation.  
   https://docs.djangoproject.com/

9. Scikit-learn: Random Forest and Other Randomized Tree Ensembles.  
   https://scikit-learn.org/stable/modules/ensemble.html#forest

10. XGBoost Documentation.  
    https://xgboost.readthedocs.io/

11. TensorFlow Keras Guide.  
    https://www.tensorflow.org/guide/keras

12. Telegram Bot API.  
    https://core.telegram.org/bots/api