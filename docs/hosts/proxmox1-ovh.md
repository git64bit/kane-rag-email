# proxmox1 — OVH Infrastructure Host

## Status

Interrogated and accepted as the first documented host for the `kane-rag-email` implementation.

This file records observed host state and existing infrastructure. It does not redesign or reassign the existing services.

## Identity

```text
hostname:        proxmox1
provider/site:   OVH
platform:        Proxmox VE host
OS:              Debian GNU/Linux 13 (trixie)
kernel:          Linux 7.0.2-2-pve
architecture:    x86-64
hardware vendor: ASRockRack
hardware model:  E3C246D4U2-2T
```

## Capacity

```text
CPU threads:     8
RAM:             62 GiB total
RAM available:   ~20 GiB at interrogation
swap:            2 GiB
```

Storage observed:

```text
nvme0n1          ~894.3 GiB
nvme1n1          ~894.3 GiB
```

Boot/root use Linux RAID1-backed devices.

Both NVMe devices also contain large ZFS-member partitions.

Proxmox storage reported:

```text
local
type:       dir
total:      ~837 GiB
used:       ~98 GiB
available:  ~739 GiB
```

## Host networking

Public-facing bridge:

```text
vmbr0
IPv4: 51.222.108.132/24
IPv6: 2607:5300:203:8784::/64
default IPv4 gateway: 51.222.108.254
```

Private Proxmox bridge:

```text
vmbr1
IPv4: 192.168.1.1/16
IPv6: fd56:98f0:6a9::1/48
```

## Kane inter-host WireGuard

A new host-level WireGuard identity was created for this host.

```text
interface:   wg0
address:     10.110.0.21/32
hub:         wg-pk
hub address: 10.110.0.1
hub endpoint: 198.58.111.109:51820
```

Public key:

```text
8J3lfXFERlYicSULkKTKJLS0R4jq8cxIrBi+SA/1z3M=
```

The peer is installed on `wg-pk` as:

```text
# proxmox1 — OVH Proxmox host
AllowedIPs = 10.110.0.21/32
```

`wg-quick@wg0` is enabled at boot on `proxmox1`.

Connectivity was verified successfully:

```text
proxmox1 10.110.0.21
    -> wg-pk 10.110.0.1

3/3 ICMP replies
0% packet loss
```

### Network-role boundary

The host-level `10.110.0.0/22` WireGuard network is treated as the inter-host backplane.

It is distinct from the existing container/service WireGuard network described below.

## Existing Proxmox containers

### CT 101 — ingress1.diagnostics.kane-il.us

Existing infrastructure role:

```text
DNS
DANE
Kane CA
public ingress infrastructure
```

Resources:

```text
cores:   1
memory:  512 MiB
rootfs:  8 GiB
swap:    512 MiB
```

Networking:

```text
private: 192.168.1.101/16
public:  15.235.0.200/24
IPv6:    2607:5300:203:8784::101/64
WG:      10.0.0.101/24
```

### CT 102 — ipfs1.diagnostics.kane-il.us

Existing infrastructure role:

```text
IPFS
```

Resources:

```text
cores:   2
memory:  8192 MiB
rootfs:  48 GiB
swap:    512 MiB
```

Networking:

```text
private: 192.168.1.102/16
public:  15.235.0.96/24
IPv6:    2607:5300:203:8784::102/64
WG:      10.0.0.102/24
```

### CT 103 — mail1.internal.diagnostics.kane-il.us

Existing infrastructure role:

```text
mail
```

Resources:

```text
cores:   1
memory:  512 MiB
rootfs:  8 GiB
swap:    512 MiB
```

Networking:

```text
private: 192.168.1.103/16
WG:      10.0.0.103/24
```

### CT 104 — hubzilla1.internal.diagnostics.kane-il.us

Existing infrastructure role:

```text
infrastructure/service component
```

Resources:

```text
cores:   2
memory:  4096 MiB
rootfs:  32 GiB
swap:    512 MiB
```

Networking:

```text
private: 192.168.1.104/16
WG:      10.0.0.104/24
```

### CT 105 — orchestrator1.internal.diagnostics.kane-il.us

Existing non-infrastructure/application orchestration role.

Resources:

```text
cores:   2
memory:  4096 MiB
rootfs:  32 GiB
swap:    4096 MiB
```

Networking:

```text
private: 192.168.1.105/16
WG:      10.0.0.105/24
```

## Existing container WireGuard fabric

The OVH containers already participate in a separate WireGuard network:

```text
10.0.0.101  ingress1
10.0.0.102  ipfs1
10.0.0.103  mail1
10.0.0.104  hubzilla1
10.0.0.105  orchestrator1
```

This network is existing infrastructure and is not altered by the new host-level Kane inter-host backplane.

## Current interpretation for kane-rag-email

This host already provides the public/infrastructure side needed by the project:

```text
public IP connectivity
DNS
DANE
Kane CA
mail infrastructure
IPFS
large local storage
existing orchestration capability
```

No additional container has been created for `kane-rag-email` at this stage.

The current task is inventory and host preparation only.
