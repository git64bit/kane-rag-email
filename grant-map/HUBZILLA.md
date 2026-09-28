# HUBZILLA

## Purpose

This document explains why **Hubzilla** is admitted into the Civic Infrastructure.

Hubzilla is not the Civic Infrastructure itself.

It is an admitted participant-facing publication, identity, interaction, and interoperability surface whose capabilities satisfy several evidenced Civic Infrastructure requirements at the same time.

The admission question is not:

> Is Hubzilla an interesting or desirable social platform?

The admission question is:

> Does Hubzilla provide necessary and productive capabilities for bounded civic participation that would otherwise require multiple independent systems or substantial custom development?

For the Civic Infrastructure, the answer is yes.

---

## Governing technology-admission rule

The Civic Infrastructure admits technology only when an evidenced civic requirement exists and the technology productively satisfies that requirement.

The sequence is:

```text
EVIDENCED CIVIC NEED
        ↓
FUNCTIONAL REQUIREMENT
        ↓
TECHNOLOGY EVALUATION
        ↓
DEMONSTRATED PRODUCTIVE UTILITY
        ↓
ADMISSION
```

Hubzilla is admitted under this rule.

It is not admitted because it belongs to the Fediverse, because it is decentralized, because it is open source, because it is socially fashionable, or because similar technologies are useful to activist communities.

Those characteristics may be useful.

They are not the reason for admission.

---

# 1. Civic requirement: participants, not users

The Civic Infrastructure does not define people as **users**.

It defines them as **participants**.

A user conventionally consumes a service operated by someone else.

A Civic Infrastructure participant deliberately enters a bounded Civic project, qualifies for that project, accepts its relationship rules, and may contribute productive capacity to it.

That contribution can include:

- time,
- knowledge,
- technical skill,
- administration,
- compute,
- storage,
- bandwidth,
- equipment,
- physical space,
- or operation of infrastructure.

Software products may internally use the term "user." That terminology does not define the Civic relationship.

Hubzilla is useful because its architecture can support a participant as more than an account on a centrally controlled service.

---

# 2. Civic requirement: owner-operators

The Civic Infrastructure explicitly recognizes the **owner-operator**.

An owner-operator is a participant who contributes sufficient resources and accepts sufficient operational responsibility to own or control and operate a qualified Civic Infrastructure node or service.

The Civic Infrastructure does not measure expansion primarily by increasing the number of accounts on one operator's server.

It permits expansion by increasing qualified, independent, owner-operated capacity.

The model is:

```text
PARTICIPANT
    |
    +-- contributes civic effort
    |
    +-- contributes knowledge
    |
    +-- contributes productive resources
            |
            +-- sufficient resources and responsibility
                    |
                    +-- OWNER-OPERATOR
```

Owner-operation is therefore one expression of surplus capacity.

A participant with available compute, storage, bandwidth, technical ability, and operational commitment can contribute infrastructure rather than merely consume infrastructure.

This is consistent with the original practical value of federated systems: a node can be owned and operated by the person or group whose presence it serves.

The Civic Infrastructure preserves that model deliberately.

It does not depend on large public hubs containing hundreds or thousands of passive accounts.

---

# 3. Why Hubzilla fits the owner-operator model

Hubzilla can be self-hosted on ordinary web infrastructure and supports identities and channels that are not conceptually restricted to one permanent hosting location.

That makes it compatible with an owner-operated model.

A qualified participant can, where the Civic project permits it and where the participant can satisfy the technical requirements, operate a Hubzilla instance rather than remaining permanently dependent on another participant's instance.

The Civic Infrastructure therefore distinguishes:

```text
PARTICIPANT
    uses qualified Civic services

OWNER-OPERATOR
    operates qualified Civic services
    and participates through infrastructure
    the owner-operator controls
```

Owner-operation does not remove project qualification, trust boundaries, evidence requirements, or operational responsibilities.

Running a server does not create Civic standing.

It creates an operational role.

---

# 4. Civic requirement: bounded project surface

A Civic project must be deliberately bounded.

The project must be able to distinguish:

- participants from outsiders,
- qualified participants from mere correspondents,
- internal project relationships from public Internet relationships,
- and project-specific privileges from generic account access.

Hubzilla's configurable site, authentication, permission, access-control, and realm behavior make it suitable for a bounded participant-facing surface.

For the Civic Infrastructure, this capability is important because the project is not intended to become an unlimited public social network.

The application layer must support the same principle already present in the broader Civic Infrastructure:

```text
COMMUNICATION
    does not automatically confer

PARTICIPATION
    does not automatically confer

PROJECT QUALIFICATION
    does not automatically confer

CIVIC STANDING
```

Hubzilla is admitted because it can participate in enforcing that separation rather than forcing the Civic Infrastructure into an open-registration social-platform model.

---

# 5. Civic requirement: participant continuity independent of one host

A Civic participant's identity and civic presence should not be conceptually identical to one particular machine.

Physical infrastructure changes.

Hosts fail.

Domains move.

Operators retire equipment.

Services are migrated.

A Civic identity that exists only because one server remains alive creates unnecessary dependence on that server and its operator.

Hubzilla addresses this requirement through **nomadic identity**.

Hubzilla documentation describes channels as nomadic identities because the identity is not permanently tied to the hub on which it was originally created.

This matters to the Civic Infrastructure because it preserves a distinction already fundamental to grant-map:

```text
CIVIC IDENTITY / PARTICIPANT PRESENCE
        !=
CURRENT COMPUTE LOCATION
```

The participant relationship is logical.

The host is operational.

Those should not be confused.

---

# 6. Civic requirement: cloning and recoverability

Hubzilla allows a channel to be cloned to another Hubzilla hub.

Cloning supports operational continuity by allowing the participant's channel identity and synchronized state to exist at more than one location.

This is relevant to:

- resilience,
- migration,
- owner-operation,
- disaster recovery,
- and reduced dependence on one hosting instance.

The Civic Infrastructure does **not** interpret cloning as a guarantee that every byte, historical message, external dependency, or application state is replicated perfectly.

Hubzilla's own documentation notes limitations, including that some posts and messages are only available on a clone from the time cloning occurs.

The Civic Infrastructure therefore uses the narrower and evidenced proposition:

> Hubzilla cloning can reduce dependence on a single hosting instance by allowing a participant's channel identity, contacts, settings, and supported synchronized content to exist at more than one qualified Hubzilla location.

That capability is sufficient to make cloning materially relevant to Civic Infrastructure recoverability.

---

# 7. Civic requirement: reduce unnecessary operator dependence

The Civic Infrastructure does not claim that Hubzilla prevents de-platforming, moderation, server failure, administrative error, or loss of access.

Those claims would be too broad.

The relevant structural property is narrower.

Where nomadic identity and cloning are used correctly, a participant's Hubzilla presence need not be wholly dependent on one hosting operator or one physical server.

That matters because the Civic Infrastructure treats concentration of operational control as an observable dependency.

The design preference is:

```text
ONE OPERATOR
    should not become an unnecessary
    single point of participant continuity
```

Owner-operation, cloning, and identity portability can reduce that dependency.

They do not eliminate every dependency.

---

# 8. Civic requirement: extend function without creating a private platform fork

The Civic Infrastructure is intentionally kept small.

New civic requirements should not automatically force the project to maintain a large private application fork.

Hubzilla provides a modular and extensible application environment.

Its functions can be extended through apps, add-ons, modules, themes, widgets, and related mechanisms.

This is important because Civic-specific functionality can be added selectively while retaining the upstream platform where upstream functionality is sufficient.

The desired relationship is:

```text
UPSTREAM PLATFORM
        |
        +-- retain standard capability
        |
        +-- add narrowly justified Civic function
                |
                +-- avoid unnecessary private fork
```

The value is not merely that Hubzilla has plugins.

The value is that the Civic Infrastructure can add a bounded function without turning every new requirement into permanent ownership of an entire social-platform codebase.

---

# 9. Civic requirement: interoperability without surrendering project boundaries

A bounded Civic project still needs controlled communication with the outside world.

External communication does not have to imply external participation.

The Civic Infrastructure already applies this distinction to email:

```text
EXTERNAL COMMUNICATION
        !=
PROJECT PARTICIPATION
```

Hubzilla provides federation and bridge capabilities, including ActivityPub support through add-ons.

This is useful because a Civic project can remain bounded while still exchanging permitted information with wider federated networks.

The purpose of federation is therefore not unlimited social expansion.

It is **controlled interoperability**.

The governing distinction remains:

```text
INTEROPERABILITY
    ability to communicate across a boundary

IS NOT

QUALIFICATION
    permission to become a Civic participant
```

ActivityPub or other federation support does not confer SASE participation, project qualification, Affected Status, Same and Equal status, Civic standing, or owner-operator status.

---

# 10. Civic requirement: rich and durable publication

Civic activity cannot always be reduced to short posts, ephemeral chat, or simple email.

A participant may need to publish:

- a timeline,
- a long-form explanation,
- a structured observation,
- supporting documents,
- photographs,
- references,
- evidence,
- analysis,
- a persistent public statement,
- or collaborative material.

Hubzilla supports multiple publication forms, including ordinary posts, articles, web pages, wikis, files, forums, and other apps.

Its web-page tools support structured pages and multiple markup forms, while article functionality supports longer-form publication.

This matters because the Civic Infrastructure needs more than conversation.

It needs **persistent participant publication**.

The publication surface can therefore complement other Civic Infrastructure components:

```text
EMAIL
    direct communication

RAG
    retrieval, analysis, and assistance

HUBZILLA
    participant publication,
    interaction,
    identity continuity,
    and selected interoperability

EVIDENCE / PROVENANCE SYSTEMS
    preservation and verification
```

No one component is expected to perform every function.

---

# 11. Civic requirement: permissions and relationship-aware access

The Civic Infrastructure contains relationships that are not interchangeable.

Examples include:

- participant,
- outsider,
- current resident,
- affected participant,
- Same-and-Equal peer,
- Board member,
- external correspondent,
- owner-operator.

A participant-facing application must therefore support more than a binary public/private model.

Hubzilla provides fine-grained access-control and permission mechanisms for posts, profiles, files, pages, and other resources.

This capability is relevant because Civic relationships can be represented through controlled visibility and interaction surfaces without making every Civic artifact universally public.

The Civic Infrastructure does not assume that every Hubzilla permission corresponds automatically to a Civic relationship.

Civic relationships are established by Civic rules.

Hubzilla permissions are one mechanism for enforcing the resulting access decisions.

---

# 12. Civic requirement: participant-owned publication surface

An owner-operated Hubzilla node has another useful property: the publication surface can be hosted under infrastructure controlled by the participant or bounded group rather than solely under a third-party platform.

That does not make the content automatically trustworthy.

It does establish an important operational fact:

> the publishing participant or group can control the infrastructure on which the publication surface resides.

For the Civic Infrastructure, this is materially different from publishing entirely through a commercial platform whose terms, moderation rules, account availability, service continuity, and product direction are externally controlled.

The value is operational control, not ideological decentralization.

---

# 13. What Hubzilla is not

Hubzilla must not be allowed to redefine the Civic Infrastructure.

Hubzilla is not:

- the Civic Infrastructure itself,
- the source of Civic standing,
- the source of project qualification,
- the source of Affected Status,
- the source of Same and Equal status,
- a substitute for evidence,
- a substitute for statutory rights,
- a substitute for project governance,
- or an excuse for unrestricted public registration.

A Hubzilla account or channel does not automatically create a Civic participant.

A Hubzilla administrator does not automatically become a Civic authority.

An owner-operator does not automatically become a governance authority.

The application remains subordinate to the Civic Infrastructure's participation, qualification, trust, evidence, and relationship rules.

---

# 14. Why Hubzilla is admitted

Hubzilla is admitted because one platform provides a combination of capabilities that map directly to evidenced Civic Infrastructure needs:

| Civic Infrastructure requirement | Hubzilla capability |
| --- | --- |
| Bounded participant-facing surface | configurable site, realm, authentication, permissions, access controls |
| Owner-operated capacity | self-hosted hub model |
| Participant continuity beyond one host | nomadic identity |
| Recoverability and host independence | cloning and channel portability |
| Narrowly extendable application surface | apps, add-ons, modules and related extension mechanisms |
| Controlled outside interoperability | federation add-ons including ActivityPub |
| Long-form and structured publication | articles, web pages, wikis, files and related publishing tools |
| Relationship-aware visibility | granular permissions and access controls |
| Participant-controlled infrastructure | owner-operated deployment |

No individual capability is sufficient by itself.

The value is the combination.

---

# 15. Owner-operator expansion model

The Hubzilla role in Civic Infrastructure expansion is not:

```text
ONE LARGE HUB
        ↓
MORE USERS
        ↓
MORE USERS
        ↓
MORE USERS
```

The preferred Civic model is:

```text
BOUNDED CIVIC PROJECT
        |
        +-- qualified participants
        |
        +-- surplus capacity
                |
                +-- qualified owner-operators
                        |
                        +-- additional operated capacity
                        +-- additional resilience
                        +-- additional publication surfaces
                        +-- additional qualified nodes
```

Growth is therefore measured by productive participation and useful operated capacity, not merely by account count.

This is one reason the term **owner-operator** is important.

The Civic Infrastructure is not trying to convert everyone into an infrastructure operator.

It does, however, preserve a path for any qualified participant with sufficient resources, competence, and commitment to contribute at that level.

---

# 16. Technology neutrality remains intact

Hubzilla's admission is conditional on utility.

It is not permanent ideological allegiance.

If Hubzilla ceased to satisfy the Civic Infrastructure requirements, or another technology satisfied the same evidenced requirements substantially better, the Civic Infrastructure would evaluate that technology under the same admission rule.

The Civic Infrastructure is committed to the requirements, not to the product.

This is the same rule that governs blockchain, tokens, AI systems, distributed storage, PKI, email, and every other technical component.

---

# 17. Grant relevance

Hubzilla demonstrates the technology-admission discipline in concrete form.

The Civic Infrastructure did not begin with:

```text
HUBZILLA
    ↓
find a civic purpose for it
```

The reasoning is:

```text
BOUNDED CIVIC PARTICIPATION
OWNER-OPERATION
IDENTITY CONTINUITY
RECOVERABILITY
CONTROLLED INTEROPERABILITY
RICH PUBLICATION
EXTENSIBILITY
RELATIONSHIP-AWARE ACCESS
        ↓
technology evaluation
        ↓
HUBZILLA
        ↓
admission
```

This is grant-relevant because it demonstrates that the project is not accumulating fashionable technologies.

It identifies an operational need first and adopts technology only when the technology materially satisfies that need.

Hubzilla is therefore evidence of the Civic Infrastructure's selection discipline as much as it is an application component.

---

# 18. Evidence boundary

This document distinguishes Hubzilla's documented capabilities from the verified state of the Kane deployment.

Hubzilla documentation establishes that the software supports capabilities including:

- nomadic identities,
- channel cloning,
- granular permissions,
- federation through add-ons,
- ActivityPub interoperability,
- modular apps and extensions,
- articles,
- web pages,
- wikis,
- files,
- forums,
- and self-hosted operation.

The presence of a capability in Hubzilla does not prove that the capability is enabled, configured, or verified in the Kane deployment.

Deployment-specific facts belong in the live infrastructure map and must be marked verified only when directly observed.

This distinction is mandatory:

```text
PRODUCT CAPABILITY
        !=
DEPLOYED CONFIGURATION
        !=
VERIFIED CIVIC OPERATION
```

---

## Reference documentation

Hubzilla's current documentation describes the relevant upstream capabilities, including nomadic identity and cloning, federation add-ons and ActivityPub support, modular apps, granular access control, articles, web pages, wikis, forums, files, and self-hosted operation.

Primary upstream references include:

- Hubzilla Help — About
- Hubzilla Help — Functions
- Hubzilla Help — Accounts, Profiles and Channels
- Hubzilla Administrator Manual — Federation Addons
- Hubzilla Help — Articles
- Hubzilla Help — Websites

The Civic Infrastructure documentation relies on those upstream sources for product capability and on `grant-map` for verified deployment state.
