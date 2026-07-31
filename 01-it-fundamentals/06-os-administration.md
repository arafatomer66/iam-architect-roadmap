---
title: Windows & Linux Administration
parent: 1. IT Fundamentals
nav_order: 6
---

# Windows & Linux Administration

## Why this is in an architecture roadmap

Because **the operating system is where access is finally enforced**, and because servers are a target system for provisioning like any other. When an IGA platform "grants access to a Linux server", something must actually happen in `/etc/passwd`, in SSSD's cache, in a sudoers file, or in a local group — and the architect must know what.

You don't need to be a sysadmin. You need to know how each OS represents identity locally, what the privileged paths are, and what an integration actually touches.

---

## Windows identity model

### Principals and tokens

When a user logs on, Windows builds an **access token** containing their SID, all group SIDs, and privileges. Every resource access compares that token against the object's **security descriptor** (owner + DACL + SACL).

- **DACL** — the list of allow/deny ACEs. Evaluated in order; **explicit deny wins** over allow at the same level, and explicit entries take precedence over inherited.
- **SACL** — what gets audited. If the SACL is empty, no access events are generated, and your detection design has nothing to work with.
- **Privileges** are separate from permissions: `SeDebugPrivilege`, `SeBackupPrivilege`, `SeImpersonatePrivilege`, `SeTakeOwnershipPrivilege`. Several of them are effectively "become SYSTEM" in one step, which is why local admin ≈ full control of the machine including all credentials on it.

{: .architect }
> **The token is built at logon.** Add a user to a group and their existing session doesn't have it — they must log off and back on. This is the single most common "but I granted it!" support call in Windows estates, and it means your provisioning SLA is not the same as the user's *experienced* SLA. Say so in the design.

### Local accounts and LAPS

Every Windows machine has local accounts and a local Administrators group. Two classic failures:

1. **The same local administrator password on every machine** — compromise one, own all of them. The fix is **LAPS / Windows LAPS**: unique, rotated, per-machine passwords stored in AD/Entra, with read access itself an audited entitlement.
2. **Local admin rights granted permanently to users** so they can install software. This defeats most endpoint controls. The mature pattern is JIT elevation via an endpoint privilege manager — see [PAM](../02-identity-fundamentals/18-privileged-access-management.md).

### Service accounts

| Type | Password management | Notes |
|:--|:--|:--|
| Local Service / Network Service / SYSTEM | Built-in | No password to manage; SYSTEM is machine-level god |
| Domain user account as a service | **Manual** | The problem child: static passwords, often shared, often over-privileged, SPN makes it Kerberoastable |
| **gMSA** (group Managed Service Account) | Automatic, 240-bit, rotated every 30 days by AD | The right answer for multi-host services |
| sMSA | Automatic, single host | Superseded by gMSA |

Moving user-based service accounts to gMSAs is one of the highest-value, least-glamorous pieces of work in an AD estate.

### Logon types worth knowing

Because they appear in every log you'll analyse: **2** interactive, **3** network (SMB, most remote access), **4** batch, **5** service, **7** unlock, **8** network cleartext, **9** new credentials (`runas /netonly`), **10** RemoteInteractive (RDP), **11** cached interactive. Type 10 and 9 are the ones attackers use most visibly; type 3 is the noise you must filter.

---

## Linux identity model

### The local files

```
/etc/passwd   name:x:UID:GID:GECOS:home:shell    (world-readable)
/etc/shadow   name:$6$salt$hash:lastchg:min:max:warn:inactive:expire
/etc/group    groupname:x:GID:member,member
/etc/sudoers  who may run what as whom
```

Identity is **numeric**: the UID and GID are what the kernel enforces, not the name. Two consequences:

- **UID collisions across systems are a real security problem.** If user `alice` is UID 1001 on host A and user `bob` is UID 1001 on host B, then files sharing an NFS mount are owned by "whoever is 1001 here". Consistent UID/GID allocation across an estate is an identity architecture decision.
- **UID 0 is root regardless of the name.** A second account with UID 0 is a hidden root account — a classic persistence trick and a thing your account reconciliation should detect.

### Centralised authentication

| Component | Role |
|:--|:--|
| **PAM** (Pluggable Authentication Modules) | The stack that decides *how* authentication, account, session and password operations happen. Where MFA modules get inserted |
| **NSS** (Name Service Switch) | Where user/group *information* comes from: files, LDAP, SSSD |
| **SSSD** | The modern daemon: talks to AD/LDAP/IPA, caches credentials, provides offline login, maps AD users to POSIX identities |
| **realmd / adcli** | Join a Linux host to an AD domain |
| **Kerberos (`krb5`)** | Ticket-based SSO to Linux services |
| **FreeIPA** | Full Linux identity domain: LDAP + Kerberos + CA + DNS + host-based access control + sudo rules centralised |

{: .concept }
> **PAM answers "can they authenticate?", NSS answers "who are they?"** They're separate stacks and can point at different sources. Being able to look someone up (`id alice` works) does not mean they can log in — the authorisation step is a separate PAM module (`pam_access`, `pam_sss` with HBAC rules). This distinction explains a large fraction of "the user exists but can't log in" tickets.

### sudo

`sudo` is Linux's privilege elevation model and it is **an entitlement that IGA should govern**:

```sudoers
%dba-team ALL=(oracle) NOPASSWD: /usr/local/bin/db-restart
alice     ALL=(ALL:ALL) ALL          # effectively root
```

The second line is root access. It should appear in your access reviews as such. Centralising sudo rules (FreeIPA, LDAP-based sudoers, or config management) is what makes them reviewable at all — sudoers files scattered across 4,000 hosts are ungoverned privilege by definition.

### SSH keys: the ungoverned credential

SSH public keys in `~/.ssh/authorized_keys` are a **standing, unexpiring, self-service credential**. Users add them, they never expire, nobody reviews them, and they survive the user's departure if the account isn't removed.

Better patterns, in ascending order of maturity:

1. Centrally managed `authorized_keys` (config management, or AD/LDAP-published keys).
2. **SSH certificates** — an SSH CA signs short-lived certificates; hosts trust the CA, not individual keys. Revocation becomes "stop signing", and expiry is automatic.
3. **Brokered/JIT access** — a PAM solution issues a short-lived credential or proxies the session, with recording.

{: .warning }
> Ask any organisation "how many SSH keys grant access to production, and who owns each?" The honest answer is almost always "we don't know." That is one of the largest ungoverned access surfaces in most enterprises, and it's invisible to IGA tools that only look at directories and applications.

---

## What an architect actually needs from this layer

When you design provisioning to servers, you are choosing among:

| Approach | Mechanism | Trade-off |
|:--|:--|:--|
| **Directory-joined** | AD/LDAP/IPA membership drives access; no local accounts | Cleanest; requires the host to reach a directory |
| **Config-management-driven** | Ansible/Puppet/Chef writes users and sudo rules from a source of truth | Good for immutable/cloud estates; the source of truth becomes the thing to govern |
| **Agent-based PAM** | An agent brokers access and elevation | Strong control and recording; licence cost and agent footprint |
| **Direct provisioning to the host** | IGA connector creates local accounts | Scales badly, drifts, hard to reconcile. Usually a legacy compromise |

And you must answer: **what happens when the directory is unreachable?** Cached credentials allow offline login (good for laptops, a governance question for servers), and their cache lifetime is a security parameter.

---

## Architect's checklist

- [ ] Are **UIDs/GIDs consistent** across the Linux estate, and who allocates them?
- [ ] Is **sudo centrally managed and visible to access reviews**, or scattered in local files?
- [ ] Are **SSH keys** inventoried, and is there a path to SSH certificates or brokered access?
- [ ] Is **LAPS** (or equivalent) deployed for local Windows administrator passwords, with read access itself governed?
- [ ] Are **domain user service accounts** being migrated to gMSAs, and is there a register of the remainder?
- [ ] Are **local accounts** on servers reconciled against IGA, including any UID 0 accounts?
- [ ] Is **audit policy / SACL** configured so the events your detection design assumes actually exist?
- [ ] What is the **offline credential caching** behaviour, and is it appropriate for servers vs laptops?
- [ ] How does a **leaver** lose access to a Linux host that was joined to the directory two years ago and hasn't been patched since?

---

**Next:** [Cloud Fundamentals](07-cloud-fundamentals.md) →
