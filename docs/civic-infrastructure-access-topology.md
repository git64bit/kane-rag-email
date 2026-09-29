# Civic Infrastructure Access and Addressing Map

This document is the operator field map for the Kane Civic Infrastructure.

Its purpose is simple: **before connecting to anything, identify the access context.** The same service may be reached differently from a public computer, from the home lab, or from another Civic Infrastructure host.

Do not treat a DNS name, LAN address, WireGuard address, and public relay address as interchangeable. They describe different paths.

## 1. Access contexts

| Context | Use | Addressing rule |
|---|---|---|
| Public participant access | Library, borrowed computer, ordinary Internet connection | Use the public service name, for example `portal.diagnostics.kane-il.us` or `witness.diagnostics.kane-il.us`. |
| Home-lab operator access | Administration from the trusted local network | Use the host's LAN address where that service is intentionally exposed on the LAN. |
| Civic inter-host access | Service-to-service traffic between Civic Infrastructure systems | Use the appropriate WireGuard/private-fabric address. |
| Public infrastructure traffic | DNS, SMTP edge, public web ingress | Use the published public address or public DNS name. |

**Operator rule:** state the host and access context before issuing configuration commands.

Example:

```text
HOST: mail1
CONTEXT: Civic inter-host
ADDRESS: 10.0.0.103
PURPOSE: authoritative mailbox service
```

This prevents a public address from being mistaken for the machine itself.

## 2. Two private WireGuard fabrics

These are separate networks and must not be mentally merged.

### Fabric A — mx1 / diagnostics service fabric

```text
10.0.0.0/24

mx1            10.0.0.1
ingress1       10.0.0.101
ipfs1          10.0.0.102
mail1          10.0.0.103
hubzilla1      10.0.0.104
orchestrator1  10.0.0.105
```

Primary use: DNS, mail, Hubzilla, IPFS and supporting diagnostics services.

### Fabric B — wg-pk / witness fabric

```text
10.110.0.0/22

wg-pk               10.110.0.1
annales              10.110.0.9
witness-hubzilla     10.110.0.19
witness-ipfs         10.110.0.20
proxmox1             10.110.0.21
```

Primary use: witness/portal infrastructure and communication with the OVH/public transport plane.

## 3. Host and service map

| Host / service | Role | Public path | Private path | LAN path |
|---|---|---|---|---|
| `ingress1.diagnostics.kane-il.us` | Authoritative BIND9, DNSSEC, DANE, public/internal DNS views | `15.235.0.200` | `10.0.0.101` | Record when verified |
| `mx1.diagnostics.kane-il.us` | Public SMTP edge and public secondary DNS | `172.237.130.143` | `10.0.0.1` | n/a |
| `mail1.internal.diagnostics.kane-il.us` | Authoritative virtual mailbox server; Postfix + Dovecot | no ordinary public host path recorded | `10.0.0.103` | Record when verified |
| `ipfs1.diagnostics.kane-il.us` | IPFS infrastructure | `15.235.0.96` | `10.0.0.102` | `192.168.1.102` |
| `hubzilla1` | Diagnostics Hubzilla service | public ingress through diagnostics infrastructure | `10.0.0.104` | Record when verified |
| `orchestrator1` | Diagnostics orchestration | as configured | `10.0.0.105` | Record when verified |
| `wg-pk.diagnostics.kane-il.us` | Relay between witness fabric and public diagnostics mail path | `198.58.111.109` | `10.110.0.1` | n/a |
| `annales` | Compute/inference host; physical host of witness-hubzilla; human management via Webmin | public-library management enters through `wg-pk` Webmin at `198.58.111.109:10000`, then the Webmin Servers Index reaches annales at `10.110.0.9:10000` | `10.110.0.9` | `10.0.0.36:10000` (Webmin) |
| `witness-hubzilla` | Participant UNIX/Usermin and witness Hubzilla host | through `198.58.111.109` | `10.110.0.19` | LXD/LAN address must be recorded separately |
| `witness-ipfs` | Witness-side IPFS | through witness/public infrastructure as configured | `10.110.0.20` | Record when verified |
| `proxmox1` | OVH public transport-plane host | provider/public addressing as configured | `10.110.0.21` | n/a |

Only verified addresses belong in this table. Unknown LAN addresses are deliberately marked instead of inferred.

## 4. Public participant services

These are the names a participant should know.

```text
portal.diagnostics.kane-il.us:20000
    Usermin participant portal
    public address currently resolves through 198.58.111.109

witness.diagnostics.kane-il.us
    Witness Hubzilla
    public address currently resolves through 198.58.111.109
```

Public access is hostname-based because TLS, DANE and Civic Trust Enrollment are attached to service names, not to the operator's local addressing convenience.

A participant at a public library should not need to know any LAN or WireGuard address.

## 5. Human management is part of host identity

For operator-facing infrastructure, an IP address without its management service is incomplete documentation.

For `annales`, the verified human-management paths are:

```text
HOME LAB
    browser
      -> https://10.0.0.36:10000/
      -> Webmin on annales

PUBLIC LIBRARY / REMOTE OPERATOR
    browser
      -> https://198.58.111.109:10000/
      -> Webmin on wg-pk
      -> Webmin Servers Index
      -> annales at 10.110.0.9:10000
```

The WireGuard address `10.110.0.9` identifies the host on the witness fabric. The port `:10000` identifies the human management surface. Both are required to make the host operationally discoverable.

The home-LAN address `10.0.0.36` must not be confused with the separate diagnostics WireGuard fabric, which also uses `10.0.0.0/24`. These are different networks with overlapping RFC1918 address space. Every occurrence of a `10.0.0.x` address in documentation must therefore state its network context.

The LXD container-management UI for annales is not yet inventoried. Until it is, annales is only partially documented from the human operator's perspective.

## 6. Home-lab operator access

The operator may use LAN addresses directly when working from the trusted home network.

That is a different activity from participant access.

```text
PUBLIC PARTICIPANT
    service DNS name
        -> public ingress
        -> service

HOME-LAB OPERATOR
    LAN address
        -> host/container directly

CIVIC HOST
    WireGuard address
        -> private service fabric
        -> destination
```

The LAN inventory must therefore be maintained as a first-class part of this document. A LAN address must not be guessed from a WireGuard address or public DNS record.

## 7. DNS split

`ingress1` serves two views of `diagnostics.kane-il.us`.

```text
external view
    public DNS information

internal view
    public information as needed
    + private infrastructure information
    + private _mailauth policy
```

Important verified paths:

```text
public query -> 15.235.0.200 -> external view

mx1 private policy query
    -> 10.0.0.101
    -> internal view
```

`mx1/ns2` is a public secondary and transfers the **external** zone from `15.235.0.200`.

It must not transfer the internal view from `10.0.0.101`.

## 8. Mail path

Current authoritative mail path:

```text
Internet / approved sender
        |
        v
mx1.diagnostics.kane-il.us
public SMTP edge
172.237.130.143
        |
        | diagnostics.kane-il.us recipient
        v
mail1
10.0.0.103
Postfix virtual recipient
        |
        v
Dovecot LMTP
        |
        v
/var/mail/vmail/diagnostics.kane-il.us/<mailbox>/Maildir
```

Outbound witness-side mail currently follows:

```text
witness-hubzilla
        |
        | certificate-verified TLS
        v
wg-pk 10.110.0.1
        |
        | DNSSEC-validated DANE TLS
        v
mx1.diagnostics.kane-il.us
        |
        | OpenDKIM signs diagnostics.kane-il.us
        v
Internet
```

The `wg-pk -> mx1` DANE hop is verified over both IPv4 and IPv6. The complete 2026-09-29 mail-path validation, sender-policy test, DNSSEC correction, and known non-blocking warnings are recorded in:

```text
grant-map/MAIL-VALIDATION-2026-09-29.md
```

### Inbound authorization boundary

Verified policy:

```text
trusted Civic MTA path
    -> diagnostics.kane-il.us senders permitted

public Internet claiming *.diagnostics.kane-il.us
    -> rejected

factoryfouroh.info
    -> explicit external exception
    -> accepted only when SPF passes

other external domains
    -> rejected unless deliberately added as an exception
```

Private policy source:

```text
_mailauth.diagnostics.kane-il.us
    internal DNS view only
    queried at 10.0.0.101
```

Postfix on `mx1` uses the local `kane-mailauth-policy` service to consult that policy.

## 9. Participant SASE25SEP26A

The first production participant demonstrates why identities and hosts must remain separate.

```text
Canonical Civic participant ID
    SASE25SEP26A

Participant UNIX account
    host: witness-hubzilla
    login: sase25sep26a

Participant mail identity
    host: mail1
    address: sase25sep26a@diagnostics.kane-il.us

Mail storage
    /var/mail/vmail/diagnostics.kane-il.us/sase25sep26a/Maildir
```

The UNIX account and mailbox belong to the same participant but are not the same system account.

## 10. Addressing and management rule for future documentation

Every host document and every operational procedure should identify addresses using the same four fields:

```text
PUBLIC:
LAN:
WIREGUARD:
SERVICE NAMES:
HUMAN MANAGEMENT:
```

Use `not applicable` or `not yet verified` instead of leaving the reader to infer an address.

Every command block should begin with the machine on which it is run:

```text
HOST: mx1
HOST: ingress1
HOST: mail1
HOST: witness-hubzilla
HOST: annales
```

That convention is mandatory for this repository because several public names terminate on relays rather than on the machine whose service they expose.

## 11. Buildability test

A new operator should be able to answer these questions from this document alone:

1. Am I connecting as a public participant, a home-lab operator, or another infrastructure host?
2. Which network should I use?
3. Which machine actually owns the service?
4. Is the public DNS name a service endpoint, a relay, or the machine itself?
5. Which IP address is appropriate for this access context?
6. Which host receives the next configuration command?
7. Which URL/port does a human operator use to manage that host?
8. If the host runs containers/VMs, what human interface manages those guests?

If any answer requires reconstructing the topology from shell history, the documentation is incomplete.
