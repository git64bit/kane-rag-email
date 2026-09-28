# TODO_RAG-AI

## Purpose

This document defines the remaining design and implementation work for the participant-facing **RAG-AI** within the Civic Infrastructure.

The RAG-AI is not a general chatbot.

Its role is deliberately narrow:

- **advisory**,
- **educational**,
- and **clerical**.

It operates inside the participant's bounded Civic environment and is intended to work primarily with:

- plain-text civic email,
- participant-owned Civic artifacts,
- project documentation,
- statutes and governing documents,
- verified records,
- timelines,
- logs,
- manifests,
- and other source-grounded material.

The RAG-AI does not become a source of authority.

It does not determine Civic standing, legal rights, institutional guilt, governance authority, political outcomes, or participant status.

Its purpose is to help the participant understand, organize, retrieve, compare, explain, and prepare.

---

## Operating surface

The participant interaction model is:

```text
PARTICIPANT
    ↓
authenticated UNIX account
    ↓
Usermin
    ↓
text UI / ncurses-style interface
    ↓
RAG-AI
```

The text interface is deliberate.

The Civic Infrastructure already treats plain text as a preferred operating medium for:

- email,
- statutes,
- records,
- logs,
- timelines,
- evidence,
- configuration,
- and clerical work.

The RAG-AI should fit that environment rather than requiring a large independent web application.

---

## Mail integration

The Civic mail environment is designed to reduce incoming content complexity.

Inbound mail policy is intended to permit messages only from:

- `diagnostics.kane-il.us`,
- `*.diagnostics.kane-il.us`,
- and explicitly permitted external namespaces.

Inbound content is intended to be reduced to plain text.

HTML and attachments are not part of the participant-facing RAG ingestion path.

The RAG-AI should therefore be able to treat civic mail as a relatively canonical textual evidence stream.

### TODO

- Verify the live inbound sender policy.
- Verify exact namespace matching behavior.
- Verify explicitly permitted external namespaces.
- Verify whether unauthorized external mail is rejected, quarantined, or otherwise handled.
- Verify HTML stripping behavior.
- Verify attachment stripping behavior.
- Verify MIME normalization.
- Define the canonical stored representation of received plain-text mail.
- Define which headers are preserved.
- Define how message provenance is represented.
- Define how message hashes are calculated.
- Define whether original raw RFC message bytes are retained separately from normalized text.
- Define how replies and threads are represented for retrieval.

---

## Usermin integration

The RAG-AI should be available from the participant's authenticated UNIX environment.

The intended interface is a text-mode application compatible with a terminal-oriented workflow, including ncurses-style interaction where useful.

The participant should not need public SSH ingress to use the RAG-AI.

The RAG-AI operates through the participant's bounded Usermin-accessible account.

### TODO

- Identify the exact Usermin module or launch mechanism.
- Define whether the RAG-AI runs:
  - as a shell command,
  - as a Usermin custom command,
  - as a terminal application,
  - or through another bounded text interface.
- Confirm ncurses compatibility in the actual Usermin execution context.
- Define terminal width and character-set assumptions.
- Define session timeout behavior.
- Define process limits.
- Define CPU and memory limits.
- Define whether long-running RAG operations are allowed.
- Define how interrupted sessions recover.
- Define how participant history is stored.
- Define whether conversation state is participant-local or project-global.
- Define what files the RAG-AI may read from the participant home directory.
- Define what files it may write.
- Define whether it may create new Civic artifacts automatically.
- Define how generated artifacts are marked as AI-assisted.

---

## Advisory role

The advisory role helps the participant understand available information without making decisions on the participant's behalf.

Examples include:

- identifying relevant records,
- identifying unanswered questions,
- explaining procedural options,
- explaining how a documented process works,
- surfacing inconsistencies,
- identifying missing evidence,
- comparing a declared rule with an observed event,
- and showing which source supports a statement.

The advisory role must remain source-grounded.

### TODO

- Define advisory response format.
- Require source references for factual claims.
- Define confidence / uncertainty language.
- Define how conflicting sources are presented.
- Define how missing evidence is surfaced.
- Define how the system distinguishes:
  - verified fact,
  - inferred relationship,
  - unresolved question,
  - hypothesis,
  - and unsupported claim.
- Define whether advisory responses may include suggested next clerical steps.
- Define prohibited advisory outputs where the model would otherwise act as authority.

---

## Educational role

The educational role explains the Civic environment and source material.

Subjects can include:

- governing documents,
- statutes,
- institutional procedures,
- project terminology,
- Civic Infrastructure concepts,
- evidence handling,
- financial records,
- trust models,
- technical systems,
- grant concepts,
- and participant responsibilities.

The educational role should explain rather than persuade.

### TODO

- Define the educational corpus.
- Define authoritative source priority.
- Define how statutes and governing documents are versioned.
- Define how obsolete or superseded documents are marked.
- Define citation behavior.
- Define how explanations distinguish:
  - source text,
  - interpretation,
  - example,
  - and analogy.
- Define whether the participant can request:
  - plain-language explanation,
  - technical explanation,
  - procedural explanation,
  - or comparative explanation.
- Define how educational responses remain non-activist.

---

## Clerical role

The clerical role is expected to perform the largest amount of repetitive work.

Examples include:

- indexing files,
- extracting dates,
- building timelines,
- comparing document versions,
- summarizing correspondence,
- classifying records,
- tracking unanswered requests,
- preparing evidence inventories,
- generating structured notes,
- producing checklists,
- organizing project artifacts,
- preparing draft correspondence,
- and creating participant-facing summaries.

The clerical role may prepare.

It does not authorize.

### TODO

- Define supported clerical task types.
- Define output formats.
- Define naming conventions.
- Define file locations for generated artifacts.
- Define artifact metadata.
- Define provenance requirements.
- Define whether generated artifacts receive hashes.
- Define whether generated artifacts can be signed.
- Define participant approval flow before any externally visible output.
- Define whether the RAG-AI may alter existing files.
- Default to append/create rather than silent modification.
- Define versioning behavior.
- Define deletion restrictions.
- Define recovery behavior for generated artifacts.

---

## Authority boundary

The RAG-AI must never silently become the Civic authority.

The governing distinction is:

```text
RAG-AI
    may retrieve evidence
    may organize evidence
    may compare evidence
    may explain evidence
    may summarize evidence
    may prepare clerical output

RAG-AI
    does not create Civic standing
    does not determine legal rights
    does not determine guilt
    does not determine governance authority
    does not determine Same and Equal
    does not determine Affected Status by itself
    does not determine political outcomes
```

The source of truth remains the underlying records, signed artifacts, statutes, project rules, verified configurations, and human decisions where human authority is required.

### TODO

- Encode authority boundaries in the system prompt.
- Encode authority boundaries in application logic where possible.
- Define response behavior when asked to make an authoritative determination.
- Define escalation language for human review.
- Define whether certain determinations require explicit source bundles.
- Define an audit trail for high-impact advisory interactions.

---

## Source-grounding

Every substantive RAG response should be grounded in retrievable source material whenever the answer depends on Civic records.

The system should prefer:

1. verified project records,
2. current authoritative documents,
3. participant-owned artifacts,
4. verified email,
5. validated project documentation,
6. secondary explanatory material,
7. model general knowledge only where appropriate and clearly separated.

### TODO

- Define source priority.
- Define retrieval ranking.
- Define chunking strategy.
- Define metadata schema.
- Define temporal versioning.
- Define document supersession rules.
- Define duplicate handling.
- Define participant-private vs shared corpora.
- Define project-private vs public corpora.
- Define exact citation format.
- Define stale-source warnings.
- Define unsupported-answer behavior.
- Define retrieval-failure behavior.

---

## Participant privacy and data boundaries

The participant's RAG environment must respect project, participant, and artifact boundaries.

A participant should not gain access to another participant's private material merely because both exist in the same infrastructure.

Same and Equal does not automatically mean unrestricted file access.

### TODO

- Define per-participant corpus boundaries.
- Define shared-project corpus boundaries.
- Define Same-and-Equal shared material rules.
- Define owner-operator visibility.
- Define administrator visibility.
- Define audit access.
- Define retention policy.
- Define deletion policy.
- Define export behavior.
- Define whether RAG embeddings contain sensitive material.
- Define embedding-store access boundaries.
- Define whether embeddings are regenerated when source access changes.

---

## Resource boundaries

The participant UNIX account is quota-limited.

Current intended participant filesystem limits:

- soft limit: **240 MB**
- hard limit: **250 MB**

Mailboxes are stored on a separate node and do not consume this participant filesystem quota.

The RAG-AI must not silently defeat the quota model by generating excessive caches, indexes, or conversation histories in the participant home directory.

### TODO

- Define where embeddings are stored.
- Define whether embeddings count against participant quota.
- Define cache limits.
- Define conversation-history limits.
- Define artifact-generation limits.
- Define temporary-file cleanup.
- Define model-output size limits.
- Define warnings near soft quota.
- Define behavior at hard quota.
- Define whether project-level RAG storage is separate from participant filesystem quota.

---

## Network boundary

The participant shell environment has no ingress SSH.

Outbound network access may be permitted under policy.

The RAG-AI must not assume that participant accounts are remotely shell-accessible.

### TODO

- Verify actual network egress policy.
- Define permitted outbound destinations.
- Define whether RAG retrieval can access external sources directly.
- Prefer curated or pre-ingested sources for Civic work.
- Define DNS behavior.
- Define proxy requirements.
- Define outbound logging.
- Define whether the RAG-AI may initiate email.
- Define whether the RAG-AI may initiate external HTTP requests.
- Define which operations require explicit participant confirmation.

---

## Email directionality

The mail model and shell model are intentionally asymmetric.

```text
EMAIL
    egress: broad
    ingress: bounded

SSH
    ingress: none
    egress: permitted under policy
```

This directionality is a deliberate Civic Infrastructure property.

### TODO

- Verify mail egress policy.
- Verify mail ingress policy.
- Verify shell egress policy.
- Verify absence of SSH ingress.
- Document exceptions.
- Document whether owner-operators have different network policies.

---

## Plain-text canonicalization

Plain text is expected to be the preferred RAG ingestion format for civic mail.

This has operational advantages:

- simpler indexing,
- easier hashing,
- easier diffing,
- easier quoting,
- easier provenance,
- lower attack surface,
- fewer rendering ambiguities,
- and less hidden remote content.

### TODO

- Define canonical newline format.
- Define Unicode normalization.
- Define header normalization.
- Define quoting rules.
- Define signature stripping policy, if any.
- Define forwarded-message handling.
- Define mail-thread segmentation.
- Define duplicate detection.
- Define whether normalization changes are recorded.
- Preserve enough original material to prove the normalized form was derived correctly.

---

## RAG response modes

The interface should make role boundaries obvious.

Candidate modes:

```text
ADVISE
    explain options and unresolved questions

EDUCATE
    explain sources, rules, and concepts

CLERK
    organize, compare, summarize, and prepare artifacts
```

These modes may share one underlying model but should remain conceptually distinct.

### TODO

- Decide whether modes are explicit commands.
- Define mode-specific prompts.
- Define mode-specific output formats.
- Define whether the participant can switch modes mid-session.
- Define default mode.
- Define mode-specific tool permissions.
- Define mode-specific file-write permissions.

---

## Evidence-first interaction

The RAG-AI should prefer questions that can be answered from evidence already present in the participant or project corpus.

Where evidence is missing, the system should say so.

It should not fill gaps with confident narrative.

### TODO

- Add "evidence available / evidence missing" indicators.
- Add source count.
- Add source age.
- Add conflicting-source indicator.
- Add unresolved-fact list.
- Add "show source" command.
- Add "show timeline" command.
- Add "compare documents" command.
- Add "what is missing?" command.
- Add "what changed?" command.

---

## Civic activity / activism boundary

The RAG-AI may assist civic activity.

It may also provide general-purpose capabilities that participants could use in activist work, but activism is not the RAG-AI's governing purpose.

The RAG-AI should be able to:

- explain an existing right,
- identify an existing duty,
- compare a rule with observed behavior,
- summarize tax or spending data,
- explain resource flows,
- and organize evidence.

It should not itself campaign for a policy outcome.

### TODO

- Define neutral handling of advocacy requests.
- Separate factual analysis from requested persuasive drafting.
- Keep the Civic Infrastructure's own outputs non-activist.
- Define whether participant-authored activist artifacts can be stored as ordinary participant content without becoming Civic Infrastructure positions.
- Preserve provenance between participant position and infrastructure-generated factual analysis.

---

## Owner-operator relationship

Owner-operators may eventually operate RAG-capable Civic nodes.

That does not grant access to all participant content.

Owner-operation is an infrastructure role, not unlimited information authority.

### TODO

- Define owner-operator RAG responsibilities.
- Define tenant / participant isolation.
- Define project-level trust requirements.
- Define model-update authority.
- Define corpus-administration authority.
- Define audit responsibilities.
- Define recovery responsibilities.
- Define owner-operator access limits to private participant material.

---

## Logging and auditability

The RAG-AI should be auditable enough to reconstruct what evidence was used for consequential outputs.

This does not require retaining every token forever.

It does require enough provenance to explain significant clerical and advisory outputs.

### TODO

- Define interaction logging.
- Define source logging.
- Define tool-call logging.
- Define artifact provenance.
- Define participant-visible logs.
- Define operator-visible logs.
- Define retention period.
- Define privacy boundaries.
- Define hash strategy for consequential outputs.
- Define replay / reconstruction requirements.

---

## Failure behavior

The RAG-AI must fail conservatively.

Preferred failure behavior:

```text
missing source
    → say source is missing

conflicting sources
    → show conflict

insufficient evidence
    → do not resolve by guessing

tool failure
    → report tool failure

permission failure
    → do not bypass

quota failure
    → report resource boundary

model uncertainty
    → expose uncertainty
```

### TODO

- Define user-facing failure messages.
- Define retry behavior.
- Define degraded mode.
- Define offline mode.
- Define fallback retrieval.
- Define model-unavailable behavior.

---

## Definition of done for the first participant-facing RAG-AI

The first usable RAG-AI surface is complete only when a qualified participant can:

1. authenticate through the intended Civic account path,
2. open the text interface,
3. query participant-authorized Civic sources,
4. receive source-grounded answers,
5. distinguish advisory, educational, and clerical output,
6. inspect the supporting sources,
7. request a timeline or comparison,
8. generate a clerical artifact,
9. save that artifact within the participant's bounded storage,
10. identify that the artifact was AI-assisted,
11. operate without public SSH ingress,
12. remain inside the participant's resource quota,
13. receive clear errors when evidence or permissions are insufficient,
14. and exit without leaving uncontrolled processes or temporary data.

The RAG-AI is not complete merely because a language model can answer questions.

It is complete when it behaves as a bounded Civic participant tool.
