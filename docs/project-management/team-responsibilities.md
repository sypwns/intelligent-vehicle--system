# JolSense — Team Responsibilities

Project: ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control  
Team: Shurenova Karima and Yerbolat Aisha  
Supervisor: Symbat Nurgalieva  
Status: Planned responsibilities; individual task assignments and review history are tracked in YouTrack.

## Allocation principle

Each student has a leading engineering area, but architecture, integration, safety review, testing and final release are shared. A lead is accountable for preparing a deliverable; it does not imply that implementation or verification has already occurred.

## Primary ownership

| Area / deliverable | Responsible lead | Supporting role | Expected evidence |
|---|---|---|---|
| Mobile application, Flutter UI and navigation | Shurenova Karima | Yerbolat Aisha: requirements and module data needs | Screens and Flutter source when implemented, UI acceptance checks |
| Vehicle monitoring dashboard and telemetry adapters | Shurenova Karima | Yerbolat Aisha: diagnostic input definition | Documented input contracts, replay tests, dashboard with freshness labels |
| Weather conditions, routing and travel-risk interface | Shurenova Karima | Yerbolat Aisha: health indicators and evaluation support | Source/provenance records, route scenarios, integration tests |
| Authorized control capability view and sandbox workflow | Shurenova Karima | Yerbolat Aisha: security/requirements review | Capability matrix, mock-mode and authorization tests |
| ML-based anomaly/fault diagnosis | Yerbolat Aisha | Shurenova Karima: UI/API integration | Dataset provenance, baseline, model card or equivalent, held-out test results when available |
| Diagnostic explanations and intelligent maintenance assistant | Yerbolat Aisha | Shurenova Karima: interface integration | Explained example outputs, maintenance scenarios, acceptance tests |
| Requirements specification | Yerbolat Aisha | Shurenova Karima: user flow and feasibility review | Requirements with identifiers, priorities and testable acceptance criteria |
| Architecture description, decisions and component diagram | Yerbolat Aisha | Shurenova Karima: mobile/backend/integration review | Markdown decisions, editable `.drawio`, exported PNG/SVG |
| Project proposal | Shurenova Karima | Yerbolat Aisha: technical scope check | Proposal with title, aims, users, scope and assumptions |
| Existing-system and technology comparisons | Shurenova Karima | Yerbolat Aisha: diagnostic and model options | Evidence-based comparison tables and justified choices |
| Literature review | Yerbolat Aisha | Shurenova Karima: weather/routing coverage review | Critical review with verified DOI/citation details |
| UX mockups and use-case diagram | Shurenova Karima | Yerbolat Aisha: diagnostic use cases | Editable source/link and readable exports |
| System context diagram | Shared | Both review actor/provider boundaries | One agreed editable source and readable export |
| Blizzard Survival Mode | Shared | Karima: UI, GPS, local check-ins and notification integration; Aisha: telemetry conditions, rule/test cases; both: Node.js heartbeat/consent and escalation review | Documented state flow, simulated positive/negative cases, consent and failure-handling evidence |
| Node.js backend and database integration | Shared until assigned in YouTrack | Define ownership per endpoint/service | API contracts, validation tests, environment placeholders and reviewed commits |
| Test planning, integration, privacy and final delivery | Shared | Each tests the other's changes where feasible | Test case records, defects, pull-request reviews, release/tag evidence |

## Collaboration workflow

1. Open a YouTrack issue with an owner, expected output, acceptance criteria, dependencies and deadline.
2. Implement or update documentation on a feature/task branch, with identifiable individual Git authorship.
3. Open a pull request describing the change, verification performed and remaining limits.
4. Have the other team member review before merging; request supervisor feedback for significant scope/safety decisions.
5. Attach the commit or pull-request link to YouTrack, then close the task only when its acceptance criteria are met.

## Escalation and sign-off

- Vehicle actuation, emergency escalation, personal location sharing, and any change that affects safety need explicit joint review and documented limitations.
- Node.js is the selected backend runtime. Specific framework, storage, hosting, and heartbeat scheduler choices are not yet confirmed by this document.
- Reassigning work is permitted if it is recorded in YouTrack and reflected in this file.
