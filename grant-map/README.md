
# grant-map

## Purpose

This top-level tree exists to make the Kane Civic Infrastructure understandable, operable, demonstrable, and reproducible by humans.

Its immediate job is to map the live infrastructure for human access, operation, reconstruction, recovery, and management. Its grant-facing job is equally important: show how a deliberately bounded Civic Infrastructure can become not merely functional, but highly desirable to many different groups that have useful spare capacity to contribute.

This is not an application-development scratchpad, a generic RAG design area, a security-policy notebook, or a collection of disconnected host notes.

The map must explain both how the system works and why the system is deliberately designed this way.

A competent human operator who did not build the system should be able to reconstruct its topology, reach its management surfaces, understand its trust boundaries, and see how the same pattern could support another bounded Civic project.

---

## Project priorities

The Civic Infrastructure is developed and presented according to four priorities, in decreasing order:

1. **IMPRESS**
2. **FUNCTION**
3. **INSPIRE**
4. **EXPAND**

These priorities are deliberate.

### 1. IMPRESS

The infrastructure must first be visibly coherent, understandable, and compelling to a human observer.

A reviewer should be able to see a real trust hierarchy, a working portal, recognizable service identities, deliberate participation gates, understandable human-management paths, and a live system whose architecture can be explained without hand-waving.

Source code alone does not accomplish this.

### 2. FUNCTION

The impressive surface must perform useful work.

Participants and operators must be able to use the system for real communications, evidence, diagnostics, participation, recovery, and administration. Demonstration value without practical utility is not enough.

### 3. INSPIRE

The design should make another group say: We could use this pattern for our own bounded community.

That group does not need to be another condominium association or another county. The architecture should be legible enough that a maker community, an industry group, a statewide sector, or a qualification-based community can understand how to reproduce the pattern under its own rules.

### 4. EXPAND

Expansion comes last.

Expansion does not mean turning Kane into an unlimited general-purpose social network or maximizing account creation. Expansion means that the design becomes easy to reproduce for other deliberately bounded projects.

The preferred growth model is replication of bounded projects, not unbounded accumulation of users.

---

## What the Grant is intended to accomplish

The Grant is intended to make this design not only operational, but **desirable enough that many different groups will want to adopt, reproduce, contribute to, or extend it**.

The objective is not to attract the largest possible number of people because access is free, instant, or frictionless.

The project is specifically not optimized around the conventional Internet growth model of free accounts, immediate access, passive consumption, and maximum user count.

Instead, it is intended to attract people and groups that possess **spare capacity** and are willing to contribute some of it.

Spare capacity can include:

- time,
- technical skill,
- professional knowledge,
- local knowledge,
- compute,
- storage,
- bandwidth,
- equipment,
- workshop capability,
- physical space,
- organizational reach,
- administrative effort,
- postal and logistical effort,
- attention,
- and patience.

The useful participant is therefore not merely someone looking for another free service. The useful participant has some capacity that is presently underused and can be redirected toward a bounded civic purpose.

The Grant should improve the parts of the system that make this participation attractive:

- better human-facing interfaces,
- clearer onboarding,
- stronger demonstrations,
- easier replication,
- better documentation,
- easier recovery,
- understandable trust,
- usable participant services,
- visible evidence of contribution,
- and a coherent path from interest to meaningful participation.

The intended result is a system that people with useful spare capacity want to join because participation is consequential, not because the service is costless.

---

## SASE is the top-level participation qualifier

The Self-Addressed Stamped Envelope, or **SASE**, is the first human participation gate.

Participation does not begin with a browser account, email address, certificate, password, database row, or administrator invitation.

It begins with a deliberate human act.

The sequence is:

1. voluntary human act,
2. SASE,
3. personal effort,
4. small personal monetary cost,
5. physical assembly,
6. physical mailing,
7. unavoidable delay,
8. successful return path,
9. participant eligibility process,
10. project qualification,
11. current relationship,
12. affected status,
13. Same and Equal relationship where applicable,
14. technical privileges.

Technical privileges can then include an account, email, portal access, project services, and later Civic standing or authority only where those are separately established.

### What SASE evidences

**Voluntary participation.**  
The individual chose to perform the enrollment act. The person was not automatically enrolled from a public record, association roster, mailing list, scraped database, or administrative import.

**Expenditure of effort.**  
The participant must obtain the materials, prepare the envelope correctly, address it, apply postage, and mail it.

**Expenditure of money.**  
The cost is intentionally small, but it is not zero. The participant personally commits a modest resource to the act of joining.

**Acceptance of delay.**  
Participation privileges are not granted instantly. The prospective participant must tolerate the normal delay of a physical postal exchange before the process completes.

This does not claim to measure a personality trait. It establishes something observable and narrower:

> The participant was willing and able to complete a delayed, multi-step enrollment process before receiving participation privileges.

**Physical-channel evidence.**  
Where the project uses an address qualification, the postal exchange can provide evidence that the participation process successfully traversed the relevant physical delivery path.

### What SASE does not prove

SASE must not be overstated.

By itself it does not prove permanent legal identity, ownership, permanent residency, Civic standing, Affected Status, Same and Equal status, governance authority, truthfulness, or expertise.

It is the top-level participation act, not a universal identity proof.

### Why the friction is intentional

Consumer systems normally remove every possible delay and cost.

The Civic Infrastructure deliberately preserves a small amount of friction because participation is meant to be an intentional act rather than an impulse click.

Consumer convenience favors an instant account, minimal effort, and immediate gratification.

Civic participation here favors deliberate enrollment, observable effort, a small personal commitment, delayed privilege, and a bounded relationship.

This friction is part of the design.

---

## Project qualification is separate from participation

SASE answers:

> Did this human deliberately elect to participate?

It does not answer:

> In which bounded Civic project does this person qualify?

Those are separate questions.

The qualification rules belong to the particular project.

Examples may include a Kane County residential relationship, a Maker Community relationship, an Illinois Agriculture relationship, a Construction-sector relationship, a Non-Engineer qualification class, or another explicitly bounded community.

The design must therefore preserve three different concepts:

**Participation Gate**  
How did the human deliberately elect to participate?  
SASE answers this.

**Project Qualification**  
Why is this person eligible for this particular bounded project?  
Project-specific rules answer this.

**Technical Trust Domain**  
Which infrastructure is authorized to operate that project?  
A project intermediate CA under the Civic Infrastructure CA answers this.

---

## Civic Infrastructure CA hierarchy

The root trust authority is the **Civic Infrastructure CA**.

The root is not a County CA.

A county, maker community, industry sector, statewide project, qualification sector, or other bounded Civic project receives its own intermediate CA beneath the Civic Infrastructure CA.

The general hierarchy is:

- Civic Infrastructure CA — root trust authority
  - Project Intermediate CA
    - service certificate
    - service certificate
    - service certificate
  - another Project Intermediate CA
    - service certificate

Kane is one project:

- Civic Infrastructure CA
  - Civic Infrastructure Kane County IL CA
    - portal.diagnostics.kane-il.us
    - witness.diagnostics.kane-il.us
    - other Kane project services

Future bounded projects could take very different forms:

- Kane County project,
- Maker Community project,
- Illinois Agriculture project,
- Construction-sector project,
- Non-Engineers project.

These examples illustrate possible project boundaries; they are not assertions that those projects already exist.

The important architectural rule is:

> An intermediate CA represents a deliberately bounded Civic project, not merely another collection of user accounts.

Browser trust in a project CA establishes technical trust in the infrastructure serving that project. It does not, by itself, confer participation, current status, affected status, standing, or authority.

---

## Kane as the first bounded Civic project

Kane County is the first concrete implementation of the pattern.

Its deliberate limitation is established by several mutually reinforcing boundaries rather than one all-purpose identity mechanism.

The conceptual sequence is:

1. **SASE** — deliberate voluntary participation.
2. **Kane Project Qualification** — qualifying relationship to the Kane project.
3. **CURRENT RESIDENT** — present-tense relationship rather than historical membership.
4. **Affected Status** — whether the participant is actually affected by the institution or issue.
5. **Same and Equal** — whether another participant is a true peer for the HOA discussion.
6. **Kane Project CA** — technical trust for Kane project services.
7. **DNS / Mail Boundary** — bounded communications by default, with explicit exceptions where required.

No single layer is allowed to impersonate another.

---

## CURRENT RESIDENT

**CURRENT RESIDENT** is deliberately present-tense.

The infrastructure is concerned with the participant's current relationship to the qualifying residence or community, not with creating a permanent identity that follows a person forever.

A former resident and a current resident are not interchangeable merely because both once had a relationship to the same property.

CURRENT RESIDENT therefore contributes to the determination of present participation without claiming permanent identity.

---

## Affected Status

**Affected Status** asks whether a participant is presently affected by the institution, rule, event, or issue under discussion.

It is separate from SASE, browser trust, email access, project qualification, general participation, Same and Equal, and Civic standing.

A person may communicate with the infrastructure without having Affected Status for a particular issue.

Likewise, admitting an outside correspondent through an explicit communications exception does not manufacture Affected Status.

---

## Same and Equal

**Same and Equal** is a narrow relationship test for discussion of an HOA issue.

It does not mean that all participants everywhere are equivalent.

Two people are Same and Equal only when both occupy the same current homeowner relationship inside the **same Association** for the matter being discussed.

Examples:

- current homeowner + current homeowner, same Association → SAME AND EQUAL
- current homeowner + current homeowner, different Associations → NOT SAME AND EQUAL
- current homeowner + former homeowner, same Association → NOT SAME AND EQUAL
- homeowner + Board member, same Association → NOT SAME AND EQUAL

The Board-member case matters because the Board member occupies an institutional or governance role relative to the homeowner. Shared ownership does not erase that asymmetry.

The Kane project may therefore contain many Associations without turning them into one undifferentiated county-wide homeowner forum.

Association A can contain Same-and-Equal discussion among its current homeowners. Association B can independently contain Same-and-Equal discussion among its current homeowners. Cross-Association discussion may still be useful, but it is not Same and Equal.

---

## DNS and mail whitelist with explicit exceptions

The Kane communications environment is deliberately bounded by default.

The current mail design recognizes the Civic namespace under *.diagnostics.kane-il.us and rejects unapproved external origins by default.

Explicit external exceptions may be created for legitimate outside correspondents such as grant trustees or other deliberately authorized parties.

Those are communications exceptions only.

An external communications exception is not SASE participation, project qualification, CURRENT RESIDENT status, Affected Status, Same and Equal status, or Civic standing.

An outside party may therefore communicate with the project without becoming a Kane participant.

---

## Boundary principle

The Kane project is not simply people who happen to use a Kane County website.

It is a deliberately bounded Civic environment composed from independent evidence and trust layers:

- SASE voluntary participation,
- Kane project qualification,
- current-resident relationship,
- issue-specific affected status,
- Same-and-Equal peer relationship where applicable,
- project-specific technical trust,
- bounded communications by default,
- and explicit exceptions that do not confer participation.

This structure must remain visible in grant-map because the operator needs to understand both the physical infrastructure and the human rules that the infrastructure exists to preserve.

---

## Expansion model

The architecture is intentionally broader than a county while each individual project remains deliberately bounded.

Possible replication axes include:

**Geographic**
- another county,
- a multi-county region,
- Illinois,
- another bounded jurisdiction.

**Community**
- maker community,
- local technical cooperative,
- another defined community of practice.

**Industry**
- agriculture,
- construction,
- another industry sector.

**Qualification or relationship**
- homeowners,
- non-engineers,
- another explicitly defined participant class.

The reusable unit is not the Kane County website.

The reusable unit is:

- SASE participation gate,
- project-specific qualification,
- project intermediate CA,
- bounded services,
- explicit relationship semantics,
- and human-operable infrastructure.

That is the unit the Grant should make attractive and reproducible.

---

## Human infrastructure mapping mission

The operational map must allow a competent human operator who did not build the system to answer:

1. What logical Civic nodes exist?
2. Which public service names belong to each node?
3. Which proxy, relay, or ingress binds each public name to a backend?
4. Which physical host, VM, or LXD instance currently implements that backend?
5. How does a human reach and manage it from the home LAN, the public Internet, and the relevant WireGuard or private fabric?
6. Which management UI, URL, port, and authentication boundary is required?
7. How are containers and VMs themselves managed?
8. What dependencies must exist for the service to remain reachable?
9. What can move without changing the logical Civic node or public service identity?
10. What is still unknown or unverified?

---

## Governing infrastructure distinction

The map must keep these objects separate:

**CIVIC NODE** — logical public function.

**SERVICE IDENTITY** — DNS name, protocol, port, and human purpose.

**ROUTING / PROXY** — how that service identity reaches its backend.

**BACKEND SERVICE** — Hubzilla, Usermin, Webmin, Dovecot, Postfix, LXD UI, or another service.

**COMPUTE LOCATION** — physical host, VM, or LXD instance.

**NETWORK PATH** — public, LAN, WireGuard, or private service fabric.

**HUMAN MANAGEMENT** — exact URL, port, UI, and access context.

A DNS name is not a host.  
An IP address is not a management interface.  
A backend location is not a Civic node identity.

---

## Required access contexts

Every manageable object must be evaluated in all applicable contexts:

- HOME-LAB HUMAN
- REMOTE/PUBLIC HUMAN
- CIVIC INTER-HOST
- PUBLIC PARTICIPANT

If an access context does not apply, say so explicitly.

---

## Required host fields

Every host, VM, or LXD instance inventory must contain:

- NAME
- ROLE
- PHYSICAL/VIRTUAL PARENT
- PUBLIC ADDRESSES
- LAN ADDRESSES
- WIREGUARD ADDRESSES
- SERVICE NAMES
- HUMAN MANAGEMENT URLS
- MANAGEMENT PORTS
- PROXY/INGRESS PATH
- GUEST/CONTAINER MANAGEMENT
- DEPENDENCIES
- CURRENTLY HOSTED CIVIC SERVICES
- VERIFIED
- UNKNOWN

Every public service inventory must contain:

- CIVIC NODE
- SERVICE NAME
- PROTOCOL
- PUBLIC PORT
- HUMAN PURPOSE
- PUBLIC EDGE/PROXY
- TLS TERMINATION
- BACKEND ADDRESS
- BACKEND PORT
- BACKEND HOST/INSTANCE
- HOME-LAB PATH
- REMOTE/PUBLIC PATH
- DEPENDENCIES
- VERIFIED
- UNKNOWN

---

## Evidence rule

The mapping assistant must interrogate the live infrastructure.

It must not infer missing addresses, ports, routing, proxy targets, management surfaces, trust relationships, or backend locations.

A fact is marked **verified** only when it is obtained from the live host or configuration or directly demonstrated by a successful access path.

Unknowns remain visible as unknowns.

---

## Human-operability rule

A system is not considered mapped merely because another machine, script, or LLM can reach it.

For human-managed infrastructure, the map is incomplete until the operator can answer:

- Where do I point my browser or terminal?
- Which address?
- Which port?
- Which management application?
- From which network context?
- What does that management surface control?
- Which browser profile is authorized?
- Which CA roots or client certificates must that browser hold?

---

## Browser trust and client authentication are infrastructure

Browser state is part of the access path.

For every browser-managed service, grant-map must record the actual browser or browser profile used by the human operator and the trust material required by that browser.

At minimum record:

- DEVICE / WORKSTATION
- OPERATING SYSTEM
- BROWSER
- BROWSER PROFILE
- SERVICE
- ACCESS CONTEXT
- SERVER / CA TRUST REQUIRED
- CLIENT CERTIFICATE REQUIRED
- CERTIFICATE SUBJECT OR LABEL
- CERTIFICATE FINGERPRINT
- EXPIRY
- PRIVATE KEY LOCATION — never publish; record only where and how it is protected
- VERIFIED

Two certificate roles must never be conflated:

**Civic Infrastructure / Project CA trust** means the browser trusts project-issued server certificates.

**LXD client certificate** means the browser presents a client identity to LXD for mutual-TLS management access.

Installing project CA trust in one browser does not enroll another browser.

Installing an LXD client certificate in one browser profile does not authorize another browser profile.

A service is not considered human-accessible until the required trust and client material is documented for the browser actually used.

See grant-map/browser-access.md.

---

## Initial mapping target: annales

The first fully inventoried system is annales.

Verified starting facts:

- HOST: annales
- ROLE: Ubuntu LXD host; compute/inference plane
- HOME-LAB LAN: 10.0.0.36
- WITNESS WIREGUARD: 10.110.0.9
- WEBMIN — HOME LAB: https://10.0.0.36:10000/
- WEBMIN — REMOTE: https://198.58.111.109:10000/ then Webmin Servers Index on wg-pk, then annales at 10.110.0.9:10000
- LXD HUMAN WEB MANAGEMENT: https://10.0.0.36:8443/
- LXD web management is verified operational from the home lab.

Important network warning:

The home LAN uses 10.0.0.x, while the separate diagnostics WireGuard fabric also uses 10.0.0.0/24. Every reference to a 10.0.0.x address must therefore state its network context.

The next mapping work is to complete the live LXD inventory and map every instance, network, storage pool, trust identity, and management path.

---

## Repository working copy

A dedicated LXD instance on annales should hold the local working copy of git64bit/kane-rag-email used by the mapping assistant.

That instance is an **operator/mapping workbench**, not a production Civic service.

It should be isolated from production workloads and given only the reach and credentials required for infrastructure discovery and repository updates.

Its final name, addresses, management path, and permissions must themselves be recorded after creation.

---

## Credential boundary

Human-operability documentation must identify the account or credential class required for a service, but secrets do not belong in this repository.

Do not commit passwords, CA private keys, client private keys, trust tokens, recovery secrets, or private credential logs.

The operator maintains those separately.

The map records enough information to know which credential is needed and where it is privately maintained, without publishing the secret itself.

---

## Definition of done

The map is complete only when a competent human operator can answer, for every Civic service:

- What is it?
- Which Civic project and Civic node does it belong to?
- What public name does a human use?
- Where does traffic enter?
- Which proxy or relay handles it?
- Where does the backend run today?
- How do I reach it from home?
- How do I reach it remotely?
- Which exact URL and port do I use?
- Which browser profile or terminal client?
- Which project CA or client certificate is required?
- How do I manage the host?
- How do I manage its containers or VMs?
- Which credential class is required?
- What breaks if this component moves or fails?

If the answer depends on undocumented memory, the map is not complete.
