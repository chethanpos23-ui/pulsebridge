# Consent Model

The design goal: a patient should be able to accept help without surrendering their whole
record.

## Scopes

Sharing is granted per category, not per caregiver-as-a-whole.

| Scope | What the caregiver sees |
|---|---|
| `medications` | Adherence status — taken / missed / skipped. Never dosage notes. |
| `appointments` | Date, time, and location of upcoming appointments. |
| `checkins.summary` | A daily "doing okay / not okay" roll-up. |
| `checkins.detail` | Individual symptom responses. |
| `escalations` | Notification when an escalation fires. |

A caregiver invited with `medications` and `appointments` cannot see symptom detail, and the
API enforces this at the queryset level rather than in the UI.

## Properties

1. **Default deny.** A new caregiver starts with no scopes. Every scope is an explicit grant.
2. **Revocable at any time, immediately.** Revocation invalidates cached tokens rather than
   waiting for expiry.
3. **Visible.** The patient sees a plain-language list: "Maria can see whether you took your
   medication and when your appointments are."
4. **Audited.** Every grant, revocation, and access is written to an append-only log the
   patient can read.
5. **No transitive sharing.** A caregiver cannot invite another caregiver.

## The escalation exception

Escalation is the one path that sends information the patient did not individually approve at
send time — by design, because a patient in trouble may not be in a position to approve it.
It is bounded:

- The escalation contact is chosen by the patient during setup and can be changed or removed.
- Escalation sends a fixed template: that a check-in was concerning and when. It does not
  forward symptom detail.
- Every escalation is logged and shown to the patient.

## Open questions

The prototype does not handle a patient who loses decision-making capacity, which is precisely
the case where post-discharge coordination matters most. Solving it properly requires legal
grounding this project does not have.
