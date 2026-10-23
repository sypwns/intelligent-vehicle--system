# Literature Review — JolSense

Project: ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control

Application: JolSense

Team: Shurenova Karima and Yerbolat Aisha

Status: Initial literature review. No implementation or experimental results obtained by the JolSense team are claimed.

## 1. Purpose and Scope

JolSense is a planned mobile application for personal vehicle owners.
It combines vehicle monitoring, ML-supported diagnostics, maintenance
guidance, weather-aware trip assessment, supported remote controls,
and Blizzard Mode safety check-ins.

This review examines research on vehicle telemetry, predictive
maintenance, explainable diagnostics, contextual routing, road-risk
assessment, and adverse-weather analysis.

The selected literature contains twelve sources, including an explicitly
identified arXiv preprint. The preprint must not be counted as a
peer-reviewed publication.

The studies inform the proposed design and evaluation. Their reported
performance must not be presented as performance achieved by JolSense.

## 2. Vehicle Monitoring and Data Reliability

Michailidis et al. [1] review OBD-II-based machine-learning applications
for vehicle monitoring, anomaly detection, driving analysis, and related
transportation functions. The review supports using diagnostic data
for vehicle-assistance applications while highlighting compatibility,
privacy, and generalization challenges.

For JolSense, this means that readings should include units, timestamps,
source information, and availability status. A measurement supported
by one vehicle must not be assumed available on every vehicle.

Pérez-González et al. [2] describe a telemetry and monitoring platform
for electric racing vehicles. Their work illustrates the engineering
of modular sensing, communication, and field validation.

However, a racing-vehicle platform does not establish compatibility
with ordinary passenger cars. JolSense can use modular data adapters
as an architectural principle without assuming the same hardware,
communication approach, or measured performance.

Sakhare et al. [3] investigate connected-vehicle data for monitoring
traffic operations around highway work zones. Speed, congestion,
and hard-braking information provide indicators of operational
conditions beyond historical crash records.

The study concerns work-zone monitoring in Indiana. It supports the
value of contextual road information but does not directly validate
individual vehicle diagnostics, stranded-driver detection, or
emergency alerts.

Together, these studies motivate input validation, provenance tracking,
and explicit handling of missing or outdated information.

## 3. Predictive Diagnostics and Maintenance Guidance

Mahale et al. [4] review artificial-intelligence approaches to predictive
maintenance in vehicles. Their discussion identifies opportunities
for earlier fault assessment alongside challenges involving data
quality, integration, and deployment.

This supports separating three types of information in JolSense:
recorded diagnostic trouble codes, rule-based warnings, and
ML-generated assessments.

Khan et al. [5] survey deep-learning approaches to predictive maintenance
across industrial applications. The work provides methodological
background on learning approaches, evaluation, and deployment.

Its industrial scope is broader than passenger vehicles. Consequently,
the survey informs method selection but cannot replace validation
on relevant automotive data.

Parate et al. [6] discuss explainable predictive maintenance using
SHAP and LIME. Their work is relevant to explaining why a model
produces a maintenance assessment.

For JolSense, an explanation should remain consistent with the
underlying model and available evidence. Explaining a prediction
does not establish that the predicted fault is present or that
the explanation identifies its physical cause.

Torkhesari et al. [7] investigate vehicle-sensor health monitoring
using autoencoders and Random Forest regression. Their approach
examines relationships between sensor measurements and estimated
values to identify potentially unreliable readings.

The available source is an arXiv preprint. Its findings require
cautious interpretation and should not be treated as independently
confirmed journal evidence.

These studies show that diagnostic performance depends on the
available sensors, labels, operating conditions, and vehicles.
JolSense must define its diagnostic target before model development
and restrict claims to the task actually evaluated.

Random Forest is the team's selected initial model. Its suitability
and performance will be assessed against an appropriate baseline;
the literature does not establish that it will be the best model
for the eventual dataset.

## 4. Weather, Routing, and Road-Risk Assessment

Nasution et al. [8] investigate contextual route recommendations
in heterogeneous traffic. Their work considers contextual factors
and vehicle characteristics when assessing routes.

This supports moving beyond distance-only route selection.
However, vehicle type and current vehicle health are different
inputs. The study does not directly establish whether live
diagnostic indicators improve route recommendations.

Ye et al. [9] examine the integration of weather information into
route selection and spatial decision support. Their work supports
associating environmental information with candidate journeys.

For JolSense, weather information should correspond to relevant
route locations and expected travel times. A single weather value
for the origin city may not describe conditions along the journey.

Khedher et al. [10] investigate multifactor road-risk recommendations
and explanations. Their work supports providing understandable
reasons for risk assessments.

Its decision context differs from personal vehicle diagnostics.
JolSense must document its own route-suitability criteria and
distinguish observed conditions from assumptions.

Ogungbire and Pulugurtha [11] investigate statewide weather-related
crash risk in North Carolina. Their workflow combines space-time
aggregation, an equivalent property damage only (EPDO) risk measure,
risk-zone identification, XGBoost classification, and SHAP interpretation.

The study supports considering spatial and temporal variation when
analysing weather-related road risk. It also illustrates combining
crash frequency and severity rather than relying on crash counts alone.

However, grid-level crash-risk zones do not directly represent
individual route suitability or account for the current health
of a particular vehicle. The paper informs contextual risk assessment
and explainability; it does not validate JolSense recommendations
or Blizzard Mode.

These studies address related but different outcomes: contextual
route selection, explainable road risk, and aggregated crash-risk
mapping. Their results cannot be compared directly as if they
measured the same task.

## 5. Adverse-Weather Perception

Mockey et al. [12] review deep-learning methods for road-image
analysis under degraded weather conditions, including rain,
fog, and snow.

The review discusses different architectures and data approaches,
alongside limitations involving weather diversity, geographical
transferability, and computational requirements.

This provides background on the difficulty of interpreting road
conditions under adverse weather. However, the reviewed methods
primarily concern image-based perception.

JolSense does not currently include a committed camera-based
computer-vision module. This source therefore informs limitations
and possible future research rather than validating the planned
weather-API integration.

## 6. Synthesis

The literature covers four connected areas:

- Vehicle telemetry and input reliability [1–3].
- Predictive diagnostics and maintenance explanations [4–7].
- Contextual routing and weather-related road risk [8–11].
- Adverse-weather image analysis [12].

A recurring challenge is combining heterogeneous data while
preserving reliability. Vehicle readings, weather forecasts,
and road information differ in coverage, timestamps, units,
and uncertainty.

Another recurring issue is transferability. A method evaluated
on one fleet, vehicle platform, region, or weather dataset may
not retain its performance elsewhere.

The studies also distinguish prediction from useful advice.
A model output needs an understandable explanation, appropriate
context, and clearly stated limitations before it can support
a driver decision.

For JolSense, these findings support the following design principles:

1. Record input sources, units, timestamps, and availability.
2. Keep observed trouble codes separate from inferred assessments.
3. Communicate missing and outdated information.
4. Explain route assessments without guaranteeing safety.
5. Evaluate diagnostic and recommendation functions separately.
6. Use only authorized interfaces for remote commands.
7. Distinguish simulated workflows from real integrations.

## 7. Implications for Blizzard Mode

Blizzard Mode is a planned safety-session function using available
vehicle, location, movement, and weather information to support
driver check-ins and consent-based contact alerts.

The reviewed papers provide background on telemetry and road risk.
They do not directly validate JolSense's proposed stop-detection
rules, check-in intervals, heartbeat monitoring, or message delivery.

These functions require separate engineering verification using
documented, safe scenarios.

Testing should include ordinary rest stops, potentially hazardous
stops, user responses, paused sessions, lost connectivity,
backend interruption, and notification failures.

A missing heartbeat indicates lost communication rather than
a confirmed emergency. Alerts should identify uncertainty and
the timestamp of the last known location.

The system must not claim guaranteed detection, message delivery,
emergency dispatch, or rescue.

## 8. Preliminary Research Gap and Contribution

The selected literature supports vehicle-health analysis and
context-aware route assessment. However, it provides limited
direct comparative evidence about whether adding current
vehicle-health indicators to weather- and road-based recommendations
improves agreement with independently reviewed route-suitability
assessments under matched scenarios.

This is a bounded gap in the reviewed sources, not a claim that
no integrated systems exist.

JolSense will investigate this question by comparing two configurations:

1. Weather and road-condition inputs.
2. The same inputs combined with supported vehicle-health indicators.

Both configurations will use the same retained scenarios,
candidate routes, environmental information, and shared settings.

Reference assessments will be independently reviewed before
system outputs are examined.

The planned primary measure is recommendation agreement rate.
Unsuitable-route selection rate is a secondary measure.

The comparison will assess recommendation appropriateness,
not actual crash reduction.

The broader application also includes monitoring, maintenance
guidance, supported remote controls, and Blizzard Mode.
Each requires its own functional verification.

## 9. Source Summary

| Ref. | Study | Relevance to JolSense | Main limitation |
|---|---|---|---|
| [1] | Michailidis et al. (2025) | OBD-II and ML applications for vehicle assistance | Broad review; vehicle compatibility varies |
| [2] | Pérez-González et al. (2025) | Modular vehicle telemetry and field validation | Electric racing context |
| [3] | Sakhare et al. (2022) | Connected-vehicle indicators for work-zone monitoring | Does not validate personal diagnostics or stranded-driver detection |
| [4] | Mahale et al. (2025) | Vehicle predictive-maintenance methods and challenges | Does not establish performance for the selected project data |
| [5] | Khan et al. (2026) | Predictive-maintenance methodology | Broader industrial scope |
| [6] | Parate et al. (2025) | Explainable maintenance predictions | Fleet-specific context; explanations do not prove causation |
| [7] | Torkhesari et al. (2025) | Sensor-health monitoring with autoencoders and Random Forest | arXiv preprint |
| [8] | Nasution et al. (2021) | Contextual and vehicle-aware route recommendations | Does not directly evaluate current vehicle-health inputs |
| [9] | Ye et al. | Weather-integrated route selection | Provider coverage and location-time alignment |
| [10] | Khedher et al. (2025) | Explainable multifactor road-risk assessment | Different decision context from personal diagnostics |
| [11] | Ogungbire and Pulugurtha (2025) | Spatiotemporal weather-related crash-risk mapping using EPDO, XGBoost, and SHAP | Grid-level analysis; does not validate individual vehicle-aware routing |
| [12] | Mockey et al. (2026) | Review of adverse-weather road-image analysis | Vision-based methods are outside the current project scope |

## 10. References

[1] Michailidis, E. T., Panagiotopoulou, A., & Papadakis, A. (2025).
A Review of OBD-II-Based Machine Learning Applications for Sustainable,
Efficient, Secure, and Safe Vehicle Driving.
Sensors, 25(13), 4057.
https://doi.org/10.3390/s25134057

[2] Pérez-González, A., et al. (2025).
An IoV-Based Real-Time Telemetry and Monitoring System for Electric Racing
Vehicles: Design, Implementation, and Field Validation.
Vehicles, 7(4), 128.
https://doi.org/10.3390/vehicles7040128

[3] Sakhare, R. S., Desai, J., Li, H., Kachler, M. A., & Bullock, D. M. (2022).
Methodology for Monitoring Work Zones Traffic Operations Using Connected
Vehicle Data.
Safety, 8(2), 41.
https://doi.org/10.3390/safety8020041

[4] Mahale, Y., Kolhar, S., & More, A. S. (2025).
A Comprehensive Review on Artificial Intelligence Driven Predictive
Maintenance in Vehicles: Technologies, Challenges and Future Research Directions.
Discover Applied Sciences, 7, 243.
https://doi.org/10.1007/s42452-025-06681-3

[5] Khan, U., Cheng, D. S., Setti, F., Fummi, F., Cristani, M.,
& Capogrosso, L. (2026).
A Comprehensive Survey on Deep Learning-based Predictive Maintenance.
ACM Transactions on Embedded Computing Systems, 25(2).
https://doi.org/10.1145/3732287

[6] Parate, V., Punithavel, R. K., Agrawal, S., & Jaiswal, A. (2025).
Explainable AI in Fleet Predictive Maintenance: Using SHAP and LIME
for Transparency and Regulatory Alignment.
International Journal of Applied Mathematics, 38(6s).
https://doi.org/10.12732/ijam.v38i6s.396

[7] Torkhesari, S., Yousefimehr, B., & Ghatee, M. (2025).
Car Sensors Health Monitoring by Verification Based on Autoencoder
and Random Forest Regression.
arXiv preprint, arXiv:2505.00876.
https://doi.org/10.48550/arXiv.2505.00876

[8] Nasution, S. M., Husni, E., Kuspriyanto, K., Yusuf, R.,
& Yahya, B. N. (2021).
Contextual Route Recommendation System in Heterogeneous Traffic Flow.
Sustainability, 13(23), 13191.
https://doi.org/10.3390/su132313191

[9] Ye, X., Li, S., Das, S., & Du, J.
Enhancing Routes Selection with Real-Time Weather Data Integration
in Spatial Decision Support Systems.
Spatial Information Research.
https://doi.org/10.1007/s41324-023-00564-8

[10] Khedher, I., Faci, N., Gazzah, S., & Faiz, S. (2025).
Recommendation and Explanation in Road Safety: A Holistic Exploration
of Multifaceted Road Risk.
World Wide Web, 28, 51.
https://doi.org/10.1007/s11280-025-01363-x

[11] Ogungbire, A., & Pulugurtha, S. S. (2025).
Spatiotemporal Risk Mapping of Statewide Weather-related Traffic Crashes:
A Machine Learning Approach.
Machine Learning with Applications, 20, 100642.
https://doi.org/10.1016/j.mlwa.2025.100642

[12] Mockey, A. M. Y., Xia, K., Perera, P. L., & Wang, T. (2026).
Review of Image Analysis Using Deep Learning Method Applied to the Road
Context in Degraded Weather Conditions.
International Journal of Computational Intelligence Systems, 19, 62.
https://doi.org/10.1007/s44196-025-01085-y

## 11. Reference Consistency

The review, literature matrix, reference library, and final report
must contain matching source details.

Sakhare et al. appears once as reference [3].
Ogungbire and Pulugurtha appears as reference [11].
Naresh and Dullam is excluded.

Reference [7] is explicitly classified as a preprint.
Publication metadata must be checked against publisher records
before final submission, including the final publication year
and complete bibliographic details for reference [9].

Updating this Markdown document does not automatically update
the Excel matrix or the .bib reference library.
