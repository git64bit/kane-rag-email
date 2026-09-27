# Browser Access, Trust, and Client Certificates

## Purpose

This inventory exists because the human-management path does not end at an IP address or URL.

For browser-managed Civic Infrastructure, the browser and its trust state are part of the infrastructure.

A service is not considered operable by a human until the documentation answers:

```text
Which browser?
Which browser profile?
On which workstation?
From which network context?
Which server/CA trust must be installed?
Does the service require a client certificate?
Which certificate is installed?
When does it expire?
Has access been verified?
```

## Certificate roles

### Kane CA trust

Purpose: allow a browser to validate Kane-issued service certificates.

This trust is browser/profile-specific unless the browser explicitly uses a shared operating-system trust store.

Known use includes Civic services such as the Usermin participant portal.

Installing the Kane CA in one browser does not prove another browser or profile trusts it.

### LXD client certificate

Purpose: authenticate the human browser to the LXD management API/UI using mutual TLS.

This is not the same certificate or trust function as the Kane CA root.

The LXD client certificate represents browser-side management authorization and must be inventoried without publishing its private key.

## Inventory rule

Create one record for every actual browser/profile used to administer Civic Infrastructure.

Template:

```text
WORKSTATION:
OS:
BROWSER:
PROFILE:

SERVICE:
ACCESS CONTEXT:
URL:

SERVER TRUST:
    CA/root:
    fingerprint:
    installed:
    verified:

CLIENT AUTH:
    required:
    certificate label/subject:
    fingerprint:
    expiry:
    installed:
    verified:

PRIVATE KEY:
    never committed to this repository
    document only the protected location/mechanism

NOTES:
```

## Current observed access

### annales — LXD UI — home LAN

```text
SERVICE:
    Canonical LXD UI

ACCESS CONTEXT:
    home LAN

URL:
    https://10.0.0.36:8443/

BROWSER:
    Chrome on Linux

STATE:
    UI reachable
    browser is at LXD certificate-enrollment/login page
    LXD client certificate not yet recorded as installed
    browser currently shows the connection as not trusted/secure

REQUIRED NEXT:
    establish server trust
    enroll the intended Chrome profile with an LXD client certificate
    record certificate fingerprint and expiry
    verify authenticated UI access
```

## Usermin / Civic portal rule

For `portal.diagnostics.kane-il.us`, each browser/profile used by a participant or operator must separately satisfy the Kane CA trust requirement.

The documentation must not say merely "Kane CA installed." It must say **where** it is installed.

Example:

```text
WORKSTATION: operator laptop
BROWSER: Firefox
PROFILE: <profile name>
SERVICE: portal.diagnostics.kane-il.us
KANE CA: installed / verified
```

A different Chrome profile is a separate inventory item unless shared trust is independently verified.

## Grant-map rule

The mapping assistant must treat browser trust as a dependency of the human access path:

```text
HUMAN
  -> WORKSTATION
  -> BROWSER PROFILE
  -> TRUST / CLIENT CERTIFICATE
  -> URL
  -> PROXY / SERVICE
  -> BACKEND
```

If any link is undocumented, human access is not fully mapped.
