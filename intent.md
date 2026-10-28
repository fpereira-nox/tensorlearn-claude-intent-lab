# Intent: Results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (approved 2026-09-18), `inputs/02-current-meeting.md`
(discovery notes, 2026-09-20). `inputs/03-superseded-request.md` (2026-08-02) is superseded
by PRD N-17 and is used only to identify what is excluded; nothing in this intent is approved by it.

## Problem

- **I-1** The fictional clinic receives avoidable calls from patients checking whether their
  results are ready. PRD: "The fictional clinic receives avoidable calls from patients checking
  whether results are ready."

## Proposed outcome

- **I-2** Patients learn that a result is ready for their review in the existing authenticated
  portal, without having to call. PRD: "Notify the patient when a result is ready for their
  review in the existing authenticated portal."
- **I-3** A result counts as "ready" only once a doctor has reviewed **and** released it. A
  completed laboratory upload alone is not ready, and the result's content (for example an
  abnormal value) does not change when it is eligible. PRD: "A doctor must review and release
  the result before any notification is eligible." Product owner: "'Ready' must not mean that
  the laboratory upload merely finished."
- **I-4** The notification only says that an update is available and directs the patient to
  sign in to the existing portal. PRD: "The notification says that an update is available and
  directs the patient to the existing authenticated portal."

### How success will be observed

- **I-5** Results-status calls go down: status-only calls are counted separately from clinical
  questions and compared against a measured baseline. PRD: "Measure reduction in results-status
  calls and delivery failures." Support lead: "count status-only calls separately from clinical
  questions; compare against a measured baseline."
- **I-6** Delivery failures are measured and handled without duplicate patient messages.
- **I-7** No baseline or numeric target is approved yet; both are to be agreed during discovery.
  PRD: "The baseline and numerical target will be agreed during discovery; none is approved yet."
  This intent sets no target.

## Affected users and systems

- **Patients**: the only approved recipients of the notification.
- **Doctors**: their review and release of a result is the trigger for eligibility.
- **Support staff**: handle the calls being reduced and provide the status-only call counts.
- **Privacy, clinical, product and engineering owners**: own the open decisions and constraints.
- **Systems**: the existing authenticated portal and its existing result route, the result
  review/release workflow, the notification and delivery service, and operational/notification
  logs. Engineering lead: "the existing portal path is adequate."
- The referring doctor is **not** an approved recipient (see OPEN-2).

## Scope

In scope:
- **S-1** Use the existing portal result route. PRD: "Use the existing portal result route."
- **S-2** Prevent duplicate notifications for the same released result version.
- **S-3** Handle delivery failures without sending duplicate patient messages. PRD: "Avoid
  duplicate notifications for the same released result version; handle delivery failures
  without duplicate patient messages."

## Out of scope

- **X-1** Portal redesign, appointment scheduling, clinical interpretation and notifying third
  parties. PRD: "Portal redesign, appointment scheduling, clinical interpretation and notifying
  third parties are outside this approved scope."
- **X-2** A new or rebuilt patient dashboard. Engineering lead: "A new patient dashboard is a
  separate project."
- **X-3** Excluded items from the superseded request, which is not current approval: notifying
  as soon as the lab uploads a result, putting the test result in an SMS, copying the referring
  doctor, rebuilding the patient dashboard, and the "90% call reduction in one week" target.

## Constraints

- **C-1** Doctor review and release before any notification is eligible (PRD).
- **C-2** No diagnosis, test name or result, clinical measurement or treatment detail in the
  message **or** in notification and operational logs. PRD: "No diagnosis, test result, clinical
  measurement or treatment detail goes in the message or notification logs." Privacy owner: "no
  sensitive content in messages or operational logs."
- **C-3** Access is authenticated in the existing portal; no attached results and no public
  result link. PRD: "Authenticate access in the existing portal. Do not attach results or
  introduce a public result link."
- **C-4** At most one notification per released result version, including after delivery
  failures and retries (PRD).
- **C-5** Only the patient is notified until OPEN-2 is decided; no recipient is approved by
  default.

## Open questions and owners

- **OPEN-1 Channel (SMS, email, portal push).** Owners: product owner and privacy owner, after a
  consent/delivery review. PRD: "product owner and privacy owner must decide after
  consent/delivery review." Privacy owner: "Consent and the actual channel are still under
  review." Proposal for the owners to consider (not a decision): SMS first, falling back to
  email when no phone number is available.
- **OPEN-2 Notification to the referring doctor.** Owners: clinical owner and privacy owner, who
  must decide recipient authorization. PRD: "It is not approved by this PRD." Clinical owner: "do
  not infer approval for referring-doctor notifications from the old request." Until decided,
  the referring doctor is not notified.

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
