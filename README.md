# CircleBack — Fairphone Take-Back Hub

A ServiceNow application that manages Fairphone's 
consumer electronics take-back programme end to end.

Built for the #BuildMoreWithBuildAgent challenge 
using ServiceNow Build Agent on a PDI.

---

## The Problem

E-waste hit 62 million tonnes in 2022. Less than 
23% was formally recycled. Companies like Fairphone 
run take-back programmes but manage them through 
emails, spreadsheets, and manual processes. There 
is no unified workflow system handling the journey 
from consumer return request through to device 
processing and reward fulfilment.

CircleBack is that system.

---

## Screenshots

### Consumer Portal with Live Impact Counter
![Consumer Portal](https://raw.githubusercontent.com/ohadoweregchimereobim-byte/Circleback-fairphone-takeback/main/screenshots/Consumer%20Portal.png)

### Status Tracker — Complete Journey
![Status Tracker](https://raw.githubusercontent.com/ohadoweregchimereobim-byte/Circleback-fairphone-takeback/main/screenshots/Status%20Tracker%20-%20Complete%20Journey.png)

### Programme Dashboard with ESG Metrics
![Dashboard](https://raw.githubusercontent.com/ohadoweregchimereobim-byte/Circleback-fairphone-takeback/main/screenshots/Programme%20Dashboard%20with%20ESG%20Metrics.png)

### Return Requests List
![Return Requests](https://raw.githubusercontent.com/ohadoweregchimereobim-byte/Circleback-fairphone-takeback/main/screenshots/Return%20Requests%20List.png)

---

## What It Does

### Consumer Side
- Submit a device return request through a clean 
  mobile-friendly portal
- Receive an automated confirmation email with a 
  unique reference number (CB-XXXX format)
- Track the return in real time through a visual 
  progress indicator showing four stages
- Get notified by email when the reward is ready

### Operational Side
- Intake Technician workspace with two filtered 
  queues — devices expected and devices awaiting 
  triage
- Automated triage routing — technician sets a 
  decision and a Processing Job is created 
  automatically
- Recycling Partner management with certification 
  tracking (R2v3, e-Stewards, ISO 14001)
- Reward fulfilment automation — Processing Job 
  completes, gift card issues, consumer gets 
  notified

### Management Side
- Programme Dashboard with live operational metrics
- ESG Impact section tracking devices diverted 
  from landfill, weight processed, and materials 
  recovered
- Returns by Status donut chart
- Processing Jobs by Type bar chart

---

## Application Architecture

**Platform:** ServiceNow Australia  
**Scope:** x_1845356_circle_0  
**Built with:** ServiceNow Build Agent (17 prompts)  
**Portal:** https://dev395191.service-now.com/circleback

### Tables

| Table | Purpose |
|---|---|
| Return Request | Consumer device submissions |
| Device Record | Physical device intake and triage |
| Processing Job | Refurbish, parts harvest, or recycle work |
| Recycling Partner | Certified recycler management |
| Reward Fulfilment | Gift card issuance and tracking |

### Automation

| Component | Type | What It Does |
|---|---|---|
| CircleBack Triage Router | Business Rule | Creates Processing Job on triage decision |
| CircleBack Set Submission Date | Business Rule | Fixes scoped datetime on record creation |
| CircleBack Return Confirmation Email | Flow | Sends confirmation email, advances status |
| CircleBack Reward Fulfilment Trigger | Flow | Issues reward on job completion |

### Portal Pages

| Page | Purpose |
|---|---|
| Return Your Device | Consumer return submission form |
| Track Your Return | Real time status tracker |

---

## How It Was Built

The domain expertise came from academic research 
comparing how Apple, Fairphone, and Dell manage 
e-waste for an MSc Engineering Management module 
at the University of South Wales.

The single biggest operational gap identified 
across all three companies was reverse logistics — 
the absence of a unified workflow system managing 
consumer device returns from submission through 
triage, processing, and reward fulfilment. 
CircleBack closes that gap.

Every Build Agent prompt was written for a specific 
component with a role context header to maximise 
output quality. The full prompt set is in the 
build-agent-prompts folder.

---

## Background

This project sits at the intersection of two things 
I have been working on simultaneously — my MSc 
Engineering Management dissertation on supply chain 
resilience, and my ServiceNow certification journey 
through the RiseUp with ServiceNow programme.

CircleBack is what happens when academic research 
meets a real platform and a deadline.

---

## Tags

`ServiceNow` `Build Agent` `E-Waste` `Circular Economy` 
`Fairphone` `MSc Engineering Management` 
`RiseUp with ServiceNow` `#BuildMoreWithBuildAgent`
