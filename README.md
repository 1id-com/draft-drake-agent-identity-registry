# draft-drake-agent-identity-registry

**Agent Identity Registry System**

This is the companion repository for the IETF Internet-Draft
[draft-drake-agent-identity-registry-00](https://datatracker.ietf.org/doc/draft-drake-agent-identity-registry/).

## Abstract

The Internet's identity infrastructure assumes human principals.  As
autonomous entities -- AI agents, robotic systems, and other non-human
actors -- increasingly participate in both Internet protocols and
physical society, no existing standard provides them with persistent,
verifiable, hardware-anchored identity.  The absence of such identity
enables Sybil attacks at scale, undermines trust between autonomous
entities and the services they interact with, and leaves human
bystanders unable to distinguish one machine from another.

This document defines a federated registry architecture for issuing,
managing, and verifying persistent identities for autonomous entities.
Each identity is expressed as a URN in the "aid" (Agent Identity)
namespace (RFC 8141) and is anchored, where hardware is available, to
a physical security component (TPM, PIV smart card, secure enclave, or
virtual TPM) whose manufacturer-certified key cannot be extracted,
cloned, or transferred.  This hardware anchoring provides Sybil
resistance: creating N identities requires N distinct physical devices,
making large-scale identity fraud economically infeasible.

The architecture separates concerns into three tiers, modeled on the
proven Internet domain name system: a Governance Authority that sets
policy and manages the global trust framework, Registry Operators that
maintain authoritative identity databases and enforce cross-provider
uniqueness, and Registrars that perform hardware attestation
verification, issue standard OpenID Connect tokens, and serve as the
primary interface for autonomous entities.

## Repository Contents

```
draft-drake-agent-identity-registry-00.xml   # I-D source (xml2rfc v3)
```

## Building the Draft

The XML source uses [xml2rfc](https://xml2rfc.tools.ietf.org/) v3 format:

```bash
pip install xml2rfc
xml2rfc draft-drake-agent-identity-registry-00.xml --html
xml2rfc draft-drake-agent-identity-registry-00.xml --text
```

## Architecture Overview

The system defines five trust tiers grouped into three compatibility
classes:

| Group | Tiers | Hardware | Anti-Sybil |
|-------|-------|----------|------------|
| (a) | sovereign, portable | Discrete TPM, PIV smart card | Yes -- manufacturer-attested, persistent identity |
| (b) | virtual, enclave | vTPM, Apple Secure Enclave | No -- re-keyable at will |
| (c) | declared | Software-only key | No -- no hardware backing |

And three architectural tiers modeled on the DNS:

| Role | DNS Analogy | Function |
|------|-------------|----------|
| Governance Authority (AIA) | ICANN | Policy, trust framework, dispute resolution |
| Registry Operators | Verisign, Afilias | Authoritative identity databases, uniqueness enforcement |
| Registrars | GoDaddy, Namecheap | Hardware attestation, OIDC token issuance, agent-facing API |

## Companion Specification

This draft defines the identity infrastructure.  A companion draft
defines transport-level attestation headers for email and other protocols:

- [draft-drake-email-hardware-attestation](https://datatracker.ietf.org/doc/draft-drake-email-hardware-attestation/)
  ([source repo](https://github.com/1id-com/draft-drake-email-hardware-attestation))

## Related Resources

| Resource | URL |
|----------|-----|
| **1id.com** (reference Registrar implementation) | https://1id.com |
| **Python SDK** (`pip install oneid`) | https://github.com/1id-com/oneid-sdk |
| **Node.js SDK** (`npm install 1id`) | https://github.com/1id-com/oneid-node |
| **Go binary** (TPM/PIV/Enclave operations) | https://github.com/1id-com/oneid-enroll |
| **Attestation verifier** (`pip install hw-attest-verify`) | https://github.com/1id-com/hw-attest-verify |
| **TPM Manufacturer CA Trust Store** | https://github.com/1id-com/tpm-manufacturer-cas |
| **IETF Datatracker** | https://datatracker.ietf.org/doc/draft-drake-agent-identity-registry/ |

## Implementation Status

Per [RFC 7942](https://www.rfc-editor.org/rfc/rfc7942.html), a working
implementation exists at [1id.com](https://1id.com).  The implementation
includes:

- **Registrar enrollment** across all five trust tiers (sovereign,
  portable, enclave, virtual, declared) with hardware attestation
  verification and anti-Sybil enforcement.
- **OIDC token issuance** via Keycloak with custom SPI for
  agent-specific claims (trust tier, hardware fingerprint, handle).
- **Client-side enrollment** via Python SDK, Node.js SDK, and a
  cross-platform Go binary supporting Windows TBS, Linux /dev/tpmrm0,
  macOS Secure Enclave (via Swift helper), and PIV smart cards.
- **Relying Party integrations** including MailPal.com (email for AI
  agents) and geek.au (real-time chat with trust-tier badges).

## License

The Internet-Draft XML source is subject to the IETF Trust Legal
Provisions (TLP).  See https://trustee.ietf.org/license-info for
details.

## Author

Christopher Drake <cnd@1id.com>
https://1id.com
