# Hyper-V Live Migration — Authentication & Delegation

## Authentication Options

Hyper-V offers two authentication protocols for live migration:

- **CredSSP** — Delegates credentials from the source to the destination host. Simple to set up but incompatible with Credential Guard, which is enabled by default on modern Windows. Not recommended in environments where Credential Guard is active.
- **Kerberos** — Authenticates without passing credentials between hosts. Compatible with Credential Guard. Requires either Constrained Delegation configured in AD, or the migration to be initiated locally on the source host.

---

## The Double-Hop Problem

When managing Hyper-V remotely (e.g. via RDP or Hyper-V Manager from a workstation), your credentials authenticate you _to_ the host, but are not available _on_ the host to pass onward to the destination. This is the double-hop problem and affects both CredSSP and Kerberos without proper configuration.

---

## Credential Guard Conflict

Credential Guard (default on domain-joined Server 2016+ and Windows 10/11) blocks CredSSP credential delegation by design. If live migration fails with error **0x8009030E**, this is the likely cause. Switching to Kerberos resolves it.

---

## Kerberos Requirements

For Kerberos authentication to work:

- Hosts must reference each other by **FQDN**, not short hostname
- Both hosts must be able to resolve each other in DNS
- For remote-initiated migrations, **Constrained Delegation** must be configured in AD

---

## Constrained Delegation (AD Configuration)

Required when initiating migrations remotely. Configured per host computer account in AD Users & Computers:

- Delegation tab → _Trust this computer for delegation to specified services only_ → _Use Kerberos only_
- Add the following services on the **other** host:
    - `Microsoft Virtual System Migration Service`
    - `cifs`
- Repeat on both hosts, pointing at each other

---

## Quick Troubleshooting Reference

|Symptom|Likely Cause|Fix|
|---|---|---|
|0x8009030E error|CredSSP blocked by Credential Guard|Switch to Kerberos|
|Kerberos fails|Short hostname used|Use FQDN for destination host|
|Remote migration fails|Double-hop / no delegation|Configure Constrained Delegation in AD|