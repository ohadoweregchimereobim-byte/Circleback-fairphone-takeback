# CircleBack — Data Model

## Application Details

| Field | Value |
|---|---|
| Application Name | CircleBack - Fairphone Take-Back Hub |
| Scope | x_1845356_circle_0 |
| Platform | ServiceNow Zurich |
| Created | May 2026 |

---

## Tables Overview

Five custom tables connected through reference 
fields to manage the complete device return lifecycle.

```
Return Request
    │
    ├── Device Record
    │       │
    │       └── Processing Job
    │               │
    │               └── Recycling Partner
    │
    └── Reward Fulfilment
```

---

## Table 1 — Return Request

`x_1845356_circle_0_return_request`

Stores every consumer device return submission. 
This is the anchor record that everything else 
links back to.

| Field | Type | Required | Notes |
|---|---|---|---|
| Return Reference | String (auto) | Yes | CB-0001 format, display value |
| Consumer Name | String | Yes | |
| Consumer Email | Email | Yes | Used for automated emails |
| Consumer Phone | Phone | No | |
| Device Type | Choice | No | Smartphone, Tablet, Laptop, Headphones, Other |
| Device Model | String | Yes | Free text e.g. Fairphone 6 |
| Device Condition | Choice | No | Working, Minor Damage, Major Damage, Unknown |
| Postal Address | String (multi) | Yes | Used for shipping label generation |
| Submission Date | DateTime | No | Auto-set on creation via business rule |
| Status | Choice | No | See lifecycle below |
| Notes | String (multi) | No | |

**Status Lifecycle:**
Submitted → Shipping Label Sent → Device Received 
→ In Processing → Complete

**Automation:**
- Confirmation email sent on creation (Flow)
- Status advances automatically through business 
  rules and flows

---

## Table 2 — Device Record

`x_1845356_circle_0_device_record`

Created when a physical device arrives at the 
processing centre. Linked to its originating 
Return Request.

| Field | Type | Required | Notes |
|---|---|---|---|
| Device ID | String (auto) | Yes | DEV-0001 format, display value |
| Return Request | Reference | Yes | Links to Return Request |
| Date Received | DateTime | Yes | |
| Received By | Reference | Yes | sys_user |
| Actual Condition | Choice | Yes | Working, Repairable, Parts Only, Scrap |
| Triage Decision | Choice | No | Refurbish, Spare Parts Harvest, Recycle, Pending |
| Serial Number | String | No | |
| Technician Notes | String (multi) | No | |
| Weight KG | Decimal | No | Used in ESG metrics |
| Triage Completed | Boolean | No | Default false, set true by Triage Router |

**Automation:**
- Creating a Device Record updates the linked 
  Return Request status to Device Received
- Setting Triage Decision triggers the Triage 
  Router business rule

---

## Table 3 — Processing Job

`x_1845356_circle_0_processing_job`

Tracks the actual work done on a returned device. 
Created automatically by the Triage Router 
business rule.

| Field | Type | Required | Notes |
|---|---|---|---|
| Job Reference | String (auto) | Yes | JOB-0001 format, display value |
| Device Record | Reference | Yes | Links to Device Record |
| Job Type | Choice | Yes | Refurbish, Spare Parts Harvest, Recycle |
| Assigned To | Reference | No | sys_user |
| Recycling Partner | Reference | No | Links to Recycling Partner |
| Status | Choice | No | Open, In Progress, Complete, Cancelled |
| Start Date | DateTime | No | Set on creation |
| Completion Date | DateTime | No | |
| Outcome Notes | String (multi) | No | |
| Materials Recovered KG | Decimal | No | Used in ESG metrics |
| Value Recovered GBP | Decimal | No | Used in ESG metrics |

**Status Lifecycle:**
Open → In Progress → Complete

**Automation:**
- Created automatically when Triage Decision 
  is set on a Device Record
- Completing a job triggers the Reward 
  Fulfilment Flow

---

## Table 4 — Recycling Partner

`x_1845356_circle_0_recycling_partner`

Certified recycling organisations used when 
Triage Decision = Recycle.

| Field | Type | Required | Notes |
|---|---|---|---|
| Partner Name | String | Yes | Display value |
| Certification | Choice | No | R2v3, e-Stewards, ISO 14001, Multiple, None |
| Country | String | Yes | |
| Contact Name | String | No | |
| Contact Email | Email | No | |
| Contact Phone | Phone | No | |
| Accepted Device Types | String (multi) | No | |
| Active | Boolean | No | Default true |
| Notes | String (multi) | No | |
| Date Added | Date | No | Auto-set on creation |

**Sample Data:**
- GreenCycle UK — R2v3 — United Kingdom
- EcoReclaim NL — e-Stewards — Netherlands
- TechRecycle DE — ISO 14001 — Germany

---

## Table 5 — Reward Fulfilment

`x_1845356_circle_0_reward_fulfilment`

Created automatically when a Processing Job 
is marked Complete. Manages the consumer 
reward end to end.

| Field | Type | Required | Notes |
|---|---|---|---|
| Reward Reference | String (auto) | Yes | RWD-0001 format, display value |
| Return Request | Reference | Yes | Links to Return Request |
| Reward Type | Choice | No | Gift Card, Discount Code, Donation to Charity |
| Reward Value EUR | Decimal | Yes | Default 20 |
| Reward Code | String | No | Auto-generated, read-only |
| Status | Choice | No | Pending, Issued, Redeemed, Expired |
| Issue Date | DateTime | No | Set on creation |
| Expiry Date | Date | No | 90 days from issue |
| Notes | String (multi) | No | |

**Automation:**
- Created automatically by Reward Fulfilment Flow
- Consumer notified by email on creation

---

## Relationships Summary

| From | To | Type | Field |
|---|---|---|---|
| Device Record | Return Request | Many to One | return_request |
| Processing Job | Device Record | Many to One | device_record |
| Processing Job | Recycling Partner | Many to One | recycling_partner |
| Reward Fulfilment | Return Request | One to One | return_request |

---

## Business Rules

| Name | Table | Trigger | What It Does |
|---|---|---|---|
| CircleBack Triage Router | Device Record | After insert/update | Creates Processing Job on triage decision |
| CircleBack Set Submission Date | Return Request | Before insert | Sets submission_date = GlideDateTime() |
| CircleBack Update Return Request on Device Intake | Device Record | After insert | Updates Return Request to Device Received |

---

## Flows

| Name | Trigger | What It Does |
|---|---|---|
| CircleBack Return Confirmation Email | Return Request created | Sends confirmation email, advances to Shipping Label Sent |
| CircleBack Reward Fulfilment Trigger | Processing Job status = Complete | Creates reward record, emails consumer, advances to Complete |

---

## ESG Metrics Sources

The Programme Dashboard ESG Impact section 
pulls from these fields:

| Metric | Source | Field |
|---|---|---|
| Devices Diverted | Processing Job | Count where status = Complete |
| Weight Processed KG | Device Record | Sum of weight_kg |
| Materials Recovered KG | Processing Job | Sum of materials_recovered_kg |
| Value Recovered GBP | Processing Job | Sum of value_recovered_gbp |
| Rewards Issued | Reward Fulfilment | Count where status = Issued or Redeemed |
