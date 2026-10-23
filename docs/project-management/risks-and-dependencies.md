# JolSense — Risks and Dependencies

Project: ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control  
Status: Initial risk register. All likelihood/impact ratings are planning judgments, not measured incident probabilities.

## Rating approach

Likelihood and impact use Low / Medium / High to help prioritize review. A risk is not considered resolved until the mitigation is demonstrated or the scope is formally adjusted. Owners are proposed engineering leads, not confirmed YouTrack assignments.

## Risk register

| ID | Risk | Likelihood | Impact | Planned mitigation and verification | Proposed owner |
|---|---|---|---|---|---|
| R-01 | Public automotive data lack target faults, labels, units or representative vehicles | High | High | Screen license and schema early; specify exact diagnostic task; use recorded sample/replay inputs; document excluded claims | Aisha |
| R-02 | ML model creates false warnings or misses important conditions | Medium | High | Compare with baseline, use held-out data, show uncertainty, avoid declaring a confirmed mechanical defect; error analysis | Aisha |
| R-03 | OBD-II compatibility or supported parameters differ by vehicle | High | Medium | Identify supported interfaces/parameters; show unavailable readings; test adapter disconnects; preserve a replay mode | Karima |
| R-04 | Weather/routing API is unavailable, stale, geographically incomplete or costly | Medium | High | Evaluate quotas and coverage; timestamp sources; use licensed fixtures for deterministic tests; display stale/error states | Karima |
| R-05 | Vehicle-health-aware route advice gives an unsafe or misleading recommendation | Medium | High | Keep rule provenance and independent suitability rubric; provide reasons, postpone/insufficient-information states, not safety guarantees | Both |
| R-06 | Unauthorized or unsafe remote vehicle commands | Medium | High | Mandatory mock/sandbox demonstration; capability discovery; no real actuation without authorization, explicit confirmation and separate safe tests | Karima with Aisha review |
| R-07 | Blizzard Survival Mode falsely escalates normal stops or traffic queues | High | High | Configurable rules, repeated check-ins, opt-out/snooze, location context; test congestion, parking and normal rest scenarios | Both |
| R-08 | Emergency alert fails due to power loss, background restrictions, network outage, server/provider issues | High | High | Node.js heartbeat monitor and explicit missed-heartbeat semantics; retry/queue where possible; alert states; platform limitations and end-to-end tests | Both |
| R-09 | Missing heartbeat is mistaken for a confirmed emergency | Medium | High | Phrase notification as possible loss of contact, show last successful location/time, apply grace period and user acknowledgements | Both |
| R-10 | Sensitive location, emergency contact, credentials or trip history leak | Medium | High | Contact opt-in, minimization and retention policy, access control, sanitized logs/fixtures, secrets outside Git | Both |
| R-11 | Node.js backend integration with diagnostic ML increases complexity | Medium | Medium | Versioned API contract, choose compatible inference path, mocked adapter tests; defer unsupported ML endpoints | Both |
| R-12 | Scope exceeds team capacity or available semester time | High | High | Prioritize mandatory prototype scenarios, stage advanced interfaces, mark incomplete features explicitly, review milestone dependencies | Both |
| R-13 | Diagrams, requirements and implementation drift apart | Medium | Medium | Cross-reference FR/NFR IDs in test cases; review architecture/diagrams in the same pull request as major changes | Aisha |
| R-14 | Limited evidence from prospective users or external reviewers | Medium | Medium | Separate assumptions from verified findings; plan approved outreach; do not invent interviews or consent | Both |

## Dependency register

| ID | Dependency | Needed for | Fallback / next decision |
|---|---|---|---|
| D-01 | Flutter SDK and supported Android target | Mobile UI, location and local check-ins | Confirm versions and background permissions; use emulator/demo mode initially |
| D-02 | Node.js framework, service hosting and scheduler selection | API and Blizzard heartbeat monitoring | Compare options and record an ADR before committing to deployment |
| D-03 | Database and retention design | User profiles, maintenance logs, session/contact state | Select storage after data-minimization review; test with sanitized data |
| D-04 | Licensed, documented vehicle data or authorized OBD-II adapter | Vehicle monitoring and ML diagnosis | Sample/replay adapter clearly labeled; do not imply live measurements |
| D-05 | Weather and routing services with viable Kazakhstan coverage | Route planning and Blizzard temperature context | Deterministic fixture mode and clear provider-unavailable state |
| D-06 | Alert provider and opted-in test recipients | Blizzard messaging verification | Telegram sandbox/test bot candidate; no actual contact notifications without consent |
| D-07 | External independent reference assessment | Formal route-recommendation research comparison | Draft rubric and reviewer plan; do not report unreviewed outcomes as validated |
| D-08 | OEM/authorized provider integration | Any real remote vehicle actuation | Mock capability/command UI; retain real control as optional later scope |
| D-09 | Supervisor/team review and traceable YouTrack tasks | Pre-defense evidence and approvals | Record pending decisions; do not label drafts accepted |

## Highest-priority controls

Emergency messaging and remote commands require fail-safe interface behavior, accurate status reporting and authorized access. Blizzard Survival Mode is a support tool: a stopped car or missed heartbeat is not proof that a driver is stranded, and JolSense cannot guarantee rescue or message delivery. The application must not autonomously call emergency services. Where supported, the app may offer a user-initiated emergency call action.

## Maintenance of this register

At each milestone review, update the likelihood/impact judgment, concrete mitigation progress, evidence link, owner and next action in YouTrack. Do not fabricate closed risks or approved external integrations.
