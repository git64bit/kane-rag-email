# Host Inventory — annales

## Purpose

`annales` is an Ubuntu LXD host in the Kane Civic Infrastructure. It is the compute/inference-plane machine and physically hosts the `witness-hubzilla` container.

This document treats **human management access as part of the host inventory**. A host is not operationally documented merely because another machine or an LLM can reach it.

## Identity

```text
HOST: annales
ROLE: compute / inference plane; Ubuntu LXD host
PHYSICAL FUNCTION: hosts witness-side LXD containers
```

## Verified network identities

```text
HOME-LAB LAN
    10.0.0.36

WITNESS WIREGUARD FABRIC
    10.110.0.9

PUBLIC RELAY / REMOTE MANAGEMENT ENTRY
    wg-pk
    198.58.111.109
```

### Address-context warning

The home LAN uses `10.0.0.36`.

A separate diagnostics WireGuard fabric also uses `10.0.0.0/24`.

These are distinct networks with overlapping RFC1918 address space. Never write or act on a `10.0.0.x` address without stating whether it means **home LAN** or **diagnostics WireGuard**.

## Human management — Webmin

Webmin is infrastructure for this host, not an optional convenience.

### From the home lab

```text
https://10.0.0.36:10000/
```

This is the direct LAN management surface for annales.

### From a public library or other remote location

The verified management path is:

```text
browser
  -> https://198.58.111.109:10000/
  -> Webmin on wg-pk
  -> Webmin Servers Index
  -> annales
  -> 10.110.0.9:10000
```

The Webmin server entry for annales is configured with:

```text
Hostname or IP address: 10.110.0.9
Port:                   10000
Description:            annales
Server type:            Ubuntu Linux
SSL server:             Yes
```

This gives a human operator a remote control path even when the home-LAN address is unreachable.

## LXD management

Status: **not yet inventoried sufficiently**.

The host runs Ubuntu LXD, but the repository does not yet record the human web interface for managing LXD instances.

Required inventory:

```text
LXD VERSION:
LXD HTTPS API ADDRESS:
LXD WEB UI URL:
LXD TRUST/AUTH METHOD:
HOME-LAB ACCESS PATH:
REMOTE/WIREGUARD ACCESS PATH:
INSTANCE LIST:
STORAGE POOLS:
NETWORKS/BRIDGES:
PROFILES:
```

Until these fields are filled, the LXD layer is not considered fully documented.

## Known instance

```text
witness-hubzilla
    role: participant UNIX/Usermin + witness Hubzilla host
    WireGuard: 10.110.0.19
```

Additional LXD addressing, bridge addresses, profiles, storage, and management paths must be inventoried from annales rather than inferred.

## Operator rule

Every future annales procedure must state both:

```text
HOST: annales
ACCESS CONTEXT: home-lab | remote-via-wg-pk | inter-host
```

and, when a browser is required:

```text
MANAGEMENT URL: <scheme>://<address>:<port>/
```

A bare IP address is insufficient for operator documentation.
