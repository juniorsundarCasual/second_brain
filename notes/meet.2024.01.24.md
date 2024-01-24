---
id: tjznyukz4istqwooedmqt48
title: '24 January 2024'
desc: ''
updated: 1706103062237
created: 1706093996197
traitIds:
  - meetingNote
---

# Battery Monitor Discussion

## Attendees

- @Martin-Andreoni
- @Shamsa-Hamad
- @Solita

## Goals

- Identify merge point for the battery monitoring

## Agenda

- Sync up on how the battery monitor is implemented.
- How can @Solita support this model development?
- How the existing architecture can accommodate this addition?

## Minutes

- Going through the Battery Thresholding and Monitoring code.
- Is there a plan to get the confirmation dialogue to "return to base" or "emergency land" move to mission control UI?

## Action Items

- @Shamsa-Hamad will share the code through GitHub.
- Identify what sort of framework we need on MLOps
  - Kubernetes?
  - MLFlow?
- @Solita - Contact point for the person who knows most about how the ROS 2 sub/pub is taking place in existing architecture. 
  - To run ML models, data needs to be obtained from visible ROS 2 Topics, so we need to device a strategy to ensure that this data reaches the models in the most efficient way possible without causing bottlenecks and with minimal latency.
  - Immediate requirement is for the simulation side. Will need a contact point for the hardware implementation in the near future.
 
# RTA Meeting

## Attendees

- @SRTA-AD

## Goals

- Discuss progress

## Agenda

- Update JIRA

## Minutes

- General objectives of SSRC confirmed with Ticky
- Talks with @Solita initiated

## Action Items

- Determine if talks with someone is @Solita team is necessary
- Edit timelines and tasks in JIRA
- Join in on GATech kickoff
- Use Samridha's dataset from ARRC to edit the way they are being recorded.

# GATech Kick Off: Federated Learning Project

## Attendees

- @SRTA-AD
- @GATech

## Goals

- Discuss project

## Agenda

- Secure and Efficient Federated Learnign for Distributed Systems

## Minutes

- Many secure ML Schemes already developed
- Opportunity and challenges
  - cyber attacks and still commonplace
  - issues with machine learning systems: poisoning, transfer learning attack, data confidentiality
- Why is distributed learning important
- Principles to design attack-resilient distributed learning system
  - security
  - sustainability
  - robustness
- privacy: protection of sensitive data or information and anonymity of individual data during the collaborative process of training machine learning models.
  - differential privacy
  - multi-party computations
- evaluation of DLS:
  - learning system performance evaluation (ARCADE for intrusion detection and AFRL for jamming detection baseline models)
  - adaptability and robustness of distributed system
- evaluation of privacy aspects:
  - differential privacy analysis
  - membershep inference attacks
- Progressive neural network freezes previous model as new patterns are added and additional layers are trained.
- Drift aware incremental learning
  - Unsupervised anomaly detection use ML to build profile of benign distribution. Alert when new data deviates from profile. "**Concept Drift**".

## Action Items

- Look into:
  - Continual task learning
  - Catastrophic forgetting