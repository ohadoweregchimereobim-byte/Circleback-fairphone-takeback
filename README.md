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

## What It Does

### Consumer Side
- Submit a device return request through a clean, 
  mobile-friendly portal
- Receive an automated confirmation email with a 
  unique reference number
- Track the return in real time through a visual 
  progress indicator
- Get notified when the reward is ready

### Operational Side
- Intake Technician workspace with filtered queues 
  for expected devices and devices awaiting triage
- Automated triage routing — set a decision and 
  a Processing Job is created automatically
- Recycling Partner management with certification 
  tracking
- Reward fulfilment automation — job completes, 
  gift card issues, consumer gets notified

### Management Side
- Programme Dashboard with live operational metrics
- ESG Impact section tracking devices diverted 
  from landfill, weight processed, and materials 
  recovered

---

## Screenshots

### Consumer Portal
![Consumer Portal](screenshots/01-consumer-portal-impact-counter.png)

### Status Tracker — Complete Journey
![Status Tracker](screenshots/02-status-tracker-complete.png)

### Programme Dashboard with ESG Metrics
![Dashboard](screenshots/03-programme-dashboard-esg.png)

### Return Requests List
![Return Requests](screenshots/04-return-requests-list.png)

---

## Application Architecture

**Platform:** ServiceNow (Zurich release)  
**Scope:** x_1845356_circle_0  
**Built with:** Build Agent (17 structured prompts)

### Tables
- Return Request — consumer submissions
- Device Record — physical device intake
- Processing Job — refurbish, parts harvest, recycle
- Recycling Partner — certified recycler management
- Reward Fulfilment — gift card issuance and tracking

### Automation
- Business Rule: Triage Router — auto-creates 
  Processing Jobs on triage decision
- Business Rule: Submission Date — fixes scoped 
  datetime on record creation
- Flow: Return Confirmation Email — sends branded 
  confirmation and advances status
- Flow: Reward Fulfilment Trigger — issues reward 
  on job completion

### Portal
- Service Portal with two pages: Return Your Device 
  and Track Your Return
- Live impact counter showing real programme metrics

---

## How It Was Built

The domain expertise came from academic research 
on e-waste management in consumer electronics 
comparing Apple, Fairphone, and Dell for an MSc 
Engineering Management module at the University 
of South Wales.

Every Build Agent prompt was designed for a specific 
component with a role context header. The full 
prompt set is in the build-agent-prompts folder.

---

## Build Agent Prompts

See [build-agent-prompts/prompts.md](build-agent-prompts/prompts.md) 
for the full set of structured prompts used to 
build this application.

---

## Live Application

Built on a ServiceNow Personal Developer Instance.  
Portal URL: https://dev395191.service-now.com/circleback

---

## Tags

ServiceNow · Build Agent · E-Waste · Circular Economy · 
Fairphone · MSc Engineering Management · 
#BuildMoreWithBuildAgent
