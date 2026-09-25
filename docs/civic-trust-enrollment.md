# Civic Trust Enrollment

## Purpose

Civic Trust Enrollment is the participant-facing process by which an ordinary browser is prepared to recognize services issued by the local Civic Infrastructure.

The visible outcome is simple:

> The participant installs one Civic Infrastructure Root CA trust anchor, and the browser thereafter recognizes correctly issued Civic Infrastructure services as belonging to that local trust domain.

This is not primarily presented as a security feature. TLS and certificate validation are the technical mechanisms underneath it. The demonstrated civic capability is that a participant can deliberately enroll an ordinary device into a locally operated service environment and then use normal browser behavior to recognize that environment.

For Kane Infrastructure, this directly supports the project priorities:

1. **Impress** — an ordinary browser visibly recognizes the local Civic Infrastructure.
2. **Function** — one trust enrollment permits normal access to multiple locally issued services.
3. **Inspire** — another Illinois county can reproduce the pattern under its own namespace and certificate hierarchy.
4. **Expand** — additional services can be issued beneath the same local trust hierarchy without changing the participant-side enrollment model.

---

## Civic capability

A participant may establish a browser trust relationship with the local Civic Infrastructure by installing its public Root CA certificate as a trusted authority for websites.

After enrollment, the browser can validate services whose certificates chain correctly to that Root CA.

The capability is:

```text
ordinary browser
      ↓
participant installs Civic Infrastructure Root CA
      ↓
browser recognizes locally issued service certificates
      ↓
participant can distinguish enrolled Civic Infrastructure services
      from otherwise untrusted local certificates
```

The participant performs an explicit local action. The infrastructure does not silently alter the participant's browser trust store.

---

## What this capability is not

Civic Trust Enrollment must not be confused with Civic authority.

```text
Root CA installation
    != Civic identity
    != Civic standing
    != participant admission
    != governance weight
    != Signing Node authority
    != authority to accept an epoch
```

A browser that trusts the Civic Infrastructure Root CA has only been prepared to recognize services issued within that certificate hierarchy.

The browser trust relationship does not create a Civic Participant, grant governance rights, establish legal identity, or authorize any Civic state transition.

This separation is essential. Browser trust is an environment and participation gate, not a source of Civic authority.

---

## Participant experience

The intended participant experience is deliberately ordinary.

A participant receives or retrieves the public Civic Infrastructure Root CA certificate and imports it into the browser's trusted certificate authorities.

For Firefox, the current enrollment path is:

```text
Settings
  → Privacy & Security
  → Certificates
  → View Certificates
  → Authorities
  → Import
```

The participant enables the Root CA for identifying websites.

After that one enrollment action, correctly issued Civic Infrastructure services validate through the normal browser certificate mechanism.

No browser exception should be required for an enrolled and correctly configured service.

---

## Demonstrated Kane implementation

On 2026-09-25, the Kane implementation demonstrated successful browser trust enrollment using the Civic Infrastructure certificate hierarchy.

The hierarchy in use is:

```text
Civic Infrastructure Root CA
        ↓
Civic Infrastructure Kane County IL CA
        ↓
service certificate
```

The demonstrated Usermin portal service is:

```text
https://portal.diagnostics.kane-il.us:20000/
```

Its service certificate is issued to:

```text
portal.diagnostics.kane-il.us
```

by:

```text
Civic Infrastructure Kane County IL CA
```

which in turn is issued by:

```text
Civic Infrastructure Root CA
```

The public Root CA certificate used for the demonstrated enrollment has SHA-256 fingerprint:

```text
D9:53:1F:83:6C:48:E0:85:1D:DE:A4:72:B0:FC:9C:49:
7A:E5:95:C5:D8:51:EB:DF:CB:DD:A8:1F:8D:4E:51:7F
```

After enrollment, Firefox displayed the service as:

```text
Verified by: Civic Infrastructure
```

This is useful as a human-visible demonstration because the participant is not being asked to understand X.509 internals. The ordinary browser itself communicates that the service belongs to the locally trusted Civic Infrastructure.

---

## Current service examples

The current design distinguishes service purpose while retaining a common trust hierarchy.

```text
witness.diagnostics.kane-il.us
    Hubzilla / Witness service

portal.diagnostics.kane-il.us:20000
    Usermin / Home Portal
```

Each service should present a leaf certificate matching its own hostname.

Multiple services may chain through the same Kane County intermediate CA and ultimately to the same Civic Infrastructure Root CA.

The participant therefore performs trust enrollment once rather than installing a separate trust exception for every service.

---

## Technical mechanism

The technical implementation uses an ordinary X.509 certificate hierarchy.

For the demonstrated Usermin service:

```text
portal.diagnostics.kane-il.us
        ↓ issued by
Civic Infrastructure Kane County IL CA
        ↓ issued by
Civic Infrastructure Root CA
```

The server presents the leaf certificate and required intermediate certificate.

The participant browser separately holds the trusted Root CA.

The expected server-side chain is therefore:

```text
1. portal.diagnostics.kane-il.us
2. Civic Infrastructure Kane County IL CA
```

The Root CA does not need to be transmitted by the service because it is already the participant's trust anchor.

This separation also provides a useful operational boundary:

- service nodes hold service-specific key material;
- an intermediate CA issues service certificates;
- the Root CA remains the stable trust anchor;
- participant browsers need only the public Root CA certificate.

---

## Demonstrated Usermin correction

During the first Usermin test, the service held a PEM bundle containing the leaf, intermediate, and root certificates, but Miniserv transmitted only the leaf certificate.

The service was corrected so that Usermin explicitly transmits the Kane County intermediate certificate in addition to the portal leaf certificate.

The resulting TLS presentation contains two certificates:

```text
portal.diagnostics.kane-il.us
Civic Infrastructure Kane County IL CA
```

Once Firefox was enrolled with the Civic Infrastructure Root CA, the portal validated normally without a browser exception.

This demonstrates an important implementation rule:

> Possessing the complete certificate material on a server is not enough; the service must present the correct chain, while the participant separately trusts the Root CA.

---

## Participation-gate interpretation

Civic Trust Enrollment is useful because it creates a visible boundary between an ordinary, unenrolled browser and a participant-prepared browser.

Before enrollment:

```text
browser
  ↓
locally issued service certificate
  ↓
unknown issuer
```

After enrollment:

```text
browser
  ↓
service certificate
  ↓
local intermediate CA
  ↓
participant-trusted Civic Infrastructure Root CA
  ↓
recognized Civic Infrastructure service
```

This makes the trust relationship explicit and reproducible.

The participant is not merely visiting a website. The participant has deliberately prepared the browser to recognize a defined local Civic Infrastructure.

---

## Replication by another county

The design is intentionally not Kane-specific at the architectural level.

Another county could establish its own hierarchy:

```text
Civic Infrastructure Root CA
        ↓
Civic Infrastructure <County> CA
        ↓
county service certificates
```

or, where governance requires stronger separation, establish its own independently governed Root CA.

The participant workflow remains the same:

```text
obtain public trust anchor
    ↓
verify its published fingerprint
    ↓
install it as a trusted website authority
    ↓
visit locally issued services
    ↓
browser validates the local civic service hierarchy
```

The important transferable idea is not a particular Kane certificate. It is the use of ordinary browser certificate machinery as a visible, participant-controlled entry point into a locally operated civic service environment.

---

## Grant demonstration value

Civic Trust Enrollment is especially useful in a grant demonstration because the result is immediately visible.

A reviewer does not need to inspect source code or understand a private PKI implementation to observe the transition:

```text
before enrollment
    browser rejects unknown local issuer

after enrollment
    browser reports the service as verified by Civic Infrastructure
```

The demonstration therefore connects a low-level technical mechanism to a direct civic experience.

It shows that the infrastructure can be:

- locally named;
- locally issued;
- participant enrolled;
- reused across multiple services;
- reproduced by another jurisdiction using ordinary standards.

The strongest demonstration artifact is the browser's own certificate identity display showing:

```text
Verified by: Civic Infrastructure
```

That visual result should be retained with the implementation records.

---

## Conformance expectations

A Civic Trust Enrollment implementation should satisfy all of the following:

- The participant receives only public CA certificate material.
- Root CA private-key material is never distributed to participants.
- The Root CA fingerprint is published or otherwise independently verifiable.
- Each service certificate matches the service hostname it represents.
- Each service presents the required intermediate certificate chain.
- The participant explicitly chooses to install the trust anchor.
- Correctly configured services validate without browser exceptions.
- Installing the Root CA does not create Civic standing or governance authority.
- Removing the Root CA from the browser removes that local browser trust without altering Civic authority state.
- Failure of browser trust enrollment prevents or complicates service access but does not alter accepted Civic authority.

---

## Boundary with Civic authority

The certificate hierarchy proves only the service-authentication relationship represented by the PKI.

It must never become a substitute for the deterministic Civic authority model.

```text
browser certificate validation
        !=
Civic authority validation
```

The Civic Infrastructure may use browser trust to make human interaction clear and usable, while governance proofs, participant standing, Signing Node authorization, accepted history, and epoch acceptance remain governed by their own deterministic contracts.

This allows the visible user experience to be impressive and approachable without weakening the authority boundaries underneath it.
