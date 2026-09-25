# Participant Record

A Civic participant has a minimal UNIX principal and a separate participant record.

For the first production participant:

```text
Canonical participant ID: SASE25SEP26A
UNIX login: sase25sep26a
Role: Participant
```

The UNIX account name records the participation-entry event, not the person's name.

## Separation

```text
/etc/passwd
  login, UID/GID, canonical participant identifier, home, shell

participant record
  real name, contact, role, participation provenance

.plan
  participant-controlled descriptive text

Civic authority records
  standing and governance state
```

Real name and contact information stay out of `/etc/passwd` and out of `.plan` by default.

## Production record

Operational participant records should be stored privately on the host, not in this public repository. Proposed location:

```text
/var/lib/kane/participants/SASE25SEP26A.json
```

Initial record shape:

```json
{
  "schema": "kane-participant-record/v1",
  "participant_id": "SASE25SEP26A",
  "unix_login": "sase25sep26a",
  "role": "Participant",
  "registered_date": "2026-09-25",
  "identity": {
    "real_name": "<participant-provided>"
  },
  "contact": {
    "primary": "<participant-provided>"
  },
  "participation_provenance": {
    "method": "SASE",
    "initiated_by": "prospect",
    "evidence": "returned self-addressed stamped envelope",
    "registered_date": "2026-09-25",
    "sequence": "A"
  }
}
```

No field should be populated by inference when the participant can supply the value directly.

## Evidence of voluntary participation

The participant record is one record in a cumulative participation process. SASE is one effort-bearing act. Civic Trust Enrollment is another. Additional gates may be added later.

```text
prospect initiates
  -> effort-bearing acts
  -> evidence retained
  -> participant record
  -> UNIX principal
  -> Civic services
```

Automatic or effortless account creation is not the intended participation model.

## .plan

Usermin exposes the traditional UNIX `.plan` file at `/home/<unix-login>/.plan`. It may be used as participant-controlled descriptive text. It is not an identity database and should not contain private contact information by default.

The existence of `.plan` does not imply that the historical Finger network service is enabled.

## Boundary

`role=Participant` does not itself grant administrative privilege, Signing Node authority, or governance standing. Those remain separate Civic authority questions.
