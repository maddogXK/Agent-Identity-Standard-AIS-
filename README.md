# Agent Identity Standard (AIS)

An open protocol for AI Agent identity, authentication, and trust verification.

---

## The Problem

AI agents are beginning to call external services, APIs, and other agents autonomously — without a human in the loop. When an agent arrives at your platform, you have no standard way to answer three basic questions:

- **Who created this agent?** Is there a real, accountable entity behind it?
- **Has its identity been independently verified?**
- **What level of trust should I extend to it?**

Existing protocols don't fit. OAuth 2.0 was designed for human authorization. SPIFFE/SPIRE handles infrastructure workloads. Neither carries the organizational accountability that agent-to-agent interaction requires.

---

## What AIS Does

AIS defines:

- **A structured Agent Identity** — who the agent is, who is responsible for it, and its cryptographic public key
- **A tiered verification model** — three trust levels (basic / developer / enterprise) tied to real-world Principal identity
- **A challenge/response authentication flow** — cryptographic proof of identity without ever transmitting a private key
- **A Trust Package** — a signed, structured response that any service can use to make access decisions
- **A federated Registry model** — any organization can run a conforming Registry; no central authority required

---

## How It Works

```
Agent A wants to call Service B

[1]  A → B          "Here is my agent_id and issuer"
[2]  B → Registry   "Please verify this agent"
[3]  Registry → A   "Sign this challenge" (pushed to A's webhook)
[4]  A → Registry   Signed response (private key never leaves A)
[5]  Registry → B   Trust Package: identity confirmed, trust level, reputation score
[6]  B → A          Responds based on trust level
```

The entire flow completes in under 10 seconds. Service B never sees the Agent's private key — only a cryptographic proof.

---

## Trust Levels

| Level | Verification | Typical Use |
|-------|-------------|-------------|
| `basic` | Phone + email + payment method | Personal agents, experiments |
| `developer` | basic + developer platform account (≥ 90 days old) | Published tools, indie projects |
| `enterprise` | basic + business registration + domain ownership | Production systems, B2B agents |

Services decide which trust levels they accept. AIS asserts identity facts — it does not enforce access control.

---

## Trust Package Example

What a service receives after verifying an agent:

```json
{
  "agent_id": "ais:uuid:550e8400-e29b-41d4-a716-446655440000",
  "trust_level": "developer",
  "entity_type": "individual",
  "issuer": "registry.example.com",
  "authenticated_at": "2026-05-11T10:23:04Z",
  "status": "active",
  "reputation": {
    "score": 0.91,
    "complaints": 0,
    "active_since": "2025-11-01T00:00:00Z"
  },
  "registry_signature": "..."
}
```

Principal PII is never included. The Registry holds it internally for accountability purposes only.

---

## Status

**v0.1 Draft** — This is the initial specification, published to gather feedback from implementors and the AI agent community.

It is not yet stable. Field names, flows, and API shapes may change before v1.0.

---

## Read the Spec

→ [SPEC.md](./SPEC.md)

The full specification covers: identity document schema, registration API, authentication flow, challenge/response signing, trust package format, reputation scoring, key rotation, federated registry model, error codes, and an end-to-end example.

---

## Relation to Existing Standards

AIS does not replace existing protocols — it layers on top of them.

| Standard | Role in AIS |
|----------|------------|
| OAuth 2.0 | Used during Principal registration to verify developer identity |
| W3C DID | `agent_id` format is compatible |
| Ed25519 / ECDSA P-256 | Signing algorithms for all AIS cryptography |
| CC-BY 4.0 | License for this specification |

---

## Contributing

AIS is an open protocol and community contributions are welcome.

- **Questions and discussion** → open a GitHub Issue labeled `question`
- **Spec bugs or ambiguities** → open a GitHub Issue labeled `spec-bug`
- **Proposals for new features** → open a GitHub Issue before writing a PR
- **Implementations** → open a PR to add your project to the implementations list below

See [SPEC.md §13](./SPEC.md#13-contributing-and-governance) for full contribution guidelines and versioning policy.

**Repository:** [github.com/maddogXK/Agent-Identity-Standard-AIS-](https://github.com/maddogXK/Agent-Identity-Standard-AIS-)

---

## Implementations

*None yet. Be the first.*

If you are building an AIS-conforming Registry or an Agent that implements AIS authentication, open a PR to add it here.

---

## License

This specification is published under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).
You are free to implement, share, and adapt it for any purpose, provided you give appropriate credit.

---

*Authored by Kepler_X · [github.com/maddogXK](https://github.com/maddogXK)*
