# Data Handling

## What is stored

| Data | Storage | Retention |
|---|---|---|
| Discharge document (original) | Object storage, encrypted at rest | Until patient deletes |
| Plain-language conversion | PostgreSQL | Until patient deletes |
| Scheduled tasks and reminders | PostgreSQL | 90 days after completion |
| Symptom check-in responses | PostgreSQL | Until patient deletes |
| Consent grants and revocations | Append-only audit table | Never deleted — the log is the record |
| Caregiver identity | PostgreSQL | Until the invitation is revoked |

## What is not stored

- No real medical record identifiers. The FHIR integration is sandbox-only.
- No message bodies from the Twilio sandbox beyond delivery status.
- No location data.
- No analytics or third-party telemetry of any kind.

## Deletion

Patient-initiated deletion removes the document, its conversion, all check-in responses, and
all scheduled tasks. The consent audit log is retained with the patient identifier replaced by
a tombstone, because an audit log that can be erased is not an audit log — but the retained
rows carry no health content.

## Boundaries this prototype does not meet

This is not a HIPAA-compliant system. There is no BAA, no formal access-control review, no
encryption key management story beyond environment variables, and no breach-notification
process. Those are the difference between a hackathon prototype and a healthcare product, and
this repository is the former.
