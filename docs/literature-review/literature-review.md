# Literature Review — JolSense

Project: ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control  
Application name: JolSense  
Team: Shurenova Karima and Yerbolat Aisha  
Status: Initial critical literature review for the main diploma project; no implementation or experimental results are claimed.

## 1. Purpose and scope

JolSense is a proposed mobile application that combines vehicle condition monitoring, machine-learning-supported diagnostics, understandable maintenance guidance, weather-aware trip assessment, supported remote-control interfaces, and a planned Blizzard Survival Mode for potentially stranded drivers. This review examines the scientific foundations of those functions, particularly the use of vehicle telemetry, predictive maintenance, contextual routing, connected-vehicle safety indicators, and adverse-weather analysis.

The central technical challenge is not simply displaying these data sources together. The project must establish whether the data are trustworthy, whether diagnostic outputs can be interpreted responsibly, and whether vehicle condition contributes useful information to weather-aware travel decisions. The selected sources include peer-reviewed research and one explicitly identified preprint. Their findings are used to guide requirements and design; they are not results obtained by the JolSense team.

## 2. Connected-vehicle monitoring and data reliability

Michailidis et al. [1] review machine-learning applications based on OBD-II data, including vehicle-health monitoring, anomaly detection, driver behavior, and intelligent road support. Their synthesis demonstrates the breadth of possible uses of standardized diagnostic information, while emphasizing practical difficulties such as heterogeneous vehicle support, privacy, computational requirements, and model generalization. For JolSense, this supports treating a sensor value as an observation with a timestamp, unit, vehicle context, and availability status rather than as universally available information.

Pérez-González et al. [2] present and field-validate an Internet-of-Vehicles telemetry platform for electric racing vehicles using modular sensors and long-range communication. The paper illustrates how distributed vehicle sensing can be engineered and tested. Its race-vehicle setting and communications architecture, however, do not establish compatibility with ordinary passenger cars or prove remote-control access. JolSense can adopt the architectural principle of modular telemetry adapters without assuming the same hardware or performance.

Sakhare et al. [3] analyze connected-vehicle speed, congestion, and hard-braking events near highway work zones. Their study shows that operational road risk may be assessed using current vehicle-derived signals rather than relying exclusively on delayed crash reports. The geographic and application scope is specific: Indiana work zones and transportation-agency monitoring. Consequently, their measurements are relevant to contextual road-risk reasoning, but they are not direct evidence that a phone application can detect stranded drivers or prevent crashes.

Together, these works motivate provenance tracking, input validation, provider-specific adapters, and careful treatment of missing or outdated readings in JolSense.

## 3. Predictive diagnostics and maintenance advice

Mahale, Kolhar, and More [4] review AI-based predictive maintenance for vehicles, covering machine learning, deep learning, explainable AI, maintenance timing, and data challenges. They identify data quality, integration, and scalability as major obstacles. This supports the project's distinction between observed diagnostic trouble codes, derived threshold warnings, and model-generated predictions.

Khan et al. [5] survey deep-learning-based predictive maintenance across industrial equipment. Their treatment of model families, learning paradigms, benchmarks, and deployment considerations offers a methodological foundation, but much of its discussion is not specific to personal vehicles. It should therefore inform candidate methods and evaluation design rather than substitute for automotive validation.

Parate et al. [6] discuss explainable AI for fleet predictive maintenance, comparing models and using SHAP and LIME to interpret maintenance predictions. This is relevant to JolSense's aim of explaining technical alerts in accessible language. Nevertheless, fleet-maintenance labels, business costs, and regulatory priorities may differ from the conditions faced by an individual driver. Explanations must remain faithful to the underlying model and should not turn an uncertain prediction into a definitive diagnosis.

Torkhesari, Yousefimehr, and Ghatee [7] propose sensor-health monitoring that compares measured values with estimates obtained from correlated sensors, using autoencoders and Random Forest regression. This is a useful example of detecting suspicious measurements rather than assuming every sensor is accurate. However, the accessible version is an arXiv preprint, not an independently verified peer-reviewed journal article. Its reported outcomes need cautious interpretation, and estimated sensor readings must never be silently presented as directly measured values.

Across these studies, a recurring limitation is transferability: a diagnostic method evaluated on a particular vehicle, sensor set, or fleet may not work equally well on other models. JolSense should restrict claims to the faults and data sources actually supported by its eventual evaluation.

## 4. Weather, route selection, and driver decision support

Nasution et al. [8] propose contextual route recommendation in heterogeneous traffic using traffic conditions, vehicle type, and environmental factors including weather. The paper illustrates the potential of combining route properties with vehicle-specific attributes. It does not, however, establish the added value of current vehicle-health diagnostics, which is an important distinction for JolSense.

Ye et al. [9] develop a spatial decision-support navigation tool that combines routing information with real-time or forecast weather from an external API. This study directly supports the proposed integration of mapping and meteorological information. It also highlights an implementation dependency: a useful recommendation requires weather information associated with the relevant route locations and times, not merely a citywide weather label.

Khedher et al. [10] investigate road-risk assessment and explainable route recommendations using objective and subjective information. Their emphasis on interpretable risk factors is pertinent to a driver-facing application, although their broader road-safety and planning context differs from individual vehicle diagnostics. JolSense should show the reasons behind a route recommendation and distinguish observed evidence from constructed assumptions.

Naresh and Dullam [11] combine machine-learning-based accident severity analysis with spatial network analysis and safer-route identification. This offers another example of turning risk estimates into operational recommendations. Its reported classification performance is specific to the study's data and methods; it must not be reused as an expected JolSense result. Nor does a route-suitability score, by itself, demonstrate actual accident reduction.

The four studies collectively suggest that environment-aware routing is feasible, but route suitability depends on what factors are included and how they are assessed. An important project-specific design question remains whether adding trustworthy vehicle-health indicators meaningfully changes recommendations under otherwise matched route and weather conditions.

## 5. Adverse-weather perception and its limits

Mockey et al. [12] review deep-learning approaches to road-scene analysis in rain, fog, snow, and other degraded visibility conditions. They organize the literature by task, data type, and architecture, discussing convolutional networks, generative adversarial networks, transformers, synthetic augmentation, and relevant evaluation metrics. The review identifies limited weather diversity in datasets, weak geographic transferability, and computational trade-offs.

This paper is useful to JolSense because it explains why weather-related information can be uncertain and region-dependent. However, the reviewed methods primarily concern image-based perception for road scenes and autonomous driving. JolSense's currently planned weather-safety module relies on weather, road, and vehicle-status information, not on a committed camera-based computer-vision pipeline. Therefore, the paper supports future possibilities and robustness considerations; it does not justify claiming that JolSense presently detects snow, fog, or road obstacles from images.

## 6. Synthesis and implications for JolSense

The reviewed literature falls into several connected but distinct areas. Vehicle-monitoring studies show how telemetry can be acquired and interpreted [1–3]. Maintenance research addresses anomaly detection, predictive modelling, and understandable explanations [4–7]. Routing studies demonstrate the value of context-aware recommendations based on traffic, weather, road attributes, and, in some cases, vehicle characteristics [8–11]. Adverse-weather perception research highlights the limits of data quality and cross-region generalization [12].

A common theme is the difficulty of combining heterogeneous data while preserving reliability. Sources have different timestamps, coverage, vehicle compatibility, quality constraints, and uncertainty. Another theme is that accurate model output is not automatically actionable advice: route and maintenance recommendations must be supported by clear decision rules, traceable inputs, and stated limitations.

For the initial JolSense design, these findings imply the following:

- Record the source, time, units, and availability of vehicle and environmental inputs.
- Keep observed diagnostic codes separate from ML-inferred anomalies and maintenance suggestions.
- Present reasons and uncertainty for route assessments rather than a guaranteed-safe label.
- Use authorized vehicle-control interfaces only; telemetry access alone is not evidence of remote-actuation permission.
- Evaluate functionality and safety behavior on documented scenarios before claiming real-world reliability.

The planned Blizzard Survival Mode introduces a further application-level need: cautious identification of potentially dangerous stops, driver check-ins, and consent-based contact notification. The papers in this review provide background on telemetry and road risk, but they do not directly validate JolSense's proposed stranded-driver detection rules, phone background behavior, heartbeat escalation, or emergency-message delivery. These functions require separate requirements, simulation tests, privacy analysis, and engineering validation. A missing heartbeat can indicate a power or connection failure rather than an emergency; alerts must communicate uncertainty.

## 7. Preliminary gap and project contribution

Existing studies support vehicle-health analysis and context-aware route planning separately. The selected literature offers less direct comparative evidence about whether adding current vehicle-health indicators to weather- and road-based route recommendations improves their agreement with independently assessed route suitability under matched scenarios. This is a bounded gap in the reviewed material, not a claim that no integrated systems exist.

The proposed contribution is an integrated driver-facing prototype and a transparent evaluation approach: compare a weather-and-road-only configuration with a configuration that additionally considers supported vehicle-health indicators, using the same candidate routes, scenario conditions, and independent reference assessments. The main outcome is recommendation appropriateness under documented scenarios, not proof of reduced crash incidence. The full diploma additionally covers maintenance assistance, monitoring, authorized remote-management workflows, and planned Blizzard Survival Mode, which will need their own verification evidence.

## 8. Source summary and relevance

| Ref. | Study | Main contribution | Limitation for JolSense |
|---|---|---|---|
| [1] | Michailidis et al. (2025) | OBD-II and ML applications for monitoring and driving support | Broad review; compatibility and generalization concerns |
| [2] | Pérez-González et al. (2025) | Field-validated modular vehicle telemetry | Electric racing setting, not ordinary-car compatibility |
| [3] | Sakhare et al. (2022) | Connected-vehicle hard-braking/congestion analysis for work-zone safety | Agency-level work-zone monitoring, not roadside rescue detection |
| [4] | Mahale et al. (2025) | Vehicle predictive-maintenance review | Quality and integration challenges; no JolSense validation |
| [5] | Khan et al. (2026) | Survey of deep-learning predictive-maintenance methods | Industrial scope broader than passenger cars |
| [6] | Parate et al. (2025) | Explainable fleet-maintenance predictions with SHAP/LIME | Fleet context and dataset-specific performance |
| [7] | Torkhesari et al. (2025) | Autoencoder and Random Forest sensor-fault monitoring | arXiv preprint; needs independent validation |
| [8] | Nasution et al. (2021) | Contextual traffic and vehicle-aware routing | Does not directly test live vehicle-health indicators |
| [9] | Ye et al. (2023) | Weather-integrated navigation and route assessment | Weather/provider availability and scope |
| [10] | Khedher et al. (2025) | Explainable multifactor road-risk recommendations | Different decision context from vehicle diagnostics |
| [11] | Naresh and Dullam (2026) | Accident-risk prediction and spatially informed routing | Reported accuracy is study-specific; not crash-prevention proof |
| [12] | Mockey et al. (2026) | Review of adverse-weather road-image analysis | Primarily vision-based; not current JolSense architecture |

## 9. References

[1] Michailidis, E. T., Panagiotopoulou, A., & Papadakis, A. (2025). A Review of OBD-II-Based Machine Learning Applications for Sustainable, Efficient, Secure, and Safe Vehicle Driving. Sensors, 25(13), 4057. https://doi.org/10.3390/s25134057

[2] Pérez-González, A., et al. (2025). An IoV-Based Real-Time Telemetry and Monitoring System for Electric Racing Vehicles: Design, Implementation, and Field Validation. Vehicles, 7(4), 128. https://doi.org/10.3390/vehicles7040128

[3] Sakhare, R. S., Desai, J., Li, H., Kachler, M. A., & Bullock, D. M. (2022). Methodology for Monitoring Work Zones Traffic Operations Using Connected Vehicle Data. Safety, 8(2), 41. https://doi.org/10.3390/safety8020041

[4] Mahale, Y., Kolhar, S., & More, A. S. (2025). A Comprehensive Review on Artificial Intelligence Driven Predictive Maintenance in Vehicles: Technologies, Challenges and Future Research Directions. Discover Applied Sciences, 7, 243. https://doi.org/10.1007/s42452-025-06681-3

[5] Khan, U., Cheng, D. S., Setti, F., Fummi, F., Cristani, M., & Capogrosso, L. (2026). A Comprehensive Survey on Deep Learning-based Predictive Maintenance. ACM Transactions on Embedded Computing Systems, 25(2). https://doi.org/10.1145/3732287

[6] Parate, V., Punithavel, R. K., Agrawal, S., & Jaiswal, A. (2025). Explainable AI in Fleet Predictive Maintenance: Using SHAP and LIME for Transparency and Regulatory Alignment. International Journal of Applied Mathematics, 38(6s). https://doi.org/10.12732/ijam.v38i6s.396

[7] Torkhesari, S., Yousefimehr, B., & Ghatee, M. (2025). Car Sensors Health Monitoring by Verification Based on Autoencoder and Random Forest Regression. arXiv preprint, arXiv:2505.00876. https://doi.org/10.48550/arXiv.2505.00876

[8] Nasution, S. M., Husni, E., Kuspriyanto, K., Yusuf, R., & Yahya, B. N. (2021). Contextual Route Recommendation System in Heterogeneous Traffic Flow. Sustainability, 13(23), 13191. https://doi.org/10.3390/su132313191

[9] Ye, X., Li, S., Das, S., & Du, J. (2023). Enhancing Routes Selection with Real-Time Weather Data Integration in Spatial Decision Support Systems. Spatial Information Research. https://doi.org/10.1007/s41324-023-00564-8

[10] Khedher, I., Faci, N., Gazzah, S., & Faiz, S. (2025). Recommendation and Explanation in Road Safety: A Holistic Exploration of Multifaceted Road Risk. World Wide Web, 28, 51. https://doi.org/10.1007/s11280-025-01363-x

[11] Naresh, V. S., & Dullam, A. (2026). Data-Driven Road Safety Enhancement: Neural Network-Based Accident Classification and Safe Route Identification Using Spatial Network Analysis. European Transport Research Review, 18, 59. https://doi.org/10.1186/s12544-026-00824-2

[12] Mockey, A. M. Y., Xia, K., Perera, P. L., & Wang, T. (2026). Review of Image Analysis Using Deep Learning Method Applied to the Road Context in Degraded Weather Conditions. International Journal of Computational Intelligence Systems, 19, 62. https://doi.org/10.1007/s44196-025-01085-y

## 10. Source verification note

The earlier candidate identifier https://doi.org/10.13140/RG.2.2.32215.28328 resolves to a paper on e-commerce sentiment analysis, not automotive diagnostics; it has therefore been excluded from this review. The vehicle telemetry paper [2], already considered in the project's literature work, has been included to retain a twelve-source review. Reference [7] is a preprint and should not be counted as peer-reviewed. The complete literature matrix is maintained separately; this Markdown file does not claim to update it.
