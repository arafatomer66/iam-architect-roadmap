---
title: OT, IoT & Edge Identity
parent: 3. Identity Domains
nav_order: 6
---

# OT, IoT & Edge Identity

## Where identity meets the physical world

Operational technology (industrial control systems, SCADA, PLCs, building management, medical devices) and the Internet of Things (sensors, meters, vehicles, consumer devices) break several assumptions that IT identity takes for granted.

{: .concept }
> **In IT, the worst case is data loss. In OT, the worst case is physical harm.** That single difference reorders every priority: **availability and safety outrank confidentiality**, a security control that could stop a turbine is worse than the threat it mitigates, and a maintenance window may come once a year — if at all. An IAM architect who brings IT instincts into an OT environment without adjusting them will be, correctly, overruled.

---

## What's different

| Assumption in IT | Reality in OT/IoT |
|:--|:--|
| Devices are patched regularly | Systems run for 15–25 years, often unpatchable, often out of vendor support |
| Reboot is acceptable | A reboot may halt production or endanger people |
| Devices have compute and memory | Constrained hardware may lack the resources for standard TLS |
| Connectivity is continuous | Intermittent, low-bandwidth, high-latency, or air-gapped by design |
| Credentials can rotate | Certificates may be burned in at manufacture |
| One user, one device | Shared consoles, shift work, operators under time pressure |
| Change control is a ticket | Change may require regulatory re-certification |
| Availability target 99.9% | Availability target is *the point of the system* |

---

## Device identity

Every device needs an identity that is **provisioned at manufacture or commissioning**, because there is no user to enrol it later.

| Mechanism | Assurance | Notes |
|:--|:--|:--|
| **Hardware root of trust** (TPM, secure element, HSM) | Highest | Key generated in hardware and never extractable |
| **Birth certificate at manufacture** | High | Issued in a controlled factory environment; needs a supply-chain trust model |
| **Zero-touch provisioning** | High | Device proves its manufacturer identity, receives an operational identity on first connect |
| **Certificate issued at commissioning** | Medium–high | Enrolled by a technician on site |
| **Pre-shared key** | Low–medium | Common, weak; frequently identical across a whole product line |
| **Default credentials** | **None** | Still the leading cause of IoT compromise. Should be impossible by design |

**The lifecycle is longer than everything around it.** A device installed today may outlive the certificate authority that issued its identity, the protocol it speaks, the company that made it, and the person who commissioned it. Design for:

- **Certificate lifetimes measured in years**, with a mechanism to renew that doesn't require a site visit.
- **Crypto agility** — the ability to change algorithms in a fleet you can't easily touch. See [post-quantum](../08-frontier/05-post-quantum.md).
- **Ownership transfer** — devices are sold, sites are divested, contracts end. Who can re-provision, and how is the previous owner's access removed?
- **Decommissioning** — a device pulled from service still holds credentials and possibly data. Revocation must not depend on the device cooperating.

---

## Human access to OT

Often the more urgent problem, because it's where incidents actually start.

**The vendor remote-access problem.** Equipment vendors need remote access to maintain their systems. The historical pattern — a modem, a jump box with a shared password, a VPN account created in 2014 — is how a large share of OT incidents begin.

The correct pattern: brokered, [just-in-time](../02-identity-fundamentals/18-privileged-access-management.md), time-bound, session-recorded, approved per engagement, with no standing access and no direct network path.

**Shared operator consoles.** A control room console logged in as `OPERATOR` for the whole shift, because the alternative is an operator fumbling with a password during an alarm. Attribution is lost. Realistic mitigations: badge tap or passkey for fast switching, session-level attribution recorded separately, or accepting the shared account with a compensating control (camera, physical access log, two-person rule) documented as a risk decision. **Do not propose a solution that adds seconds to an emergency response** — it will be defeated on day one, and you'll have made things worse.

**Contractors and turnaround crews.** Hundreds of temporary workers during a shutdown, needing access for days. Bulk sponsored identities with hard expiry, and physical access tied to logical access.

**IT/OT convergence.** The Purdue model's network segmentation is eroding as OT systems connect to cloud analytics. Identity becomes a control where the network boundary used to be — which is exactly the [Zero Trust](../08-frontier/01-zero-trust.md) argument, applied where it's hardest.

---

## Protocols and constraints

Industrial and IoT protocols were largely designed before security was a requirement. Modbus, DNP3 and many fieldbus protocols have **no authentication at all** — possession of network access is authorisation. Newer options (OPC UA with security profiles, DNP3 Secure Authentication, MQTT over TLS with client certificates) are better but adoption trails.

For constrained devices: **DTLS** over UDP, **CoAP** instead of HTTP, **OSCORE** for object security, and elliptic-curve crypto because RSA is too heavy. Where devices genuinely can't do modern crypto, the answer is architectural — a gateway that terminates security on the device's behalf and enforces identity at the boundary. That gateway then becomes a critical trust component and must be governed as one.

---

## Fleet-scale management

At a million devices, per-device human decisions are impossible:

- **Group and hierarchy** by type, site, firmware version, owner.
- **Automated enrolment** with attestation — no manual step.
- **Staged rollout** for any credential or firmware change, with automatic rollback. A bad certificate pushed to an entire fleet is an unrecoverable outage if devices can't be reached to fix it.
- **Revocation at scale** that doesn't rely on each device polling a CRL it may never reach — short-lived credentials, or a broker that refuses connections centrally.
- **Anomaly detection** on device behaviour, since a compromised device usually behaves differently before it does harm.

{: .architect }
> **The identity architect's most useful contribution in OT is usually not a new system — it's a defensible model for who may touch what, and a brokered path for vendor and remote access.** OT engineers have deep domain knowledge and legitimate scepticism of IT-driven change. Come with: an understanding that safety and availability come first, a design that fails toward *operational continuity*, no requirement for reboots or latency in the control path, and a willingness to accept documented risk where the alternative is genuinely worse. Credibility here is earned by what you *don't* insist on.

---

## Architect's checklist

- [ ] Is there an **inventory of devices** with their identity mechanism and credential expiry?
- [ ] Do devices have a **hardware root of trust**, or shared/default credentials?
- [ ] How are device credentials **renewed without a site visit**?
- [ ] Is there an **ownership transfer and decommissioning** process, and does revocation work without device cooperation?
- [ ] Is **vendor remote access** brokered, time-bound, approved and recorded — with no standing access?
- [ ] Are **shared operator accounts** documented as accepted risk with compensating controls?
- [ ] Do any proposed controls add **latency or failure modes to the control path**?
- [ ] Are credential and firmware changes rolled out in **stages with rollback**?
- [ ] Is there a **crypto agility** plan for a fleet with a 20-year life?
- [ ] Have **safety and availability** been explicitly prioritised over confidentiality where they conflict, with sign-off?

---

**Next:** [Stage 4 — Architecture Practice](../04-architecture-practice/) →
