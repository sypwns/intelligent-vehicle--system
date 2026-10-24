# JolSense — Project Milestones

Project: ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control  
Application name: JolSense  
Team: Shurenova Karima and Yerbolat Aisha  
Status: Planning document for the main diploma project; dates and completion evidence must be maintained in YouTrack.

## Purpose

This document organizes the development of JolSense into reviewable milestones. The product combines vehicle monitoring, ML-assisted diagnostics, maintenance guidance, weather-aware route recommendations, supported remote-control functionality, and Blizzard Survival Mode. The milestone plan covers the diploma application, rather than only the separate research experiment.

## Milestone plan

| Milestone | Planned scope and deliverables | Completion criteria / evidence | Lead / contributors | Dependencies | Target date / status |
|---|---|---|---|---|---|
| M1 — Proposal and scope | Approved project scope, intended users, aims, requirements and feature boundaries | Current proposal and requirements committed, reviewed, and traceable to YouTrack tasks | Both; Karima leads proposal, Aisha leads requirements | Supervisor review and scope clarification | Pending supervisor-approved date; review pending |
| M2 — Evidence and comparisons | Critical literature review; existing-system and technology comparisons | Review includes required source coverage; comparisons document alternatives, criteria and sources | Aisha: literature; Karima: comparisons | Source verification and technology alternatives | Pending supervisor-approved date; evidence to verify |
| M3 — Architecture and UX | Architecture description and decisions; context, use-case and component diagrams; clickable UX prototype or documented mockups | Editable diagrams and readable exports; key scenarios represented; architecture matches requirements | Aisha: architecture/component diagram; Karima: UX/use-case diagram; context jointly reviewed | M1–M2; provider feasibility | Pending supervisor-approved date; in planning |
| M4 — Project foundation | Flutter mobile shell, Node.js backend skeleton, safe config examples, persistence decision, module contracts | Clean setup instructions, basic app/API run, no secrets in repository, first unit checks pass | Aisha: Node.js foundation and mobile API coordination; Karima: Flutter shell and diagnostic ML interface; both: integration | M3; technology selection | Pending supervisor-approved date; not verified |
| M5 — Monitoring and diagnostics | Vehicle profiles, sample/replay telemetry, dashboards, DTC explanations, bounded ML diagnostic prototype | Acceptance tests for supported FR-01–FR-07; documented dataset/provenance and model validation | Karima: diagnostic ML and model interface; Aisha: Node.js monitoring API and mobile integration; both: telemetry validation | Data licensing, input schema, baseline data, M4 | Pending supervisor-approved date; not verified |
| M6 — Maintenance and weather routing | Maintenance records and reminders, forecast/route adapters, route advisories, vehicle-aware comparison | Tests for FR-08–FR-11 and FR-18–FR-20; missing-data handling; documented route scenario examples | Karima: maintenance and diagnostic health signals, weather/routing UI; Aisha: Node.js API coordination and routing integration | API access, route sources, validated health outputs | Pending supervisor-approved date; not verified |
| M7 — Blizzard Survival Mode | Stopped-vehicle detection, driver check-ins, consented emergency contacts, backend heartbeat monitor, alert simulation | Simulated normal and hazardous cases; missed check-in and heartbeat tests; evidence of opt-in and alert-status handling | Aisha: Node.js session/heartbeat endpoints and mobile coordination; Karima: diagnostic signals/check-in UI; both: backend escalation review | M4–M6; privacy design, notification channel, OS constraints | Pending supervisor-approved date; not verified |
| M8 — Remote-control boundary | Capability discovery, mock control demonstration, optional authorized provider exploration | Mock mode cannot actuate a real vehicle; capability and error states truthful; actual commands excluded unless separately verified | Karima lead; Aisha reviews service boundaries | Compatibility/authorization; security review | Pending supervisor-approved date; not verified |
| M9 — Integration and verification | End-to-end functional/security/reliability tests, selected performance checks, route recommendation research comparison | Executed test records, known defects and limitations; independent route rubric if research comparison is performed | Both | Working modules; consent/access where needed | Pending supervisor-approved date; not verified |
| M10 — Final documentation and delivery | Installation guide, README, diagrams, results, demo, risk closure and final presentation | Reviewed release tag, repository/YouTrack traceability, reproducible demonstration with known limitations | Both | M1–M9 | Pending supervisor-approved date; not verified |

## Dates, ownership, and tracking

Target dates for M4–M10, `pre-defense-2`, and `final-pre-defense` must be agreed with the supervisor and entered in YouTrack. No dates are supplied here because none have been verified. When agreed, replace the pending-date labels with the exact date and related YouTrack issue links. M4 and M5 are the next implementation milestones; each requires an assigned student, priority, due date, and specific acceptance criteria in YouTrack.

## Checkpoints and versioning

Use the submission tags required by the diploma process: `pre-defense-1`, `pre-defense-2`, and `final-pre-defense`. Create each tag only for the exact reviewed submission; do not recreate or move a published tag. Link the corresponding commit/tag and a summary of incomplete work in YouTrack.

## Tracking rules

- Create one YouTrack task for each meaningful deliverable or milestone subtask. Record an assignee, priority, acceptance criteria, dependency and agreed deadline.
- Use branches and commits containing the task ID where practical, then link the pull request back to YouTrack.
- Report a milestone as completed only after its listed acceptance evidence exists in the repository.
- Confirm absolute dates with the team and supervisor; the table intentionally does not invent deadlines or approvals.
- Update this plan when dependencies, scope or capacity change, preserving the explanation in Git history.
