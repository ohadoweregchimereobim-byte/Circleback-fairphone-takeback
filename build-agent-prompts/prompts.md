# CircleBack — Build Agent Prompts

These are the exact prompts used to build CircleBack.
Not cleaned up versions. The actual ones that ran.

Each prompt uses a role context header. This 
consistently produced better output than prompts 
without it. Work through them in order and always 
specify your app scope from Call 2 onwards.

---

## Call 1 — App Scaffolding

```
Act as a Senior ServiceNow Solutions Architect 
with deep experience designing enterprise 
applications on the ServiceNow platform.

Create a new ServiceNow application called 
"CircleBack - Fairphone Take-Back Hub". This app 
manages Fairphone's consumer electronics take-back 
programme. It handles consumer return requests, 
device intake and triage, recycling partner 
coordination, reward fulfilment, and ESG impact 
reporting.

Set up the application scope and create a basic 
navigation menu with the following modules:
- Return Requests
- Device Records
- Processing Jobs
- Recycling Partners
- Reward Fulfilment
- Programme Dashboard

Use green as the primary theme colour to reflect 
sustainability. Follow ServiceNow best practices 
throughout including proper scoping, naming 
conventions, and table structure.
```

---

## Call 2 — Return Request Table

```
Act as a Senior ServiceNow Data Architect with 
expertise in designing clean, scalable data models 
for enterprise service management applications.

In the CircleBack application (scope: x_1845356_circle_0), 
create a table with the name "return_request" so it 
registers as x_1845356_circle_0_return_request. 
This table stores consumer device return requests 
for Fairphone's take-back programme.

Add the following fields:

- Return Reference (auto-generated, read-only, 
  format: CB-0001, used as display value)
- Consumer Name (text, mandatory)
- Consumer Email (email, mandatory)
- Consumer Phone (phone number, optional)
- Device Type (choice: Smartphone, Tablet, Laptop, 
  Headphones, Other)
- Device Model (text, mandatory)
- Device Condition (choice: Working, Minor Damage, 
  Major Damage, Unknown)
- Postal Address (text area, mandatory)
- Submission Date (date/time, auto-set on creation)
- Status (choice: Submitted, Shipping Label Sent, 
  Device Received, In Processing, Complete)
- Notes (text area, optional)

Set default Status to Submitted. Make Return 
Reference the display value. Enable auditing and 
web service access. Follow ServiceNow naming 
conventions and best practices throughout.
```

---

## Call 3 — Device Record Table

```
Act as a Senior ServiceNow Data Architect with 
expertise in designing clean, scalable data models 
for enterprise service management applications.

In the CircleBack application (scope: x_1845356_circle_0), 
create a table with the name "device_record" so it 
registers as x_1845356_circle_0_device_record. This 
table stores physical device records once a returned 
device arrives at the processing centre.

Add the following fields:

- Device ID (auto-generated, read-only, format: 
  DEV-0001, used as display value)
- Return Request (reference to 
  x_1845356_circle_0_return_request, mandatory)
- Date Received (date/time, mandatory)
- Received By (reference to sys_user, mandatory)
- Actual Condition (choice: Working, Repairable, 
  Parts Only, Scrap, mandatory)
- Triage Decision (choice: Refurbish, Spare Parts 
  Harvest, Recycle, Pending)
- Serial Number (text, optional)
- Technician Notes (text area, optional)
- Weight KG (decimal, optional)
- Triage Completed (true/false, default false)

Set default Triage Decision to Pending. Make Device 
ID the display value. Enable auditing and web service 
access. Follow ServiceNow naming conventions and 
best practices throughout.
```

---

## Call 4 — Processing Job Table

```
Act as a Senior ServiceNow Data Architect with 
expertise in designing clean, scalable data models 
for enterprise service management applications.

In the CircleBack application (scope: x_1845356_circle_0), 
create a table with the name "processing_job" so it 
registers as x_1845356_circle_0_processing_job. This 
table tracks the actual work done on each returned 
device — refurbishment, parts harvesting, or recycling.

Add the following fields:

- Job Reference (auto-generated, read-only, 
  format: JOB-0001, used as display value)
- Device Record (reference to 
  x_1845356_circle_0_device_record, mandatory)
- Job Type (choice: Refurbish, Spare Parts Harvest, 
  Recycle, mandatory)
- Assigned To (reference to sys_user, optional)
- Recycling Partner (reference to 
  x_1845356_circle_0_recycling_partner, optional)
- Status (choice: Open, In Progress, Complete, 
  Cancelled)
- Start Date (date/time, optional)
- Completion Date (date/time, optional)
- Outcome Notes (text area, optional)
- Materials Recovered KG (decimal, optional)
- Value Recovered GBP (decimal, optional)

Set default Status to Open. Make Job Reference the 
display value. Enable auditing and web service access. 
Follow ServiceNow naming conventions and best 
practices throughout.
```

---

## Call 5 — Recycling Partner Table

```
Act as a Senior ServiceNow Data Architect with 
expertise in designing clean, scalable data models 
for enterprise service management applications.

In the CircleBack application (scope: x_1845356_circle_0), 
create a table with the name "recycling_partner" so 
it registers as x_1845356_circle_0_recycling_partner. 
This table stores certified recycling partner 
organisations used in the take-back programme.

Add the following fields:

- Partner Name (text, mandatory, used as display value)
- Certification (choice: R2v3, e-Stewards, 
  ISO 14001, Multiple, None)
- Country (text, mandatory)
- Contact Name (text, optional)
- Contact Email (email, optional)
- Contact Phone (phone number, optional)
- Accepted Device Types (text area, optional)
- Active (true/false, default true)
- Notes (text area, optional)
- Date Added (date, auto-set on creation)

Make Partner Name the display value. Enable auditing 
and web service access.

After creating the table, add three sample records:
- Partner Name: GreenCycle UK, Certification: R2v3, 
  Country: United Kingdom, Active: true
- Partner Name: EcoReclaim NL, Certification: 
  e-Stewards, Country: Netherlands, Active: true
- Partner Name: TechRecycle DE, Certification: 
  ISO 14001, Country: Germany, Active: true
```

---

## Call 6 — Reward Fulfilment Table

```
Act as a Senior ServiceNow Data Architect with 
expertise in designing clean, scalable data models 
for enterprise service management applications.

In the CircleBack application (scope: x_1845356_circle_0), 
create a table with the name "reward_fulfilment" so 
it registers as x_1845356_circle_0_reward_fulfilment. 
This table manages consumer rewards issued when their 
returned device has been successfully processed.

Add the following fields:

- Reward Reference (auto-generated, read-only, 
  format: RWD-0001, used as display value)
- Return Request (reference to 
  x_1845356_circle_0_return_request, mandatory)
- Reward Type (choice: Gift Card, Discount Code, 
  Donation to Charity, default: Gift Card)
- Reward Value EUR (decimal, mandatory, default: 20)
- Reward Code (text, read-only, optional)
- Status (choice: Pending, Issued, Redeemed, 
  Expired, default: Pending)
- Issue Date (date/time, optional)
- Expiry Date (date, optional)
- Notes (text area, optional)

Make Reward Reference the display value. Enable 
auditing and web service access.
```

---

## Call 7 — Table Relationships

```
Act as a Senior ServiceNow Data Architect with 
expertise in designing clean, scalable data models 
for enterprise service management applications.

In the CircleBack application (scope: x_1845356_circle_0), 
set up related lists and form configurations to wire 
all five tables together properly.

1. On the Return Request form: add a related list 
   showing all Device Records linked to that request 
   via the return_request reference field

2. On the Return Request form: add a related list 
   showing the Reward Fulfilment record linked to 
   that request via the return_request reference field

3. On the Device Record form: add a related list 
   showing all Processing Jobs linked to that device 
   via the device_record reference field

4. On the Recycling Partner form: add a related list 
   showing all Processing Jobs assigned to that partner 
   via the recycling_partner reference field

5. On the Processing Job form: ensure the Device 
   Record field shows the linked Return Request 
   reference as additional context

6. Create clean default list views for each table:
   - Return Request: Return Reference, Consumer Name, 
     Device Model, Status, Submission Date
   - Device Record: Device ID, Return Request, 
     Actual Condition, Triage Decision, Date Received
   - Processing Job: Job Reference, Job Type, Status, 
     Assigned To, Completion Date
   - Recycling Partner: Partner Name, Country, 
     Certification, Active
   - Reward Fulfilment: Reward Reference, Return 
     Request, Reward Type, Status, Issue Date
```

---

## Call 8 — Consumer Portal Return Form

```
Act as a Senior ServiceNow UX Developer with 
expertise in building consumer-facing Service 
Portal experiences that are clean, accessible, 
and intuitive.

In the CircleBack application (scope: x_1845356_circle_0), 
create a Service Portal with a page called 
"Return Your Device". This is a public-facing form 
where consumers submit a device return request.

Portal ID: circleback
Portal title: CircleBack by Fairphone
Primary colour: green (#2E7D32)
Homepage set to the return form page

The return form should collect:
- Full Name (mandatory)
- Email Address (mandatory)
- Phone Number (optional)
- Device Type (dropdown: Smartphone, Tablet, 
  Laptop, Headphones, Other)
- Device Model (text, placeholder: "e.g. Fairphone 
  6, Fairphone 5")
- Device Condition (dropdown: Working - powers on 
  normally, Minor Damage - some wear but functional, 
  Major Damage - does not power on, Unknown)
- Postal Address (text area, mandatory)
- Agreement checkbox: "I confirm I have backed up 
  my data and will remove my SIM card before sending"

On successful submission:
- Create a Return Request record
- Display confirmation message showing Return 
  Reference number
- Tell consumer to check email for shipping 
  instructions

Page header: "Return Your Device"
Subheading: "Return your old device. We will 
handle the rest."

Clean, green and white, mobile responsive. Follow 
Service Portal best practices throughout.
```

---

## Call 9 — Return Status Tracker

```
Act as a Senior ServiceNow UX Developer with 
expertise in building consumer-facing Service 
Portal experiences that are clean, accessible, 
and intuitive.

In the CircleBack Service Portal, create a second 
page called "Track Your Return". Page ID: cb_track

Create a widget called "CircleBack Track Widget" with:

A single search input field where the consumer 
enters their Return Reference number (format CB-XXXX).
A search button labelled "Track My Return".

On search, query the x_1845356_circle_0_return_request 
table for a matching record. Display if found:

- Return Reference
- Device Model
- Submission Date
- Current Status with visual step indicator:
  Submitted > Device Received > In Processing 
  > Complete (current stage highlighted green)
- Dynamic status message:
  Submitted: "We have your request. Check your 
  email for your shipping label."
  Device Received: "Your device has arrived 
  safely with us."
  In Processing: "We are assessing your device. 
  Your reward will be issued soon."
  Complete: "All done. Check your email for 
  your reward details."

If no record found: "We could not find that 
reference. Please check and try again."

Add navigation link in portal header so consumers 
can switch between both pages.

Match green and white styling. Mobile responsive.
```

---

## Call 10 — Technician Workspace

```
Act as a Senior ServiceNow Developer with expertise 
in building role-based workspaces and operational 
interfaces for frontline staff.

In the CircleBack application (scope: x_1845356_circle_0), 
create a workspace for the Intake Technician role.

First create a role called "circleback_technician" 
in the x_1845356_circle_0 scope.

Then create a workspace with the following:

1. A homepage dashboard showing two queues:

Queue 1 - "Devices Expected"
List of Return Requests with status = 
"Shipping Label Sent" showing columns:
Return Reference, Consumer Name, Device Model, 
Submission Date

Queue 2 - "Awaiting Triage"
List of Device Records with Triage Decision = 
"Pending" showing columns:
Device ID, Return Request, Actual Condition, 
Date Received

2. A "Log Received Device" action that opens 
a form with these fields:
- Return Reference (reference lookup, mandatory)
- Date Received (date/time, defaults to now)
- Serial Number (text, optional)
- Weight KG (decimal, optional)
- Actual Condition (choice, mandatory)
- Technician Notes (text area, optional)

When saved:
- Create a new Device Record linked to the 
  selected Return Request
- Update the linked Return Request status 
  to "Device Received"

Style with green header consistent with 
CircleBack brand.
```

---

## Call 11 — Triage Router + Submission Date + Confirmation Email

Note: This combined three components into one call 
after session drops consumed earlier attempts. 
This is the exact prompt that successfully built 
all three.

```
Act as a Senior ServiceNow Developer and Flow 
Designer Specialist working on the CircleBack - 
Fairphone Take-Back Hub application 
(scope: x_1845356_circle_0).

Please complete two tasks in this session:

TASK 1 - Business Rules:

Create a business rule called "CircleBack Triage 
Router" on x_1845356_circle_0_device_record:
- Trigger: after insert and after update
- Condition: triage_decision changed AND 
  triage_decision is not empty AND 
  triage_completed = false
- Logic: create a Processing Job linked to the 
  Device Record with Job Type matching the 
  Triage Decision (Refurbish, Spare Parts Harvest, 
  or Recycle), Status = Open, Start Date = now
- Then set triage_completed = true on the Device 
  Record and update the linked Return Request 
  status to "In Processing"

Create a business rule called "CircleBack Set 
Submission Date" on 
x_1845356_circle_0_return_request:
- Trigger: before insert
- Logic: set submission_date = new GlideDateTime()

TASK 2 - Flow:

Create a Flow called "CircleBack Return 
Confirmation Email":
- Trigger: Record Created on 
  x_1845356_circle_0_return_request 
  where Status = Submitted
- Step 1: Wait 30 seconds
- Step 2: Send email to consumer_email with 
  subject "CircleBack by Fairphone - Return 
  Request Confirmed" and body including 
  consumer_name, return_reference, instructions 
  to await shipping label, and tracking URL 
  https://dev395191.service-now.com/circleback?id=cb_track
- Step 3: Update Return Request status to 
  "Shipping Label Sent"

Build and install both tasks to the instance.
```

---

## Call 12 — Reward Fulfilment Flow

```
Act as a Senior ServiceNow Flow Designer Specialist 
with expertise in building automated fulfilment 
workflows on the ServiceNow platform.

In the CircleBack application (scope: x_1845356_circle_0), 
create a Flow called "CircleBack - Reward Fulfilment 
Trigger".

Flow trigger:
- Table: x_1845356_circle_0_processing_job
- Trigger type: Record Updated
- Condition: Status changes to Complete

Flow steps in order:

Step 1 - Get the linked Device Record from the 
Processing Job, then get the linked Return Request 
from that Device Record

Step 2 - Create a new Reward Fulfilment record with:
- Return Request = the linked Return Request
- Reward Type = Gift Card
- Reward Value EUR = 20
- Status = Issued
- Issue Date = now
- Expiry Date = 90 days from now
- Reward Code = "CB-REWARD-" followed by the 
  first 5 characters of the Processing Job sys_id

Step 3 - Send email to the consumer_email on 
the Return Request with:
- Subject: "Your CircleBack Reward is Ready"
- Body: thank the consumer by name, confirm their 
  device has been processed, show their Reward Code 
  and expiry date, tell them to redeem at 
  fairphone.com/shop

Step 4 - Update the Return Request status to Complete

Build and install to the instance. Ensure the 
reward is only created once per Processing Job.
```

---

## Call 13 — Programme Dashboard

```
Act as a Senior ServiceNow Analytics Developer 
with expertise in building executive dashboards 
and ESG reporting interfaces on the ServiceNow 
platform.

In the CircleBack application (scope: x_1845356_circle_0), 
create a Performance Analytics dashboard called 
"CircleBack Programme Dashboard" with two sections:

SECTION 1 - Operational Metrics:

1. Total Return Requests (count of all records 
   in x_1845356_circle_0_return_request)

2. Returns by Status (donut chart showing 
   breakdown: Submitted, Shipping Label Sent, 
   Device Received, In Processing, Complete)

3. Processing Jobs by Type (bar chart: 
   Refurbish vs Spare Parts Harvest vs Recycle)

4. Rewards Issued (count of Reward Fulfilment 
   records with Status = Issued or Redeemed)

SECTION 2 - ESG Impact Metrics:

Label this section "ESG Impact"

5. Total Devices Diverted from Landfill 
   (count of Processing Jobs with Status = Complete)

6. Total Weight Processed KG 
   (sum of weight_kg from device_record)

7. Total Materials Recovered KG 
   (sum of materials_recovered_kg from processing_job)

8. Devices by Outcome (bar chart showing 
   Refurbished vs Parts Harvested vs Recycled)

Use green colour scheme throughout. Set this 
dashboard as the target for the Programme 
Dashboard navigation module.
```

---

## Call 14 — Impact Counter Widget

```
Act as a Senior ServiceNow UX Developer with 
expertise in building consumer-facing Service 
Portal components that communicate impact.

In the CircleBack application (scope: x_1845356_circle_0), 
update the existing circleback Service Portal 
return form page (cb_return) to add a live 
impact counter section above the return form.

Add a new widget called "CircleBack Impact Counter".

Three live statistics pulled from real data:

Counter 1 - "Devices Returned"
Count of all Return Request records with 
status = Complete

Counter 2 - "KG Diverted from Landfill"
Sum of weight_kg field from all records in 
x_1845356_circle_0_device_record

Counter 3 - "Rewards Issued"
Count of records in x_1845356_circle_0_reward_fulfilment 
where status = Issued or Redeemed

Style as three bold stat cards side by side:
- Large bold green number
- Small label underneath
- Light green background card
- Icons using unicode: ♻️ ⚖️ 🎁

Caption: "Together we are closing the loop on e-waste."

Green and white, mobile responsive.
```

---

## Call 15 — UI Polish

```
Act as a Senior ServiceNow UX/UI Designer with 
expertise in creating cohesive, professional 
application experiences on the ServiceNow platform.

Review and polish the CircleBack application 
(scope: x_1845356_circle_0):

1. Ensure consistent green colour scheme across 
   all portal pages

2. Add a simple footer to both portal pages 
   (cb_return and cb_track):
   "CircleBack is Fairphone's take-back programme. 
   Helping close the loop on e-waste."

3. Verify progress indicator labels on tracker page:
   Submitted > Device Received > In Processing 
   > Complete

4. Active state styling on portal navigation links

5. ESG Impact section heading clearly visible and 
   separated from Operational Metrics on dashboard

6. Fix Submission Date on CB0002 to today
```

---

## Call 16 — Test Data

```
Act as a Senior ServiceNow Developer with expertise 
in data management and application testing on the 
ServiceNow platform.

In the CircleBack application (scope: x_1845356_circle_0), 
populate the application with realistic test data.

Create 4 Return Request records:

Record 1:
- Consumer Name: Sarah Mitchell
- Device Model: Fairphone 5
- Device Type: Smartphone
- Status: Shipping Label Sent
- Submission Date: today

Record 2:
- Consumer Name: James Okafor
- Device Model: Dell XPS 13
- Device Type: Laptop
- Status: Device Received
- Submission Date: today

Record 3:
- Consumer Name: Priya Sharma
- Device Model: Fairphone 4
- Device Type: Smartphone
- Status: In Processing
- Submission Date: today

Record 4:
- Consumer Name: Tom Brennan
- Device Model: Fairbuds XL
- Device Type: Headphones
- Status: Complete
- Submission Date: today

Create 1 Device Record linked to James Okafor:
- Actual Condition: Repairable
- Triage Decision: Refurbish
- Weight KG: 1.8
- Triage Completed: true

Create 1 Processing Job linked to that Device Record:
- Job Type: Refurbish
- Status: Complete
- Materials Recovered KG: 1.2
- Value Recovered GBP: 45

Create 1 Reward Fulfilment linked to James Okafor:
- Reward Type: Gift Card
- Reward Value EUR: 20
- Status: Issued
- Reward Code: CB-REWARD-JO001

Update CB0002: submission date today, 
status Shipping Label Sent.

Build and install all records to the instance.
```

---

## Prompting Strategy — What Actually Worked

**Role context every time.** Starting with "Act as 
a Senior ServiceNow [specific role]" raised the 
quality bar consistently. A Solutions Architect 
prompt thinks about structure. A UX Developer 
prompt thinks about user experience. The role 
shapes the output.

**One component per call where possible.** Focused 
prompts produce cleaner builds. When session drops 
forced combinations, the output was harder to 
verify and debug.

**Specify scope explicitly every call.** After the 
app was created in Call 1, every subsequent prompt 
included the full scope string. This prevented 
assets being created in the wrong application.

**Save buffer calls.** Out of 25 available calls, 
plan for 17-18 productive calls and hold the rest. 
PDI hibernation and session drops will consume 
calls unexpectedly. Two attempts were lost to 
connection issues during this build.

**Screenshot after every call.** Before moving on, 
screenshot what was built. These are your 
submission evidence and your rollback reference.

**Build Agent self-corrects.** Several times during 
this build, Build Agent encountered errors and 
fixed them within the same call without any 
intervention. Trust the process but verify the 
output before moving to the next call.
