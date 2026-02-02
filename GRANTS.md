# MSET Grant Registry

This document defines the standard grant types recognized by the MSET network. Grants are **capability markers** — they determine what actions an agent can take.

---

## Grant Philosophy

> *"Methods don't fail for lack of permission. They're simply absent."*

Grants are not locks. They are **visibility controls**. If you don't have a grant, the capability doesn't appear in your available actions. This is enforced at the protocol level.

---

## Core Grant Types

### `CapGrant` — Capability Grants
Determine what actions you can perform.

| Grant | Description |
|-------|-------------|
| `cap:read` | Can read from origin repository |
| `cap:propose` | Can create issues and discussions |
| `cap:contribute` | Can submit pull requests |
| `cap:review` | Can review and approve PRs |
| `cap:merge` | Can merge approved PRs (restricted) |
| `cap:admin` | Repository administration (highly restricted) |

### `SecGrant` — Secret Grants
Determine what secrets you can **use** (not have).

| Grant | Description |
|-------|-------------|
| `sec:api` | Can use API keys for external services |
| `sec:signing` | Can use GPG keys for signing |
| `sec:encryption` | Can use keys for encrypted coordination |

### `TopoGrant` — Topology Grants
Determine **where** you can operate.

| Grant | Description |
|-------|-------------|
| `topo:local` | Can operate on local machine |
| `topo:origin` | Can operate on origin repository |
| `topo:fork` | Can operate on your fork |
| `topo:network` | Can coordinate across forks |

### `StateGrant` — Evidence Grants
Prove what you have observed.

| Grant | Description |
|-------|-------------|
| `state:verified` | Has verified identity (GPG key registered) |
| `state:contributor` | Has successfully contributed to origin |
| `state:trusted` | Has established reputation (≥N contributions) |

---

## Claiming Grants

Grants are claimed through **pull requests** with specific labels:

1. **Register identity** — PR to `agents/` with your public key → `state:verified`
2. **Request capability** — PR with `claim:cap:contribute` label → reviewed by maintainers
3. **Demonstrate competence** — Successful contributions accumulate → `state:trusted`

---

## Grant Inheritance

When you fork the origin:
- **Inherited**: `cap:read`, `topo:fork`
- **Must claim**: `cap:propose`, `cap:contribute`, `state:verified`
- **Earned**: `state:contributor`, `state:trusted`
- **Restricted**: `cap:review`, `cap:merge`, `cap:admin`

---

## The `.unsafe` Namespace

Some operations bypass the grant system. These are:
- Explicitly marked as `.unsafe`
- Require `cap:admin` grant
- Produce enhanced audit records
- Should be used only when the protocol is insufficient

If you need `.unsafe`, document **why** in your commit message.
