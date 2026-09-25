# Participant Utility Candidates

## Purpose

This document is a working inventory of participant-facing utilities that may be provided inside the Civic participant UNIX environment.

These are **candidates**, not frozen requirements.

The purpose of the inventory is to capture useful participant functions as they are discovered during production deployment, without prematurely turning them into a single large application or automated provisioning system.

The utilities should remain small, legible, replaceable, and closely tied to real participant needs.

---

## Design principle

A participant utility should translate underlying machine state into participant-relevant Civic meaning.

For example:

```text
df
    reports filesystem capacity

civic-quota
    reports the participant's actual Civic storage entitlement
```

The participant should not need to understand:

- LXD UID shifting;
- host filesystem device names;
- quota database internals;
- service implementation details;
- administrative commands.

At the same time, the utility should not invent or conceal rules.

It should present the real participant-facing state derived from authoritative system data.

---

## General boundaries

Participant utilities should normally be:

- usable from a real UNIX shell;
- usable through Usermin Terminal;
- text-first;
- suitable for ncurses or another terminal UI where useful;
- small enough to understand;
- read-only unless an explicit participant action is required;
- independent of a particular desktop or mobile platform;
- replaceable without changing Civic authority.

A utility may explain or expose Civic state.

A utility must not create authority merely by presenting it.

```text
utility output
    != Civic standing

utility action
    != governance authority

UNIX access
    != Signing Node authority
```

---

# 1. civic-quota

## Purpose

Show the participant's actual Civic storage entitlement and current use.

This is the first identified production utility.

## Why it is needed

The UNIX `df` command reports the capacity of the underlying filesystem.

For the current participant environment:

```text
/home
    underlying filesystem: ~141 GB
```

But an ordinary participant may actually have:

```text
soft storage limit: 225 MiB
hard storage limit: 250 MiB
soft file limit:    9,000
hard file limit:    10,000
grace period:       7 days
```

Therefore `df` answers the wrong participant question.

The participant needs to know:

- how much Civic storage is allocated;
- how much is used;
- how much remains;
- how many files are in use;
- the soft and hard limits;
- whether a grace period is active;
- whether the account remains within entitlement.

## Candidate output

```text
Civic Infrastructure — Participant Storage

Participant:   SASE25SEP26A

Storage
  Used:         52 KiB
  Soft limit:  225 MiB
  Hard limit:  250 MiB
  Available:   ~250 MiB

Files
  Used:         15
  Soft limit:   9,000
  Hard limit:   10,000

Grace period:   7 days
Status:         WITHIN ENTITLEMENT

Purpose:
  Civic Infrastructure material only
```

## Boundary

The utility is read-only.

It reports quota state but cannot change the quota.

The participant should not need to see host-side shifted UIDs such as `1001002` or host device names such as `/dev/sde4`.

---

# 2. civic-identity

## Purpose

Show the participant the identifiers and operational account information that apply to the current session.

Possible information:

```text
Canonical participant ID
UNIX login
common role
home directory
shell
account registration date
participation provenance summary
```

## Why it may be useful

The participant should be able to answer:

> Who does the Civic Infrastructure currently recognize me as?

without inspecting `/etc/passwd`, private administrative records, or application databases.

## Boundary

The utility should distinguish:

```text
operational participant identity
    != Civic authority
```

It should not expose private participant metadata that is not intended for the current user-facing context.

---

# 3. civic-keys

## Purpose

Inventory participant-controlled Civic cryptographic material.

Possible categories:

```text
SSH
OpenPGP
X.509
Civic application keys
firmware verification keys
trusted CA material
```

## Why it may be useful

A participant UNIX account is expected to become a durable Civic cryptographic workspace.

The participant should be able to understand what key material exists without manually searching several hidden directories.

## Boundary

The utility should never display private-key material by default.

It may show:

- key type;
- fingerprint;
- creation date;
- expiration date;
- public identifier;
- location;
- status.

Possession of a key must never be presented as proof of Civic standing.

---

# 4. civic-mail

## Purpose

Present the participant-facing state of Civic mail.

Possible information:

- Civic mail address;
- mailbox status;
- plain-text-only policy;
- attachment handling policy;
- recent message counts;
- mail transport status relevant to the participant.

## Why it may be useful

The Civic mail environment is deliberately different from general consumer email.

The participant should be able to see the governing mail boundary directly.

## Boundary

The utility should reinforce:

```text
mail
    communication

evidence repository
    durable evidence objects
```

HTML is blocked and attachments are intended to be removed from ordinary mailbox delivery rather than treated as personal mailbox storage.

---

# 5. civic-evidence

## Purpose

Provide a participant-facing view of evidence objects and evidence references associated with the participant.

Possible functions:

- list evidence references;
- show hashes;
- show receipt dates;
- show CIDs where applicable;
- verify a known object against a recorded digest;
- display provenance.

## Why it may be useful

The participant should not need to remember storage implementation details to determine whether a Civic artifact was preserved or referenced.

## Boundary

The utility may report evidence state.

It must not silently modify or destroy preserved evidence.

---

# 6. civic-recovery

## Purpose

Help a participant understand and preserve the minimum material needed to reconstruct the Civic workspace.

Possible areas:

- SSH configuration;
- OpenPGP exports;
- certificates;
- Civic application configuration;
- recovery instructions;
- receipt indexes;
- firmware manifests;
- participant-controlled Civic records.

## Why it may be useful

The account explicitly permits backup of **Civic state**, but not general workstation backup.

A dedicated utility can make that boundary practical.

## Boundary

```text
backup Civic state
    in scope

backup personal computer
    out of scope
```

---

# 7. civic-firmware

## Purpose

Manage participant-visible Civic firmware artifacts.

Possible functions:

- list approved firmware;
- show version;
- show checksum;
- verify signature;
- identify target device;
- show release manifest;
- retain bounded recovery copies.

## Why it may be useful

Firmware is explicitly within the Civic participant UNIX account purpose where it supports Civic Infrastructure devices.

## Boundary

The utility should distinguish approved or verified firmware from arbitrary binary storage.

---

# 8. civic-trust

## Purpose

Show the participant the current Civic certificate trust material relevant to the account.

Possible information:

- Civic Infrastructure Root CA fingerprint;
- Kane County intermediate CA;
- participant-visible service certificates;
- expiration dates;
- browser trust-enrollment guidance.

## Why it may be useful

Civic Trust Enrollment is one participation-related act and should remain understandable after initial enrollment.

## Boundary

```text
browser trust
    != Civic identity
    != Civic standing
    != governance authority
```

---

# 9. civic-plan

## Purpose

Provide a simple participant-facing interface to the traditional UNIX `.plan` file.

## Why it may be useful

Usermin already exposes `.plan`, and it may serve as a participant-controlled plain-text description tied directly to the UNIX account.

A Civic wrapper may eventually clarify its intended use.

## Boundary

The `.plan` file is not:

- the private participant record;
- the real-name/contact database;
- a Civic authority record.

Private contact information should not be placed there by default.

---

# 10. civic

## Purpose

A future top-level terminal interface may gather mature participant utilities into one coherent ncurses or text-based application.

Possible menu:

```text
Civic Infrastructure

1. My participation
2. Storage
3. Mail
4. Keys and certificates
5. Evidence
6. Firmware
7. Recovery
8. Trust
9. Account information

q. Exit
```

## Why it may be useful

Individual utilities should be allowed to mature independently first.

Once their responsibilities are stable, a common `civic` interface can provide navigation without turning all functions into one inseparable program.

---

## Candidate naming convention

Participant utilities should use a simple namespace:

```text
civic-<function>
```

Examples:

```text
civic-quota
civic-identity
civic-keys
civic-mail
civic-evidence
civic-recovery
civic-firmware
civic-trust
civic-plan
```

A future umbrella command may simply be:

```text
civic
```

---

## Development order

Candidate order should follow real operational need rather than a feature roadmap.

The first candidate is `civic-quota` because a real production participant already has a real quota that ordinary UNIX tools do not present correctly from the participant's point of view.

Additional utilities should be added when an actual participant need appears.

The governing rule is:

> **Build participant utilities from observed Civic needs, not from a desire to fill a menu.**
