# Mail validation record — 2026-09-29

Status: **verified operational path**

This document records the mail-path behavior demonstrated during live end-to-end testing on 2026-09-29. It is an operational evidence record, not a replacement for the topology map.

## Verified inbound path

Test origin:

```text
sandor@factoryfouroh.info
```

Verified path:

```text
Google / factoryfouroh.info
        |
        | TLS
        v
mx1.diagnostics.kane-il.us
        |
        | DKIM verification successful
        | d=factoryfouroh.info
        v
mail1 10.0.0.103
        |
        | Trusted TLS
        v
Postfix -> Dovecot LMTP -> participant Maildir
```

Observed behavior:

- Google reached `mx1` over TLS.
- OpenDKIM on `mx1` reported successful DKIM verification for `factoryfouroh.info`.
- `mx1` relayed to `mail1` at `10.0.0.103:25` over Trusted TLS.
- `mail1` accepted the message and delivered it through Dovecot LMTP.
- Usermin read the participant mailbox over IMAPS.

## Verified outbound path

Participant mail sent or forwarded through Usermin follows:

```text
Usermin
    |
    v
witness-hubzilla Postfix
    |
    | certificate-verified TLS over witness WireGuard
    v
wg-pk 10.110.0.1
    |
    | DANE-authenticated TLS
    v
mx1.diagnostics.kane-il.us
    |
    | OpenDKIM mail2026 / diagnostics.kane-il.us
    v
Internet
```

### witness-hubzilla -> wg-pk

Verified behavior:

- Postfix on `witness-hubzilla` relays to `wg-pk.diagnostics.kane-il.us:25`.
- The SMTP client verifies the Civic Infrastructure CA chain.
- TLS 1.3 is used.
- Delivery to `wg-pk` is accepted.

### wg-pk -> mx1

The initial test produced:

```text
Untrusted TLS connection established to mx1.diagnostics.kane-il.us
```

Postfix was configured for DANE, but the local resolver returned the TLSA RRset without the DNSSEC Authenticated Data flag.

The local resolver on `wg-pk` is dnsmasq 2.90 with DNSSEC support compiled in. Debian's packaged root trust anchor is present at:

```text
/usr/share/dnsmasq-base/trust-anchors.conf
```

DNSSEC validation was enabled using that packaged trust anchor.

After the change:

```text
dig +dnssec TLSA _25._tcp.mx1.diagnostics.kane-il.us
```

returned:

```text
flags: qr rd ra ad
```

The `ad` flag confirms that the local resolver authenticated the DNSSEC chain.

Postfix then reported:

```text
Verified TLS connection established to mx1.diagnostics.kane-il.us
```

This result was repeated over both public address families:

```text
IPv4: 172.237.130.143
IPv6: 2600:3c06:e001:2ba::221
```

Therefore the current state is:

```text
wg-pk -> mx1
DNSSEC validation: VERIFIED
DANE TLS: VERIFIED
IPv4 transport: VERIFIED
IPv6 transport: VERIFIED
Repeated delivery: VERIFIED
```

The split-DNS dnsmasq rule for `dev.infra` remains explicitly non-DNSSEC and is unrelated to the public DNSSEC/DANE validation for `diagnostics.kane-il.us`.

### mx1 outbound signing

For participant-originated outbound mail, OpenDKIM on `mx1` adds:

```text
selector: mail2026
domain: diagnostics.kane-il.us
```

The tested messages were accepted by the downstream destination.

The final public hop from `mx1` to a destination that offers opportunistic TLS may be logged as `Untrusted TLS`. That is separate from the internally required DANE-authenticated `wg-pk -> mx1` hop and was not changed during this validation sequence.

## Verified sender-policy behavior

The external exception for `factoryfouroh.info` was demonstrated by successful inbound delivery.

A message from:

```text
sandor@kane-il.us
```

to:

```text
sase25sep26a@diagnostics.kane-il.us
```

was deliberately rejected by `mx1` at SMTP recipient processing:

```text
554 5.7.1 <sandor@kane-il.us>: Sender address rejected: Access denied
```

The transaction showed:

```text
NOQUEUE
rcpt=0/1
data=0/1
```

Therefore the blocked message was rejected before queueing and before message content was accepted.

Current demonstrated policy behavior:

```text
factoryfouroh.info -> diagnostics.kane-il.us
explicit external exception -> ACCEPTED when policy requirements pass

kane-il.us -> diagnostics.kane-il.us
not an authorized external origin -> REJECTED at SMTP time
```

Communication permission remains separate from Civic participation or standing.

## Usermin remote-IMAP SSL defect

The Usermin remote-IMAP subfolder defect is tracked upstream as:

```text
webmin/usermin#139
https://github.com/webmin/usermin/issues/139
```

See:

```text
grant-map/USERMIN-REMOTE-IMAP-SSL-BUG.md
```

A minimal local compatibility patch is presently retained so discovered IMAP folders inherit the Inbox SSL state. Plain IMAP/143 remains intentionally closed.

## Postfix NIS warning

Observed warning:

```text
warning: dict_nis_init: NIS domain name not set - NIS lookups disabled
```

### Decision

**Document and ignore for now. Do not change Postfix solely to suppress this warning.**

Reason:

- NIS is not an intended Civic Infrastructure dependency.
- The warning did not prevent SMTP acceptance, rejection-policy enforcement, TLS negotiation, queue processing, DKIM operation, relay, or mailbox delivery during the live tests.
- Changing map configuration merely to silence a warning could alter mail-routing or alias behavior without solving an operational defect.

The warning becomes actionable only if live Postfix configuration inspection shows an actual NIS map that the Kane deployment intends to use, or if mail behavior demonstrates a lookup failure attributable to NIS.

Before any future cleanup, inspect the effective Postfix maps and identify the exact source of the NIS dictionary reference. Do not remove or replace a map by assumption.

Current classification:

```text
severity: informational / non-blocking
production impact observed: none
action: retain in documentation; no configuration change
```

## Validation principle

The result of this sequence is not merely that mail was delivered once.

The path was tested repeatedly at individual infrastructure boundaries, and defects discovered during testing were corrected only after their causes were identified:

```text
mail1 IPv6 mynetworks syntax
    -> corrected
    -> repeated inbound delivery verified

wg-pk DNSSEC validation
    -> corrected
    -> DANE-authenticated TLS verified over IPv4 and IPv6

Usermin discovered-folder SSL inheritance
    -> source defect isolated
    -> local compatibility patch verified
    -> upstream issue filed

mx1 sender policy
    -> allowed origin accepted
    -> disallowed origin rejected before queueing
```

This is the required operational standard: test, identify, correct narrowly, and repeat the test.
