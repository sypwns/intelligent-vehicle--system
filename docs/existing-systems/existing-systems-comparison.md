# Existing Systems Comparison

**Project:** JolSense  
**Diploma topic:** ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control  
**Status:** Draft based on public documentation  
**Review date:** 23 October 2026

## 1. Purpose

This document compares JolSense's planned functionality with four
existing systems: StarLine 2, Pandora Connect, Carly, and OBDeleven.

The selected systems represent two relevant categories:

- Vehicle security, telematics, and remote control.
- OBD-based monitoring, diagnostics, and maintenance support.

The comparison identifies existing capabilities, implementation
constraints, and opportunities for JolSense. It does not claim that
JolSense is the first system to provide any individual function.

## 2. Method and Evidence

The comparison uses official product documentation and application
descriptions published by the developers. No hands-on product testing
was performed.

StarLine 2 is the primary StarLine application assessed. The separate
StarLine Navigator application is also considered when discussing
navigation, to avoid incorrectly treating route planning as absent
from the StarLine ecosystem.

Feature availability may depend on:

- Vehicle make, model, year, and installed equipment.
- Hardware connections and supported control units.
- Subscription or product package.
- Software version and region.

### Assessment Labels

- **Documented:** explicitly described in a reviewed source.
- **Conditional:** documented, but dependent on a supported configuration.
- **Not confirmed:** not established by the reviewed sources; this
  does not prove that the feature is unavailable.
- **Planned:** intended for JolSense, but not yet implemented or validated.

Product descriptions establish advertised capabilities, not independently
measured performance.

## 3. Selected Systems

### 3.1 StarLine 2 and the StarLine Ecosystem

StarLine 2 supports interaction with compatible StarLine security and
telematics equipment. Documented functions include vehicle security
control, remote engine-start settings, status information, location
tracking, and journey history [1–3].

Battery and temperature information and comfort controls depend on
the installed equipment and supported vehicle configuration [1–2].

StarLine also offers a separate Navigator application with route
planning, traffic-aware navigation, and supported vehicle controls
[4–5]. Therefore, maps, ordinary navigation, and journey history
cannot be presented as distinctive JolSense features.

**Relevance to JolSense:** remote-control workflows, status presentation,
notifications, and navigation integration.

### 3.2 Pandora Connect

Pandora Connect is designed for vehicles equipped with compatible
Pandora telematics systems.

Its developer-published description documents vehicle and sensor
status, location, event and journey history, and supported security,
engine, heater, and boot controls [6].

**Relevance to JolSense:** remote commands, vehicle tracking, event
history, and compatibility-dependent monitoring.

### 3.3 Carly

Carly provides OBD-based diagnostics and live-data functions through
its scanner and mobile application.

Smart Mechanic provides additional diagnostic guidance for supported
faults. Service and maintenance functions are also described [7–8].

**Relevance to JolSense:** understandable diagnostic information,
live-data presentation, and maintenance support.

### 3.4 OBDeleven

OBDeleven provides diagnostic scans, fault-code information, live
sensor readings, and supported vehicle customizations.

One-Click Apps provide predefined customization and service functions.
Feature coverage depends on vehicle, region, equipment, and plan
[9–12].

**Relevance to JolSense:** OBD monitoring, diagnostic explanations,
maintenance functions, and explicit compatibility restrictions.

## 4. Comparison Criteria

The same ten criteria are applied to all systems:

1. Required equipment and connection.
2. Vehicle-state monitoring.
3. Fault diagnostics and explanations.
4. ML-based future-fault prediction.
5. Maintenance support.
6. Remote vehicle commands.
7. Maps, journey history, and navigation.
8. Combined weather and vehicle-condition route assessment.
9. Safety check-ins and missed-response escalation.
10. Compatibility and operational dependencies.

## 5. Comparison Matrix

| Criterion | StarLine 2 / ecosystem [1–5] | Pandora Connect [6] | Carly [7–8] | OBDeleven [9–13] | JolSense — planned |
|---|---|---|---|---|---|
| **1. Required equipment and connection** | Compatible StarLine security or telematics equipment; Navigator is a separate application. | Compatible Pandora telematics equipment. | Carly OBD scanner and application. | OBDeleven OBD device and application. | OBD-II adapter for monitoring; separate authorized interface for remote commands. |
| **2. Vehicle-state monitoring** | Conditional battery, temperature, and vehicle-status information. | Conditional vehicle, sensor, fuel, and temperature information. | Live readings; available parameters vary by vehicle. | Live readings such as RPM, coolant temperature, and battery voltage where supported. | Supported readings with units, timestamps, and connection status; unavailable data explicitly identified. |
| **3. Fault diagnostics and explanations** | General OBD scanning and repair-oriented explanations are not confirmed in the reviewed sources. | General OBD scanning and repair-oriented explanations are not confirmed in the reviewed sources. | Diagnostic scans and Smart Mechanic guidance for supported faults. | Fault-code scans, descriptions, and recommended actions; coverage varies. | Read supported trouble codes and explain diagnostic findings in understandable language. |
| **4. ML-based future-fault prediction** | A validated future-fault prediction model is not confirmed. | A validated future-fault prediction model is not confirmed. | Potential-issue detection is advertised; ML methods and future-prediction validation are not established by the reviewed sources. | A validated future-fault prediction model is not confirmed. | Evaluate Random Forest for a defined task; future prediction depends on suitable time-linked fault data. |
| **5. Maintenance support** | A diagnostic maintenance-planning assistant is not confirmed in the reviewed sources. | A diagnostic maintenance-planning assistant is not confirmed in the reviewed sources. | Service information and maintenance functions are documented. | Maintenance reminders and supported service resets are documented. | Maintenance reminders and guidance based on available information. |
| **6. Remote vehicle commands** | Conditional security, engine-start, and comfort controls. | Conditional security, engine, heater, and boot controls. | Telematics-based remote locking and engine start are not confirmed. | Coding and customizations are documented; these do not establish telematics-based remote control. | Door locking/unlocking planned; engine start, boot, and windows depend on authorized access and compatibility. |
| **7. Maps, journey history, and navigation** | StarLine 2 documents location and tracks; separate StarLine Navigator provides route planning and traffic-aware navigation. | Vehicle location and journey history are documented; turn-by-turn navigation is not confirmed in the reviewed description. | Journey navigation is not confirmed in the reviewed sources. | Journey navigation is not confirmed in the reviewed sources. | Display candidate routes and assessment results; full turn-by-turn navigation is not a confirmed deliverable. |
| **8. Combined weather and vehicle-condition route assessment** | A route-suitability assessment jointly using forecast weather and diagnostic vehicle condition is not confirmed. | Equivalent combined assessment is not confirmed. | Equivalent combined assessment is not confirmed. | Equivalent combined assessment is not confirmed. | Compare candidate routes using available weather, road information, and vehicle-health indicators. |
| **9. Safety check-ins and missed-response escalation** | An equivalent prolonged-stop, cold-weather, check-in, and server-heartbeat workflow is not confirmed. | An equivalent workflow is not confirmed in the reviewed description. | An equivalent workflow is not confirmed. | An equivalent workflow is not confirmed. | Blizzard Mode: user-enabled monitoring, check-ins, pauses, help requests, and configured contact escalation. |
| **10. Compatibility and operational dependencies** | Installed equipment, vehicle integration, software, and connectivity for relevant functions. | Installed equipment, connections, sensors, and connectivity for relevant functions. | Vehicle support, scanner, and product package. | Vehicle, control unit, region, device, and plan. | Tested phone and adapter configuration, API coverage, authorized control access, background permissions, and connectivity. |

## 6. Findings

### 6.1 Functions Already Available in Existing Systems

The reviewed systems demonstrate that the following functions already
exist in commercial products:

- Vehicle-state monitoring.
- Fault-code information and diagnostic explanations.
- Maintenance-related support.
- Remote security and engine commands.
- Vehicle location and journey history.
- Route planning and navigation within the StarLine ecosystem.

JolSense must not present these individual capabilities as unique
inventions.

### 6.2 Proposed Distinction: Contextual Journey Assessment

JolSense plans to combine available vehicle-health information with
weather forecasts and route information.

The proposed contribution is an assessment of journey or route
suitability based on these inputs, rather than simply displaying
a map or individual sensor readings.

The reviewed documentation does not establish an equivalent combined
workflow in the four selected systems. This is a finding about the
reviewed sources, not proof of market-wide novelty.

The project will evaluate whether adding vehicle-health indicators
improves recommendation suitability compared with a baseline using
weather and road-condition information alone.

### 6.3 Proposed Distinction: Blizzard Mode

Blizzard Mode will provide a user-enabled safety-monitoring session
for potentially concerning prolonged stops in cold or adverse weather.

The planned workflow includes:

1. Registering the safety session with the backend.
2. Assessing available stop-duration, location, vehicle, and weather inputs.
3. Requesting an audible and visual safety check-in.
4. Allowing the driver to confirm safety, pause checks, or request help.
5. Escalating unanswered checks according to configured deadlines.
6. Monitoring periodic status updates on the server.
7. Initiating notifications to opted-in emergency contacts.

The initial proposed notification channel is Telegram. Provider
integration remains pending.

A missed heartbeat indicates interrupted communication, not a
confirmed emergency. An alert will describe the missed response or
communication loss and identify the timestamp of the last known
location.

An equivalent combined workflow is not confirmed in the reviewed
competitor documentation. Further verification is required before
claiming it as a competitive distinction.

## 7. Implications for JolSense Design

### 7.1 Clear Status and Compatibility

JolSense should identify the supported vehicle readings and commands,
show timestamps, and distinguish unavailable data from normal values.

A battery-voltage reading should not be presented as a complete
battery-health assessment.

### 7.2 Understandable Diagnostic Information

Diagnostic explanations should distinguish:

- The observed reading or fault code.
- A model estimate.
- Possible interpretations.
- Recommended checks.
- Uncertainty and limitations.

The application should not present a possible cause as a confirmed
mechanical diagnosis.

### 7.3 Separate Monitoring and Remote Control

OBD-II data acquisition does not establish support for remote locking,
engine start, boot opening, or window control.

These commands require a separate compatible and authorized interface.
Simulated commands must be labelled and must not be reported as real
vehicle-control results.

### 7.4 Honest Route Assessment

Forecast weather is not a direct measurement of road-surface condition.

JolSense should identify its data sources and freshness and describe
outputs as estimated suitability or risk. Missing information should
not produce an unsupported “safe” result.

### 7.5 Transparent Safety Sessions

Blizzard Mode should display whether server monitoring is registered,
the last successful update, and notification status.

Location may be transmitted to the backend during an active session.
Disclosure to emergency contacts occurs under the configured alert
conditions and requires prior consent.

A notification request or provider acknowledgement does not prove
that the contact received, read, or acted on the message.

## 8. Evaluation and Limitations

This comparison is a documentation review, not an experimental
benchmark.

It does not establish that JolSense is more accurate, more reliable,
or safer than the existing products. Such claims require comparable
testing and evidence.

“Not confirmed” means that sufficient evidence was not found in the
reviewed material. It does not mean that the function is absent from
every product version or supported configuration.

All JolSense capabilities remain planned until implemented and tested.
Blizzard Mode testing will use constructed scenarios and test contacts;
it will not require dangerous real-world exposure.

The reviewed product descriptions may change. Their coverage should
be checked again before the final submission.

## 9. Conclusion

Existing products provide strong examples of telematics, remote control,
OBD diagnostics, maintenance support, and navigation.

JolSense will build on these established functions while investigating
two integrated workflows: vehicle-condition-aware journey assessment
and Blizzard Mode safety check-ins.

Their usefulness and reliability remain to be evaluated. The project
will report implemented capabilities, experimental results, and
limitations without claiming guaranteed fault prevention, accident
reduction, or rescue.

## 10. References

1. StarLine. StarLine GSM+GPS Master 6 V2: mobile application functions.  
   https://www.starline.ru/2024/novinki/novinka-starline-gsm-gps-master-6-v2-dobavte-umnuyu-telematiku/

2. StarLine. Smart Comfort in the StarLine application.  
   https://www.starline.ru/2024/novinki/starline-umnyj-komfort-v-tvoih-rukah/

3. StarLine LLC. StarLine 2: developer-published application description.  
   https://apps.apple.com/ru/app/starline-2/id1463863869

4. StarLine LLC. StarLine Navigator: developer-published application description.  
   https://play.google.com/store/apps/details?id=ru.starline.navigator

5. StarLine. StarLine Maps: mapping and routing services.  
   https://maps.starline.ru/

6. Pandora. Pandora Connect: developer-published application description.  
   https://apps.apple.com/ru/app/pandora-connect/id1510941006

7. Carly. Smart Mechanic.  
   https://www.mycarly.com/how-it-works/smart-mechanic/

8. Carly. Car Maintenance.  
   https://www.mycarly.com/how-it-works/car-maintenance/

9. OBDeleven. What is OBDeleven?  
   https://support.obdeleven.com/en/articles/5602625-what-is-obdeleven

10. OBDeleven. What is Live Data?  
    https://support.obdeleven.com/en/articles/11009883-what-is-live-data

11. OBDeleven. What are One-Click Apps and how to use them?  
    https://support.obdeleven.com/en/articles/5605415-what-are-one-click-apps-and-how-to-use-them

12. OBDeleven. Which features will work in my country?  
    https://support.obdeleven.com/en/articles/8910926-which-features-will-work-in-my-country

13. OBDeleven. How to use widgets on the OBDeleven app.  
    https://support.obdeleven.com/en/articles/14014848-how-to-use-widgets-on-the-obdeleven-app

