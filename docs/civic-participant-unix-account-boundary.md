# Civic Participant UNIX Account Boundary

## Purpose

A Civic participant UNIX account is a **purpose-bounded computing environment for participation in the Civic Infrastructure**.

It is not a general-purpose personal computing account, cloud-storage account, hosting account, or ordinary consumer email account.

The account exists to provide a participant with a small, durable, standards-based UNIX workspace in which Civic Infrastructure tools, credentials, records, and recovery material can persist independently of any particular web application or device platform.

The central principle is:

> **The account is intentionally small because its purpose is intentionally narrow.**

The storage quota is therefore not an apology for limited capacity. It is part of the account definition.

## Participant account model

~~~text
prospect-initiated participation
        ↓
participation evidence
        ↓
real UNIX principal
        ↓
bounded home directory
        ↓
Usermin / SSH / Terminal
        ↓
Civic applications and Civic state
~~~

The UNIX account is the durable operational principal. Usermin is a browser-accessible interface to that account. SSH is another possible interface. A future terminal client, local console, or replacement web terminal may provide another interface without changing the participant account itself.

## Production account example

~~~text
Canonical participant ID: SASE25SEP26A
UNIX login:              sase25sep26a
UID inside container:    1002
Home:                    /home/sase25sep26a
Shell:                   /bin/bash
sudo:                    none
~~~

The account is an ordinary UNIX account rather than an application-specific identity.

## Storage entitlement

The initial ordinary Participant storage entitlement is:

~~~text
Block soft limit:   230400
Block hard limit:   256000

Approximate size:
    soft: 225 MiB
    hard: 250 MiB

File soft limit:    9000
File hard limit:    10000

Grace period:       7 days
~~~

The entitlement is deliberately modest. The account is expected to hold Civic Infrastructure state measured primarily in kilobytes and megabytes, not hundreds of gigabytes.

The quota communicates the intended scope of the account and prevents a Civic workspace from silently becoming general-purpose storage.

## Why both byte and file-count quotas matter

The account is bounded in two dimensions.

~~~text
byte quota
    limits total storage volume

inode/file quota
    limits filesystem object proliferation
~~~

A process may consume little disk space while creating very large numbers of tiny files. Caches, malformed scripts, temporary files, and poorly behaved applications can exhaust filesystem resources without consuming much storage.

The file-count quota therefore protects the purpose-bounded nature of the account just as much as the byte quota.

## What belongs in the participant UNIX account

The participant UNIX account may contain persistent Civic Infrastructure state such as:

~~~text
~/.ssh/
    SSH keys
    authorized_keys
    known_hosts
    SSH configuration

~/.gnupg/
    OpenPGP keys
    trust database
    configuration

~/certs/
    participant certificates
    public CA material
    certificate chains
    fingerprints

~/civic/
    Civic application configuration
    receipts
    manifests
    small evidence records
    proofs
    local metadata
    participant application state

~/firmware/
    approved Civic device firmware
    signatures
    checksums
    release manifests

~/recovery/
    encrypted recovery material
    key exports
    configuration backup
    account reconstruction material

~/bin/
    participant-accessible Civic tools
    scripts
    terminal applications
~~~

The exact directory structure may evolve. The governing principle is that stored material must support Civic Infrastructure participation.

## Operational Civic material

Some larger files may legitimately exist temporarily or periodically, including firmware images, signed release bundles, exported proof sets, diagnostics, small document packages, encrypted Civic backups, generated reports, and temporary working files created by Civic applications.

These remain within the account purpose when they directly support Civic Infrastructure work. The quota provides a practical signal when an application or workflow begins to exceed that purpose.

## What does not belong in the participant UNIX account

The account is not intended for personal photo collections, music or video libraries, unrelated personal documents, general workstation backups, browser download archives, private cloud-storage substitution, unrelated development trees, game data, personal hosting, general shell hosting, unrelated databases, ordinary personal email archives, or bulk file synchronization unrelated to Civic Infrastructure.

The test is simple:

> **Would this data exist here if the participant were not participating in the Civic Infrastructure?**

If the answer is no, it may belong. If the answer is yes, it normally does not.

## Civic backup boundary

Backup is a legitimate function of the account, but only for **Civic state**.

Appropriate backup material may include SSH configuration and keys, OpenPGP configuration and exports, certificates, Civic application configuration, receipts, proof records, small evidence metadata, firmware manifests, checksums, recovery instructions, and participant-controlled Civic records.

The distinction is:

~~~text
backup my Civic state
    allowed purpose

backup my laptop
    outside account purpose
~~~

## Cryptographic material

A Civic participant UNIX account is expected to hold cryptographic material.

Examples include:

~~~text
SSH
    access to permitted Civic systems

OpenPGP
    authenticated or encrypted correspondence

X.509
    certificates and trust material

Civic application keys
    where allowed by governing contracts

firmware verification material
    device and release verification
~~~

However, possession of operational key material must never be confused with Civic authority.

~~~text
SSH key
    != Civic standing

OpenPGP key
    != Civic standing

X.509 certificate
    != Civic standing

UNIX account
    != Civic standing

Usermin access
    != Civic standing

possession of a private key
    != automatic governance authority
~~~

Civic authority remains governed by the separate deterministic Civic authority model.

## Email boundary

Participant email is also purpose-bounded.

The intended model is:

~~~text
plain text accepted
HTML blocked
attachments stripped
~~~

The mailbox exists for Civic communication, not general-purpose personal correspondence or document storage.

### Plain text

Plain text is preferred because Civic communication should preserve meaning independently of rendering.

A useful Civic message should remain intelligible through:

~~~text
sender
recipient
date
headers
body
~~~

Plain text is easier to preserve, hash, inspect, quote, diff, index, archive, process deterministically, present to RAG systems, and reproduce in terminal applications.

Formatting should not carry essential Civic meaning.

### HTML blocking

HTML mail introduces mechanisms that are unnecessary for Civic communication, including remote images, tracking resources, CSS, embedded objects, rendering-dependent presentation, alternate MIME bodies, hidden markup, and presentation differences between clients.

The Civic mail path should favor a simple and inspectable textual artifact.

## Attachment boundary

Email attachments should not become participant mailbox storage.

The intended mail architecture separates communication from evidence or document intake.

~~~text
email body
    communication

attachment
    controlled evidence/document path
~~~

Where attachments are accepted at an ingress layer, the preferred future behavior is:

~~~text
incoming MIME message
        ↓
preserve exact original where required
        ↓
extract attachment
        ↓
hash attachment
        ↓
store through controlled evidence intake
        ↓
deliver plain-text message to participant
        ↓
include attachment receipt/reference
~~~

A participant-visible replacement may contain the object name, SHA-256 digest, evidence reference, size, and receipt timestamp.

The governing principle is:

> **Attachment stripping must not mean evidence destruction.**

It means that arbitrary binary content should not silently become mailbox storage.

## Mail is not evidence storage

The account architecture should maintain a clear separation:

~~~text
mail
    communication

UNIX home
    participant working state

evidence repository
    durable evidence objects

RAG corpus
    derived searchable representation

Civic authority state
    deterministic authority records
~~~

This prevents email from becoming the accidental archive for every document exchanged by participants. It also prevents repeated copies of large attachments from propagating through Inbox, Sent, forwarding, backup, indexing, and RAG pipelines.

## Usermin role

Usermin provides a browser interface to the UNIX account.

Useful participant-facing functions may include:

~~~text
Terminal
Mail
File Manager
Disk Quotas
Change Password
.plan
other narrowly selected UNIX functions
~~~

The primary path is:

~~~text
browser
    ↓
Usermin
    ↓
Terminal
    ↓
real UNIX account
    ↓
Civic applications
~~~

The terminal is therefore not a generic hosted shell product. It is a browser-accessible **Civic terminal** attached to a real UNIX account.

## Text-based Civic applications

The participant account is intended to support Civic applications built with ordinary UNIX and terminal technologies, including ncurses applications, text user interfaces, shell programs, local command-line tools, mail-oriented interfaces, RAG clients, evidence tools, participant enrollment tools, Civic ceremony clients, firmware management tools, and recovery tools.

A terminal application can operate through:

~~~text
Usermin Terminal
SSH
local console
future standards-based terminal interface
~~~

without being rewritten as a platform-specific mobile, Windows, Apple, Chrome, or proprietary web application.

## Platform independence

The participant's operational environment is a true UNIX-like account.

Windows, macOS, ChromeOS, Android, iOS, browsers, web frameworks, and desktop applications may act as clients. They do not define the participant's Civic computing environment.

The durable environment is the UNIX account itself.

## Resource entitlement rather than general hosting

The participant receives actual computing resources:

~~~text
UID/GID
home directory
persistent filesystem state
login shell
mail
terminal access
Civic applications
bounded storage
bounded file count
~~~

This is materially different from a conventional web account represented only by an application database row.

At the same time, the account is not an unrestricted shell-hosting service. The resources are granted for Civic Infrastructure participation and remain bounded accordingly.

## Quota as diagnostics

The quota also serves as a diagnostic signal.

~~~text
5 MiB
    keys, configuration, receipts
    normal

30 MiB
    firmware and signed release material
    plausible

180 MiB
    accumulated Civic material
    worth review

240 MiB
    near hard limit
    investigate account use and application behavior
~~~

A large increase may reveal accidental attachment retention, runaway logs, caches, application defects, improper personal storage, duplicate evidence, excessive firmware retention, or backup misconfiguration.

The quota is therefore both an entitlement and an observability mechanism.

## Participant-visible account rule

The account should eventually present a concise rule to every participant:

> **This UNIX account exists solely to support participation in the Civic Infrastructure.**
>
> It provides persistent storage for Civic keys, certificates, configuration, correspondence, firmware, evidence references, recovery material, and related Civic applications. It is not intended for general personal storage, hosting, personal email, or unrelated computing.

This should remain short enough to be understood as part of the Civic operating culture rather than as conventional long-form acceptable-use boilerplate.

## Administrative boundary

Ordinary participants receive a real UNIX account, normal login shell, private home directory, quota, Usermin access, Terminal, and Civic applications.

They do not receive by default:

~~~text
sudo
Webmin administrative authority
host administration
unrestricted system configuration
quota administration
Civic governance authority
Signing Node authority
~~~

Quota enforcement is performed at the host filesystem layer.

For the current LXD deployment:

~~~text
annales
    /dev/sde4
    mounted at /mnt/mailboxes
    ext4 user/group quotas
        ↓
LXD bind mount
        ↓
witness-hubzilla:/home
        ↓
participant home directories
~~~

The container sees the participant home directory while the LXD host enforces the filesystem quota.

## Current UID mapping example

For the first production participant:

~~~text
inside witness-hubzilla:
    uid 1002
    gid 1003

LXD host mapping base:
    1000000

on annales:
    uid 1001002
    gid 1001003

host path:
    /mnt/mailboxes/sase25sep26a
~~~

Quota administration therefore occurs against the shifted host UID while the participant continues to operate normally as UID 1002 inside the container.

This implementation detail must not leak into participant-facing identity semantics.

## Replaceability

The account model must survive replacement of individual interfaces and services.

~~~text
Usermin replaced
    UNIX account remains

Hubzilla replaced
    UNIX account remains

browser replaced
    UNIX account remains

mail client replaced
    UNIX account remains

LDAP introduced later
    participant semantics remain

terminal implementation replaced
    Civic applications remain portable
~~~

The participant's operational anchor is the UNIX principal and its bounded Civic state.

## Governing principle

The Civic participant UNIX account is intentionally modest.

It is not designed to compete with consumer cloud services.

It is designed to provide something more specific:

> **A small, durable, real UNIX workspace whose entire purpose is Civic participation.**

The quota, plain-text mail policy, attachment boundary, terminal-first application model, cryptographic workspace, and backup rules all follow from that purpose.
