# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded by PRD N-17 and is used only as history; nothing in this intent is approved by it alone.

## Problem

- **I-1** The fictional clinic receives avoidable calls from patients checking whether their results are ready. (PRD: "The fictional clinic receives avoidable calls from patients checking whether results are ready.")

## Proposed outcome

- **I-2** Notify the patient when a result is ready for their review in the existing authenticated portal. (PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal.")
- **I-3** A result is "ready" only after a doctor has reviewed it and released it. Lab upload alone does not make a result ready. (PRD: "A doctor must review and release the result before any notification is eligible."; notes, product owner: "'Ready' must not mean that the laboratory upload merely finished.")

### How success will be observed

- **I-4** Results-status calls: count status-only calls separately from clinical questions and compare them against a measured baseline. (PRD: "Measure reduction in results-status calls and delivery failures."; notes, support lead: "count status-only calls separately from clinical questions; compare against a measured baseline.")
- **I-5** Delivery failures are counted. (PRD: "Measure reduction in results-status calls and delivery failures.")
- **I-6** No baseline or numeric target is approved yet; both are to be agreed during discovery. This intent sets no target. (PRD: "The baseline and numerical target will be agreed during discovery; none is approved yet.")

## Affected users and systems

- **I-7** Patients: the only approved recipients of the notification. (PRD: "Notify the patient")
- **I-8** Doctors: their review and release of a result is the gate for any notification.
- **I-9** Support staff: the status-only calls they handle are the main success measure.
- **I-10** The existing authenticated portal and its existing result route: the notification points the patient there. (PRD: "Use the existing portal result route."; notes, engineering lead: "the existing portal path is adequate.")
- **I-11** Notification delivery and notification logs: subject to the content rule in I-13.
- The referring doctor is **not** an approved recipient; see OPEN-2.

## Out of scope

- **I-12** Excluded from this work (PRD: "Portal redesign, appointment scheduling, clinical interpretation and notifying third parties are outside this approved scope."; notes, engineering lead: "A new patient dashboard is a separate project."):
  - Portal redesign.
  - A new or rebuilt patient dashboard (a separate project).
  - Appointment scheduling.
  - Clinical interpretation of results.
  - Notifying third parties.
  - Anything requested only by the superseded request: sending results as soon as the lab uploads them, including the test result in an SMS, copying the referring doctor, rebuilding the patient dashboard and a 90% call-reduction target in one week.

## Constraints

- **I-13** The message says only that an update is available and directs the patient to the existing authenticated portal. No diagnosis, test result, clinical measurement or treatment detail goes in the message or in notification or operational logs. (PRD: "No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs."; notes, privacy owner: "no sensitive content in messages or operational logs.")
- **I-14** Results are accessed only through authentication in the existing portal. No attached results and no public result link. (PRD: "Do not attach results or introduce a public result link.")
- **I-15** No notification before a doctor has reviewed and released the result (see I-3).
- **I-16** No duplicate notifications for the same released result version. (PRD: "Avoid duplicate notifications for the same released result version")
- **I-17** Delivery failures are handled without sending the patient duplicate messages. (PRD: "handle delivery failures without duplicate patient messages.")
- **I-18** No recipient other than the patient is approved; adding one needs an explicit decision (OPEN-2).

## Open questions and owners

- **OPEN-1 Channel** (SMS, email or portal push). Owners: **product owner** and **privacy owner**, deciding after the consent and delivery review. Not decided here. (PRD: "Channel (SMS, email, portal push): product owner and privacy owner must decide after consent/delivery review."; notes, privacy owner: "Consent and the actual channel are still under review.")
- **OPEN-2 Referring-doctor notification.** Owners: **clinical owner** and **privacy owner**, deciding recipient authorization. Not approved by the PRD, and no approval is taken from the superseded request. (PRD: "clinical owner and privacy owner must decide recipient authorization. It is not approved by this PRD."; notes, clinical owner: "do not infer approval for referring-doctor notifications from the old request.")

## Decision record

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
