# Project Proposal

## Project Title
ML-Based Intelligent Vehicle System for Monitoring, Diagnostics, Weather Safety, and Remote Control

## Team and Supervisor
- Students: Shurenova Karima and Yerbolat Aisha
- Group: SE-2405
- Supervisor: Symbat Nurgalieva

## Project Overview
We plan to develop a mobile application that combines vehicle monitoring, machine-learning-based diagnostics, maintenance assistance, weather-aware route recommendations, and remote control through supported interfaces.

The project is currently at the research and planning stage. The functions described below are planned capabilities.

## Problem Statement
Drivers may find it difficult to interpret diagnostic codes and sensor measurements when making maintenance decisions. Planning a journey also requires considering weather, road conditions, and the technical condition of the vehicle.

Our project will investigate how these sources of information can be combined to provide understandable maintenance and travel recommendations. Discussions with potential users have not yet been conducted, so user needs remain to be validated.

## Target Users
The intended users are drivers and vehicle owners, particularly those who need clear explanations of vehicle condition and support with maintenance and trip planning.

## Project Aim
To develop a mobile vehicle-management prototype combining monitoring, diagnostic assistance, maintenance planning, weather-aware routing, and supported remote-control functions.

## Research Aim
To evaluate whether incorporating vehicle-health indicators improves the appropriateness of weather-aware route recommendations compared with using weather and road-condition inputs alone across matched driving scenarios.

## Primary Research Question
How does adding vehicle-health indicators to weather and road-condition inputs affect route-recommendation appropriateness across the same defined driving scenarios?

## Research Objectives
1. Identify vehicle-health, weather, and road-condition variables that can be obtained or represented credibly, and define the evaluation scenarios.
2. Define a route-suitability rubric and obtain independent review of reference recommendations before comparing system outputs.
3. Develop two comparable recommendation configurations: environmental inputs alone and environmental inputs combined with vehicle-health indicators.
4. Evaluate both configurations on the same scenarios using recommendation agreement as the primary outcome and unsuitable-route selections as a supporting outcome.
5. Analyse paired differences and failure cases to identify where vehicle-health information helps, has no effect, or reduces recommendation appropriateness.

## Planned Functional Scope
- Display available vehicle-condition information.
- Apply machine learning to selected diagnostic tasks where suitable data are available.
- Explain diagnostic outputs in understandable language.
- Provide maintenance recommendations and personalised service planning.
- Assess weather-related travel conditions and recommend routes.
- Provide remote-control functions through supported interfaces, with available commands to be confirmed.

## Scope Boundaries
The prototype will support selected diagnostic tasks and defined evaluation scenarios. Supported vehicles, data sources, external services, and remote-control commands remain to be confirmed.

The research comparison focuses on route recommendations. It does not evaluate every application function or establish a reduction in real-world accidents.

Remote-control implementation depends on access to a suitable supported interface. Universal compatibility with all vehicles is outside the current scope.

## Planned Evaluation
We will compare:
1. Recommendations using weather and road-condition inputs.
2. Recommendations using the same inputs plus vehicle-health indicators.

Both configurations will process matched scenarios with the same candidate routes and shared settings. Recommendations will be compared with independently reviewed reference assessments.

Constructed scenarios will be identified explicitly and distinguished from observed journeys. No experimental results are available yet.

## Expected Outcomes
- A mobile application prototype demonstrating the selected functions.
- Diagnostic explanations and maintenance recommendations.
- Weather-aware route recommendations.
- A documented comparison of recommendations with and without vehicle-health inputs.
- Documentation of requirements, architecture, testing, and limitations.

These are intended deliverables, not completed results.

## Current Status
- Research focus, question, aim, and objectives have been drafted.
- Methodology preparation is in progress.
- A GitHub repository and YouTrack project have been created.
- User discussions have not yet been conducted.
- The technology stack and automotive dataset have not yet been selected.
- Implementation, independent assessment, and evaluation remain pending.
- Supervisor feedback has not yet been recorded in the submitted materials.

## Project Management
Work will be tracked through assigned YouTrack tasks. Changes will be developed in task branches and submitted through pull requests for review by the other team member.

Task for this document:
https://sypwns.youtrack.cloud/issue/IVS-2

## Decisions Still Required
- Validate user needs through discussions.
- Confirm mandatory prototype features and individual responsibilities.
- Select and justify the technology stack.
- Verify suitable automotive data and external services.
- Confirm supported remote-control interfaces and commands.
- Arrange independent review of the evaluation criteria.
- Obtain supervisor feedback on the proposal.
