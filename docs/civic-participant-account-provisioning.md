# Civic Participant Account Provisioning

## Purpose

This document defines the **manual production procedure** for creating a new Civic Infrastructure participant account.

It is intentionally documentation-first.

The provisioning process is not implemented as a script because the participant model is still evolving and each provisioning action is significant enough to be observed, understood, and verified independently.

The objective is not merely to create a UNIX user.

The objective is to create a **bounded Civic participant environment** whose operational state, storage entitlement, participation provenance, and access boundaries are all deliberate.

---

## Governing principle

A valid participant account is not created by a single command.

It is the result of several coordinated actions performed on different systems:

```text
participation evidence
        ↓
canonical participant identifier
        ↓
UNIX principal
        ↓
home directory
        ↓
quota entitlement
        ↓
private participant record
        ↓
Usermin access
        ↓
Civic-only operating boundary
        ↓
verification
```

If any required step is omitted, provisioning is incomplete.

---

# 1. Participation must precede account creation

## What

A participant account is created only after the prospective participant has completed the applicable participation-initiation process.

The current first production mechanism is:

```text
SASE
Self-Addressed Stamped Envelope
```

The SASE is one form of evidence that the prospective participant initiated the process and expended effort.

Additional participation gates may be added later.

## Where

Participation evidence is retained outside the UNIX account itself.

The UNIX account must not become the sole record of why participation was created.

## Why

The system must distinguish:

```text
automatically created account
        !=
voluntary participation
```

The participant account follows participation evidence. It does not create that evidence.

---

# 2. Assign the canonical participant identifier

## What

A canonical participant identifier is assigned from the participation event.

Current format:

```text
SASE25SEP26A
```

Meaning:

```text
SASE
    participation originated through the SASE process

25SEP26
    registered 25 September 2026

A
    serial sequence for that registration date
```

## Where

The canonical identifier is used in:

- the private participant record;
- the UNIX GECOS/comment field;
- Civic records that need a stable participant reference;
- future evidence and participation references.

## Why

The identifier records **how participation entered the system**, not who the person is.

It is deliberately institutional and minimally identifying.

## How

Preserve the canonical form exactly:

```text
SASE25SEP26A
```

The normalized UNIX login is lowercase:

```text
sase25sep26a
```

---

# 3. Create the UNIX principal

## What

Create a real UNIX account with:

- a unique UID;
- its own primary group;
- a private home directory;
- a normal login shell;
- no sudo privileges;
- the canonical participant identifier in GECOS.

## Where

Host:

```text
witness-hubzilla
```

The UNIX account exists inside the LXD container.

## Why

The UNIX principal is the durable operational participant environment.

Usermin is only one interface to it.

The account must remain useful even if Usermin, Hubzilla, a browser, or another application is later replaced.

## How

For the first production participant:

```bash
useradd \
  --create-home \
  --shell /bin/bash \
  --comment "SASE25SEP26A" \
  sase25sep26a

passwd sase25sep26a
```

Verify:

```bash
getent passwd sase25sep26a
id sase25sep26a
passwd -S sase25sep26a
ls -ld /home/sase25sep26a
sudo -l -U sase25sep26a 2>&1
```

Expected properties:

```text
normal UNIX account
active password
private home directory
/bin/bash
no sudo
```

---

# 4. Confirm home-directory ownership

## What

Verify that the participant home exists and is owned by the participant.

## Where

Inside:

```text
witness-hubzilla:/home/<unix-login>
```

Current first production example:

```text
/home/sase25sep26a
```

## Why

The participant must own the persistent UNIX workspace that supports Civic activity.

The home directory is not merely a temporary application directory.

## How

Example:

```bash
ls -ldn /home/sase25sep26a
```

The directory should be owned by the participant UID/GID and should not be world-readable.

The first production account currently uses:

```text
drwxr-x---
```

---

# 5. Determine the LXD host UID/GID mapping

## What

Determine the host-side shifted UID and GID corresponding to the participant inside the container.

## Where

Host:

```text
annales
```

Container:

```text
witness-hubzilla
```

Current LXD mapping base:

```text
1000000
```

For the first production participant:

```text
inside container:
    uid 1002
    gid 1003

on annales:
    uid 1001002
    gid 1001003
```

## Why

The participant home filesystem is mounted from the LXD host.

Quota enforcement occurs against host-visible numeric ownership, not the container's unshifted UID.

## How

On `annales`, confirm ownership:

```bash
find /mnt/mailboxes -maxdepth 1 -mindepth 1 -printf '%u:%g %p\n'
```

For the first production participant:

```text
1001002:1001003 /mnt/mailboxes/sase25sep26a
```

Never assume the shifted UID without verifying the current LXD idmap.

---

# 6. Assign the participant quota

## What

Every ordinary participant receives a bounded storage entitlement.

Current production Participant quota:

```text
Block soft limit:   230400
Block hard limit:   256000

Approximate size:
    soft: 225 MiB
    hard: 250 MiB

File soft limit:    9000
File hard limit:    10000

Grace period:       7 days
```

## Where

Quota administration occurs on:

```text
annales
```

Filesystem:

```text
/dev/sde4
```

Mounted at:

```text
/mnt/mailboxes
```

Presented inside the container as:

```text
witness-hubzilla:/home
```

Filesystem:

```text
ext4
usrquota enabled
grpquota enabled
```

## Why

The Civic UNIX account is a purpose-bounded workspace.

It exists for:

- Civic keys;
- certificates;
- Civic configuration;
- plain-text correspondence;
- firmware;
- evidence references;
- recovery material;
- Civic application state;
- bounded backups of Civic state.

It is not intended for:

- general personal storage;
- personal media;
- bulk backup;
- general hosting;
- unrelated computing.

The quota is therefore part of the account contract.

A participant account without its quota is incompletely provisioned.

## How

For `SASE25SEP26A`, the host-side UID is:

```text
1001002
```

Apply the ordinary Participant quota on `annales`:

```bash
setquota -u 1001002 \
  230400 256000 \
  9000 10000 \
  /mnt/mailboxes
```

Verify:

```bash
repquota -n /mnt/mailboxes | grep -E '^#1001002'
```

Quota enforcement must already be active:

```bash
quotaon -p /mnt/mailboxes
```

Expected:

```text
user quota on
group quota on
```

Do not attempt to administer `/dev/sde4` from inside `witness-hubzilla`.

The LXD guest sees the mounted filesystem but does not own the host block device.

---

# 7. Add the participant to the Civic Participant group

## What

Every ordinary participant UNIX account must be added to the supplemental UNIX group:

```text
civic-participants
```

For the first production participant:

```text
sase25sep26a
    primary group: sase25sep26a
    supplemental group: civic-participants
```

## Where

Host:

```text
witness-hubzilla
```

## Why

The `civic-participants` group is the operational class used to apply the ordinary Participant environment consistently.

It provides a stable group target for Usermin Module Restrictions and future participant-scoped configuration without turning each participant into a Webmin administrative user.

Membership in this UNIX group does not itself create Civic standing, governance authority, operator authority, or Signing Node authority.

## How

After the UNIX account exists and its quota has been assigned, add the account to the group:

```bash
usermod -aG civic-participants <unix-login>
```

For the first production participant:

```bash
usermod -aG civic-participants sase25sep26a
```

Verify:

```bash
getent group civic-participants
id sase25sep26a
```

Expected relationship:

```text
sase25sep26a
    primary:      sase25sep26a
    supplemental: civic-participants
```

The `-aG` form is important: the participant is appended to the supplemental group without replacing other supplemental memberships.

A participant account that is intended to receive the ordinary Participant Usermin policy but is not a member of `civic-participants` is incompletely provisioned.

---

# 20. Create the private participant record

## What

Create a private record containing information that does not belong in `/etc/passwd`.

The record may contain:

- canonical participant ID;
- UNIX login;
- real name;
- real contact information;
- role;
- registration date;
- participation provenance.

## Where

Proposed production location:

```text
/var/lib/kane/participants/
```

Example:

```text
/var/lib/kane/participants/SASE25SEP26A.json
```

## Why

The UNIX account database must remain mechanically minimal.

Real identity and contact information require their own access controls, provenance, retention, and future migration rules.

## How

The initial record model is documented separately in:

```text
docs/participant-record.md
```

The participant record must not be committed to the public repository.

Private participant data must remain operational state.

---

# 8. Preserve the role boundary

## What

The common role is:

```text
Participant
```

## Where

The role belongs in the private participant record and future role-aware Civic applications.

It does not need to be encoded as sudo access or a Webmin administrative account.

## Why

The ordinary participant role must remain separate from:

- system administration;
- Webmin administration;
- operator authority;
- Signing Node authority;
- governance standing.

## How

Verify:

```bash
sudo -l -U <unix-login>
```

Ordinary participants should not receive sudo.

Do not automatically synchronize ordinary UNIX participant creation into Webmin administrative-user creation.

---

# 9. Usermin access

## What

The participant uses the real UNIX account through Usermin.

## Where

Current portal:

```text
https://portal.diagnostics.kane-il.us:20000/
```

Authentication path:

```text
UNIX account
    ↓
PAM
    ↓
Usermin
```

## Why

Usermin is a browser-accessible interface to a real UNIX environment rather than a separate participant identity system.

The account remains valid independently of Usermin.

## How

After account creation and password assignment, verify login through Usermin.

Important participant-facing functions include:

- Terminal;
- Mail;
- File Manager;
- Disk Quotas;
- Change Password;
- `.plan`;
- future Civic terminal applications.

---

# 10. Terminal availability

## What

Every ordinary participant receives access to a real terminal session.

## Where

Initially through Usermin Terminal.

Potential future access may also include SSH or other standards-based terminal interfaces.

## Why

The Civic Infrastructure application model is terminal-first.

Applications may be implemented as:

- ncurses applications;
- TUIs;
- shell tools;
- local command-line tools;
- RAG clients;
- evidence tools;
- ceremony clients;
- firmware tools;
- recovery tools.

This keeps Civic applications independent of Windows, Apple, ChromeOS, mobile platforms, or a particular web frontend.

## How

Verify that the participant can open a terminal and that the shell identity is the expected UNIX account:

```bash
whoami
id
pwd
```

Expected:

```text
participant UNIX login
participant UID/GID
/home/<participant>
```

---

# 11. `.plan` availability

## What

Usermin exposes the traditional UNIX `.plan` file.

## Where

Participant home:

```text
/home/<unix-login>/.plan
```

## Why

The file may serve as participant-controlled plain-text descriptive material attached directly to the UNIX account.

It is not an identity database.

It is not a participant authority record.

It should not contain private contact information by default.

## How

The participant may edit `.plan` through Usermin.

The existence of `.plan` does not imply that the historical Finger service is enabled.

---

# 12. Civic-only storage boundary

## What

The participant home is reserved for Civic Infrastructure purposes.

## Where

```text
/home/<unix-login>
```

## Why

The low quota is intentional.

The account is for:

- SSH keys;
- OpenPGP keys;
- certificates;
- Civic configuration;
- firmware;
- Civic application data;
- evidence references;
- Civic recovery data;
- Civic-state backups.

It is not a general-purpose home directory for unrelated personal computing.

## How

The governing boundary is documented in:

```text
docs/civic-participant-unix-account-boundary.md
```

---

# 13. Mail boundary

## What

Participant mail is purpose-bounded Civic communication.

Current intended policy:

```text
plain text accepted
HTML blocked
attachments stripped
```

## Where

The participant's Civic mail environment.

## Why

Mail must not become accidental document storage or uncontrolled binary evidence storage.

Plain text is easier to preserve, inspect, hash, index, quote, diff, and process deterministically.

## How

Attachment handling must eventually preserve evidence through a controlled evidence-intake path rather than silently destroying it.

The governing rule is:

> **Attachment stripping must not mean evidence destruction.**

Mail, evidence storage, RAG corpus, UNIX state, and Civic authority state remain separate systems.

---

# 14. Civic Trust Enrollment

## What

Browser trust enrollment is another participation-related act.

The participant may install the Civic Infrastructure Root CA so that the browser recognizes locally issued Civic services.

## Where

Participant-controlled browser/device trust store.

## Why

This is another explicit, effort-bearing action associated with entry into the Civic environment.

It is not Civic standing.

## How

See:

```text
docs/civic-trust-enrollment.md
```

The browser trust action remains distinct from SASE, UNIX account creation, and Civic authority.

---

# 15. Verify the completed participant environment

A participant account is not considered fully provisioned until each required property has been checked.

## On witness-hubzilla

Verify:

```bash
getent passwd <unix-login>
id <unix-login>
passwd -S <unix-login>
ls -ld /home/<unix-login>
sudo -l -U <unix-login> 2>&1
```

Confirm:

```text
correct canonical identifier
correct UNIX login
normal shell
active password
private home
no sudo
```

## On annales

Verify host ownership:

```bash
find /mnt/mailboxes -maxdepth 1 -mindepth 1 -printf '%u:%g %p\n'
```

Verify quota:

```bash
repquota -n /mnt/mailboxes
```

Confirm:

```text
correct shifted UID
correct soft block limit
correct hard block limit
correct soft file limit
correct hard file limit
```

## Through Usermin

Verify:

```text
login succeeds
Terminal opens
shell identity is correct
home directory is correct
participant sees intended Usermin functions
participant does not receive administrative Webmin authority
```

---

# 16. Provisioning failure conditions

The following are provisioning failures:

- account created before required participation evidence exists;
- canonical participant identifier missing or malformed;
- personal name used as the UNIX login where the canonical identifier should be used;
- participant given sudo unintentionally;
- participant automatically created as a Webmin administrator;
- home directory missing;
- home directory ownership incorrect;
- home directory publicly readable beyond the intended policy;
- quota not assigned;
- quota assigned to the wrong shifted UID;
- quota verification omitted;
- participant not added to the `civic-participants` supplemental group;
- private participant record written to public source control;
- Usermin login not verified;
- participant terminal unavailable where Terminal is part of the service;
- general-purpose storage or hosting implicitly enabled contrary to the Civic account boundary.

A participant UNIX account with no quota must be treated as **incomplete provisioning**, not as a valid ordinary Participant account.

---

# 17. Manual-first operational policy

This procedure is deliberately documented as a sequence of visible administrative actions rather than hidden inside a provisioning script.

Reasons include:

- the participant model is still being refined;
- participation provenance matters;
- UID mapping must be observed;
- quota assignment must be verified;
- different systems own different parts of the process;
- errors should remain legible to the operator;
- documentation should survive future platform changes;
- a later implementation may differ while preserving the same required outcomes.

The durable requirement is not a particular shell script.

The durable requirement is:

> **Every participant must emerge from provisioning with the same defined Civic properties and boundaries.**

---

# 18. Current production topology

```text
annales
Ubuntu LXD host
    |
    | /dev/sde4
    | ext4 + usrquota + grpquota
    | mounted at /mnt/mailboxes
    |
    +-- witness-hubzilla
          /home bind-mounted from /mnt/mailboxes
          PAM
          Usermin
          Terminal
          participant UNIX accounts
```

For the first production participant:

```text
Canonical ID:
    SASE25SEP26A

UNIX login:
    sase25sep26a

inside witness-hubzilla:
    uid 1002
    gid 1003

on annales:
    uid 1001002
    gid 1001003

host home:
    /mnt/mailboxes/sase25sep26a

container home:
    /home/sase25sep26a
```

---

# 19. Final provisioning principle

Creating a Civic participant is not equivalent to creating a login.

The complete operation establishes:

```text
participation provenance
+
canonical participant reference
+
real UNIX principal
+
private persistent home
+
bounded storage entitlement
+
`civic-participants` operational group membership
+
private participant metadata
+
Usermin/PAM access
+
terminal environment
+
Civic-only usage boundary
+
verified non-administrative role
```

Only when those properties are present should the participant account be treated as fully provisioned.
