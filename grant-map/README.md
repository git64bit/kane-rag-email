# grant-map

## Purpose

This top-level tree exists for one purpose only:

> **Map the complete Kane Civic Infrastructure for human access, operation, reconstruction, and management.**

It is not an application-development area, a RAG design area, a security-policy notebook, or a general architecture discussion.


## Project priorities

The operated Civic Infrastructure is developed and presented according to four priorities, in decreasing order:

1. **IMPRESS**
2. **FUNCTION**
3. **INSPIRE**
4. **EXPAND**

These are not interchangeable.

**IMPRESS** comes first because the infrastructure must be visibly understandable and compelling to a human observer. A working browser trust relationship, a recognizable county service identity, a usable portal, and a clearly mapped management path have demonstration value that source code alone does not provide.

**FUNCTION** comes next: the impressive surface must perform useful civic work for an actual participant and operator.

**INSPIRE** means another county, association, or civic group should be able to see the pattern and understand how it could be reproduced.

**EXPAND** comes last. Expansion does not mean dissolving the Kane County boundary into a general-purpose network. The preferred expansion model is replication: another county can construct its own bounded infrastructure from the same pattern.

## Deliberate county boundary

The Civic Infrastructure is intentionally constrained to a **single county**. This is a design feature, not an unfinished scalability problem.

The county boundary is established by several independent mechanisms. No single mechanism is expected to prove every kind of eligibility, identity, standing, or authority.

The boundary is layered:

```text
COUNTY
  |
  +-- COUNTY CA
  |     county-specific technical trust domain
  |
  +-- SASE
  |     deliberate physical participation / delivery-point evidence
  |
  +-- CURRENT RESIDENT
  |     present relationship to an eligible local residence
  |
  +-- AFFECTED STATUS
  |     present relationship to the institution or issue
  |
  +-- DNS / MAIL WHITELIST
  |     county-contained communications by default
  |     explicit exceptions do not create participation
  |
  +-- SAME AND EQUAL
        issue-specific peer relationship inside one Association
```

### County CA

The County CA establishes the county-specific technical trust environment.

For Kane, the service hierarchy is:

```text
Civic Infrastructure Root CA
    -> Civic Infrastructure Kane County IL CA
    -> county service certificates
```

Browser trust in the Kane County service hierarchy does **not** by itself establish residency, homeowner status, affected status, participant standing, or governance authority. It establishes that the human browser deliberately recognizes services issued by this Civic Infrastructure.

Another county should normally reproduce the pattern with its own county-specific trust domain rather than being absorbed into Kane's operational trust domain.

### SASE

The Self-Addressed Stamped Envelope is an intentional participation gate tied to a physical delivery point.

Its purpose is not to prove a person's permanent legal identity. It provides practical evidence that someone can receive and return material through the relevant physical address and is willing to perform a small observable act to participate.

SASE therefore contributes evidence of local participation without turning the infrastructure into an identity-provider system.

### CURRENT RESIDENT

**CURRENT RESIDENT** is deliberately present-tense.

The infrastructure is concerned with the current relationship between a participant and an eligible residence, not with creating a permanent person record that follows an individual forever.

A former resident and a current resident are therefore not interchangeable merely because both once had a relationship to the same property.

### Affected Status

**Affected Status** asks whether a person is presently affected by the institution, rule, event, or issue under discussion.

It is separate from browser trust, email access, SASE evidence, and general participation.

A person may be able to communicate with the infrastructure without having Affected Status for a particular matter. Likewise, an explicit external communication exception does not manufacture Affected Status.

### DNS whitelist and explicit exceptions

Communications are deliberately county-contained by default.

The current mail design permits the normal Civic communications namespace under:

```text
*.diagnostics.kane-il.us
```

and rejects unapproved external origins by default.

Explicit exceptions may be created for legitimate outside correspondents, such as grant trustees or other deliberately authorized parties. Those exceptions are communication exceptions only.

```text
EXTERNAL EXCEPTION
    != CURRENT RESIDENT
    != AFFECTED STATUS
    != SAME AND EQUAL
    != CIVIC STANDING
```

An outside correspondent may communicate with the system without becoming a Kane participant.

### Same and Equal

**Same and Equal** is not a general statement that all participants are equivalent.

It is a narrow relationship test for discussion of an HOA issue.

Two people are **Same and Equal** only when they occupy the same current homeowner relationship inside the **same Association** for the matter being discussed.

Examples:

```text
current homeowner + current homeowner
same Association
    -> SAME AND EQUAL

current homeowner + current homeowner
different Associations
    -> NOT SAME AND EQUAL

current homeowner + former homeowner
same Association
    -> NOT SAME AND EQUAL

homeowner + Board member
same Association
    -> NOT SAME AND EQUAL
```

The Board-member example matters because the Board member occupies an institutional/governance role relative to the homeowner. Shared property ownership does not erase that asymmetry.

The county infrastructure may therefore contain many Associations without turning them into one undifferentiated county-wide peer forum.

```text
KANE COUNTY CIVIC INFRASTRUCTURE
    |
    +-- Association A
    |     current homeowner <-> current homeowner
    |              SAME AND EQUAL
    |
    +-- Association B
    |     current homeowner <-> current homeowner
    |              SAME AND EQUAL
    |
    +-- cross-Association discussion
          may be useful
          but is NOT Same and Equal
```

### Boundary principle

The county boundary is therefore not simply:

```text
users who happen to live in Kane County
```

It is a deliberately limited civic environment composed of:

```text
county-specific technical trust
+ physical participation evidence
+ current-resident relationship
+ issue-specific affected status
+ county-contained communications by default
+ explicit exceptions that do not confer participation
+ Same-and-Equal peer discussion only where the relationship actually matches
```

This boundary must remain visible in `grant-map` because the human operator needs to understand not only **where** infrastructure runs, but also **why the infrastructure is intentionally bounded the way it is**.

The map must allow a competent human operator who did not build the system to answer:

1. What logical Civic nodes exist?
2. Which public service names belong to each node?
3. Which proxy, relay, or ingress binds each public name to a backend?
4. Which physical host, VM, or LXD instance currently implements that backend?
5. How does a human reach and manage it from:
   - the home LAN,
   - the public Internet,
   - the relevant WireGuard/private fabric?
6. Which management UI, URL, port, and authentication boundary is required?
7. How are containers/VMs themselves managed?
8. What dependencies must exist for the service to remain reachable?
9. What can move without changing the logical Civic node or public service identity?
10. What is still unknown or unverified?

## Governing distinction

The map must keep these objects separate:

```text
CIVIC NODE
    logical public function

SERVICE IDENTITY
    DNS name + protocol + port + human purpose

ROUTING / PROXY
    how that service identity reaches its backend

BACKEND SERVICE
    Hubzilla, Usermin, Webmin, Dovecot, Postfix, LXD UI, etc.

COMPUTE LOCATION
    physical host / VM / LXD instance

NETWORK PATH
    public / LAN / WireGuard / private service fabric

HUMAN MANAGEMENT
    exact URL, port, UI, and access context
```

A DNS name is not a host.  
An IP address is not a management interface.  
A backend location is not a Civic node identity.

## Required access contexts

Every manageable object must be evaluated in all applicable contexts:

```text
HOME-LAB HUMAN
REMOTE/PUBLIC HUMAN
CIVIC INTER-HOST
PUBLIC PARTICIPANT
```

If an access context does not apply, say so explicitly.

## Required fields

Every host, VM, or LXD instance inventory must contain:

```text
NAME:
ROLE:
PHYSICAL/VIRTUAL PARENT:
PUBLIC ADDRESSES:
LAN ADDRESSES:
WIREGUARD ADDRESSES:
SERVICE NAMES:
HUMAN MANAGEMENT URLS:
MANAGEMENT PORTS:
PROXY/INGRESS PATH:
GUEST/CONTAINER MANAGEMENT:
DEPENDENCIES:
CURRENTLY HOSTED CIVIC SERVICES:
VERIFIED:
UNKNOWN:
```

Every public service inventory must contain:

```text
CIVIC NODE:
SERVICE NAME:
PROTOCOL:
PUBLIC PORT:
HUMAN PURPOSE:
PUBLIC EDGE/PROXY:
TLS TERMINATION:
BACKEND ADDRESS:
BACKEND PORT:
BACKEND HOST/INSTANCE:
HOME-LAB PATH:
REMOTE/PUBLIC PATH:
DEPENDENCIES:
VERIFIED:
UNKNOWN:
```

## Evidence rule

The mapping assistant must interrogate the live infrastructure. It must not infer missing addresses, ports, routing, proxy targets, or management surfaces.

A fact is marked **verified** only when it is obtained from the live host/configuration or directly demonstrated by a successful access path.

Unknowns stay visible as unknowns.

## Human-operability rule

A system is not considered mapped merely because another machine, script, or LLM can reach it.

For human-managed infrastructure, the map is incomplete until the human operator can identify:

```text
WHERE DO I POINT MY BROWSER OR TERMINAL?
WHICH ADDRESS?
WHICH PORT?
WHICH MANAGEMENT APPLICATION?
FROM WHICH NETWORK CONTEXT?
WHAT DOES THAT MANAGEMENT SURFACE CONTROL?
WHICH BROWSER PROFILE IS AUTHORIZED?
WHICH CA ROOTS OR CLIENT CERTIFICATES MUST THAT BROWSER HOLD?
```

## Browser trust and client authentication are infrastructure

Browser state is part of the access path.

For every browser-managed service, `grant-map` must record the actual browser or browser profile used by the human operator and the trust material required by that browser.

At minimum:

```text
DEVICE / WORKSTATION:
OPERATING SYSTEM:
BROWSER:
BROWSER PROFILE:
SERVICE:
ACCESS CONTEXT:
SERVER / CA TRUST REQUIRED:
CLIENT CERTIFICATE REQUIRED:
CERTIFICATE SUBJECT OR LABEL:
CERTIFICATE FINGERPRINT:
EXPIRY:
PRIVATE KEY LOCATION: never publish; record only where/how it is protected
VERIFIED:
```

Two different certificate roles must never be conflated:

```text
KANE CA TRUST
    browser trusts Kane-issued server certificates
    required for Civic services such as the Usermin portal

LXD CLIENT CERTIFICATE
    browser presents a client identity to LXD
    required for LXD mutual-TLS management access
```

Installing the Kane CA in one browser does not enroll another browser. Installing an LXD client certificate in one browser/profile does not authorize another browser/profile.

A service is not considered human-accessible until the required trust/client material is documented for the browser actually used.

See `grant-map/browser-access.md`.

## Initial mapping target

The first fully inventoried system is `annales`.

Known starting facts:

```text
HOST: annales
ROLE: Ubuntu LXD host; compute/inference plane

HOME-LAB LAN:
    10.0.0.36

WITNESS WIREGUARD:
    10.110.0.9

WEBMIN — HOME LAB:
    https://10.0.0.36:10000/

WEBMIN — REMOTE:
    https://198.58.111.109:10000/
      -> Webmin Servers Index on wg-pk
      -> annales at 10.110.0.9:10000

LXD HUMAN WEB MANAGEMENT:
    to be configured and verified
```

The next step is to make the LXD web interface usable from the home LAN, document its exact URL/authentication model, and then inventory every LXD instance hosted by annales.

## Repository working copy

A dedicated LXD instance on `annales` should hold the local working copy of `git64bit/kane-rag-email` used by the mapping assistant.

That instance is an **operator/mapping workbench**, not a production Civic service. It should be isolated from production workloads and given only the network reach and credentials required for infrastructure discovery and repository updates.

Its final name, addresses, management path, and permissions must themselves be recorded here after creation.
