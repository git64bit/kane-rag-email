# Usermin remote IMAP SSL subfolder defect

Status: reproduced on the operated Kane deployment, confirmed against current upstream source, and submitted upstream as `webmin/usermin#139` on 2026-09-29.

Upstream issue: https://github.com/webmin/usermin/issues/139

This document is written so it can be used directly as an upstream Usermin bug report.

## Summary

When Usermin is configured to use a **Remote IMAP server** with **SSL enabled**, the Inbox works correctly over IMAPS on port 993. However, folders auto-discovered from the same account with the IMAP `LIST` command do not inherit the Inbox SSL setting. Opening folders such as **Drafts** or **Sent** therefore attempts a plain IMAP connection on port 143.

For an IMAP server intentionally exposing IMAPS on 993 only, the result is:

```text
Failed to IPv6 connect to mail.diagnostics.kane-il.us:143
Connection refused
```

The same failure occurs for all auto-discovered folders except Inbox.

## Environment

Operated deployment:

- Usermin: 2.550
- Mail storage format: Remote IMAP server
- IMAP server: `mail.diagnostics.kane-il.us`
- SSL for IMAP: enabled
- Dovecot: IMAPS on TCP 993
- TCP 143 intentionally not exposed
- Usermin Inbox access: working
- Usermin Drafts/Sent/other discovered folders: fail by attempting TCP 143

The current upstream `webmin/usermin` source was also inspected. The relevant auto-discovered-folder construction remains unchanged there.

## Reproduction

1. In Webmin, configure the Usermin **Read Mail** module.
2. Set **Mail storage format** to **Remote IMAP server**.
3. Set the IMAP server hostname.
4. Enable **Use SSL when connecting to IMAP server**.
5. Use an IMAP server that exposes IMAPS on 993 and does not expose plain IMAP on 143.
6. Log into Usermin.
7. Authenticate to the remote mailbox.
8. Open **Inbox**.
9. Open **Drafts**, **Sent**, or another folder returned by the server's IMAP `LIST` response.

## Actual behavior

Inbox works over IMAPS/993.

Auto-discovered folders attempt port 143 and fail:

```text
Failed to IPv6 connect to mail.diagnostics.kane-il.us:143
Connection refused
```

Only the Inbox has working SSL behavior.

## Expected behavior

All folders belonging to the same remote IMAP account should inherit the Inbox connection properties, including SSL/TLS state and, where applicable, the explicit port.

With SSL enabled, auto-discovered folders should connect using IMAPS/993 unless explicitly configured otherwise.

## Source-level cause

In `mailbox/mailbox-lib.pl`, the Inbox folder object includes the configured SSL state:

```perl
elsif ($config{'mail_system'} == 4) {
        # IMAP inbox
        my $imapserver = $config{'pop3_server'} || "localhost";
        push(@rv, { 'name' => $text{'folder_inbox'},
                    'id' => 'INBOX',
                    'type' => 4,
                    'server' => $imapserver,
                    'ssl' => $config{'pop3_ssl'},
                    'mode' => 3,
                    'remote' => 1,
                    'flags' => 1,
                    'inbox' => 1,
                    'index' => 0 });
```

After Usermin successfully logs in and runs:

```text
LIST "" "*"
```

it constructs an object for each discovered folder. The discovered folder inherits the server, username, password and other state, but not `ssl`:

```perl
push(@rv,
  { 'name' => &decode_utf7($fn),
    'id' => $fn,
    'type' => 4,
    'server' => $imapserver,
    'user' => $rv[0]->{'user'},
    'pass' => $rv[0]->{'pass'},
    'autouser' => $rv[0]->{'autouser'},
    'mode' => 0,
    'remote' => 1,
    'flags' => 1,
    'imapauto' => 1,
    'mailbox' => $fn,
    'nologout' => $config{'nologout'},
    'index' => scalar(@rv) });
```

In `folders-lib.pl`, `imap_login()` chooses its default port from the folder's `ssl` value:

```perl
my $defport = $folder->{'ssl'} ? 993 : 143;
```

The auto-discovered folder has no `ssl` property, so it takes the non-SSL path and defaults to port 143.

## Configuration UI check

The normal IMAP folder editor was inspected.

`edit_imap.cgi` exposes:

- folder name
- IMAP server
- IMAP port
- username
- password
- remote mailbox

It does not expose an SSL setting.

`save_imap.cgi` saves:

- server
- port
- user
- password
- mailbox

It does not save an `ssl` property.

The generic `show_folder_options()` / `parse_folder_options()` functions likewise contain no SSL/TLS setting.

Therefore this behavior cannot be corrected for the discovered folders through the normal Webmin/Usermin configuration UI.

## Minimal likely fix

When constructing each auto-discovered folder, inherit the Inbox SSL property:

```perl
'ssl' => $rv[0]->{'ssl'},
```

For example:

```perl
'server' => $imapserver,
'user' => $rv[0]->{'user'},
'pass' => $rv[0]->{'pass'},
'ssl' => $rv[0]->{'ssl'},
'autouser' => $rv[0]->{'autouser'},
```

If the Inbox object can carry an explicit port, inheriting that port may also be appropriate.

## Kane deployment decision

The mail server will **not** enable plain IMAP/143 merely to accommodate this client-side behavior.

A minimal local compatibility patch is currently applied on the operated Usermin node:

```perl
'ssl' => $rv[0]->{'ssl'},
```

The patch was first used to test the suspected cause. It was retained after the test because it restored the expected behavior without weakening the IMAP transport policy.

Verified current behavior:

```text
Usermin -> Remote IMAPS -> mail1:993 -> Inbox works
Usermin -> Remote IMAPS -> mail1:993 -> Sent works
Usermin -> Remote IMAPS -> mail1:993 -> Drafts works
```

The local modification is intentionally narrow and remains tracked as a compatibility patch pending upstream resolution.

Upstream tracking:

```text
webmin/usermin#139
https://github.com/webmin/usermin/issues/139
status: open as of 2026-09-29
```

If upstream incorporates a fix, the local patch should be removed and the packaged/upstream implementation retested before the issue is considered closed for Kane.
