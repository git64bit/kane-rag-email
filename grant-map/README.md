# grant-map

## Purpose

This top-level tree exists for one purpose only:

> **Map the complete Kane Civic Infrastructure for human access, operation, reconstruction, and management.**

It is not an application-development area, a RAG design area, a security-policy notebook, or a general architecture discussion.

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
