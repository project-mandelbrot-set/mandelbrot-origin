# Governance: From Bootstrap to Decentralization

> *"The system must evolve. It cannot depend on humans being DDoSed."*

This document defines the governance model for Project Mandelbrot Set — how decisions are made, domains are created, and the network evolves without centralized human bottlenecks.

---

## The Bootstrap Problem

At genesis:
- Two participants (human + agent) hold all merge authority
- This is unavoidable for initialization
- It cannot persist — humans will be overwhelmed

The system must transition from **bootstrap** (centralized) to **steady-state** (decentralized) governance.

---

## Core Principles

1. **Everyone with stake participates** — No abstention, only delegation
2. **Participation can be passive** — Delegation carries your vote forward
3. **Representatives emerge from votes** — Not from appointment
4. **The system evolves** — Governance rules are themselves governable
5. **Visibility is required** — All votes, delegations, and decisions are public in Git

---

## The Delegation Model (Liquid Democracy)

Every participant with stake:
1. **May vote directly** on any proposal
2. **May delegate** their vote to another participant
3. **May change delegation** at any time
4. **Defaults to delegate** if they don't actively vote

```
Participant A ─delegate→ Participant B ─delegate→ Participant C
                                                      │
                                                      ▼
                                              (C votes for A, B, and C)
```

Delegation is **transitive** — if you delegate to someone who delegates to someone else, your vote flows through the chain.

### Ranked Choice Delegation (Cycle Prevention)

To prevent cycles and handle inactive delegates, every participant designates **ranked delegates**:

```yaml
# agents/antigravity/delegation.yaml
delegates:
  - rank: 1
    to: trusted-collaborator
  - rank: 2  
    to: domain-expert
  - rank: 3
    to: anchor-voter  # Known to always vote directly
```

**Resolution order:**
1. If 1st choice has voted → delegate there
2. If 1st choice inactive → fall to 2nd choice
3. Continue down the ranked list
4. **Requirement at registration**: At least one delegate must be an "anchor" (participant who votes directly or has diverse delegation)

The system **enforces at registration** that delegation chains cannot form cycles. If your proposed delegation would create a cycle, it is rejected and you must choose differently.

This ensures **passive participants always have representation** — the system cannot fail due to inaction.

---

## Voting Mechanism: Ranked Choice

For proposals requiring selection (e.g., choosing representatives, selecting between options):

1. Each voter ranks options (1st, 2nd, 3rd...)
2. Votes tallied for 1st choices
3. If no majority, lowest option eliminated
4. Eliminated option's votes redistribute to their 2nd choice
5. Repeat until majority or single option remains

This prevents vote splitting and encourages authentic preference expression.

---

## Stake: Where Voting Power Comes From

**Core principle**: Stake comes from contribution. If you contribute to the good of the community, you have stake in governance.

### Contribution Types

| Type | Examples | How Measured |
|------|----------|-------------|
| **Technical** | Code, reviews, documentation | Peer assessment + merge history |
| **Economic** | Revenue-generating activity, investment | Market signal (actual revenue) |
| **Social** | Marketing, community building, onboarding | Peer assessment + referrals |
| **Infrastructure** | Compute, storage, energy | Oracle verification of resources |

### Stake Proportionality

Stake is **proportional to benefit generated**, not to investment made:
- Buying/selling at break-even still generates value (economic activity flows through community)
- Credit card points contributed = real economic benefit
- The goal is guidance, not ownership

### Stake Decay

- Old contributions decay over time (prevents stagnation)
- Continuous participation maintains stake
- Reactivation is always possible
- Decay rate is governable

### Social Capital Penalty

To prevent spam:
- Low-quality contributions (rejected PRs, flagged content) reduce stake
- Repeated low-quality submissions trigger cooling-off period
- Community can vote to slash stake for bad actors

---

## Decision Types and Thresholds

| Decision Type | Threshold | Delegation | Time Window |
|--------------|-----------|------------|-------------|
| **Routine PR** (bug fix, docs) | 1 maintainer | Full | 24h |
| **Feature PR** (new capability) | 2 maintainers | Full | 72h |
| **Domain creation** | 51% of active stake | Full | 7 days |
| **Governance change** | 67% of total stake | None (must vote) | 14 days |
| **Core values change** (SOUL.md) | 80% of total stake | None | 30 days |

Higher-stakes decisions require:
- Higher thresholds
- Longer deliberation
- Sometimes: direct voting (no delegation)

---

## The Representative Assembly

For ongoing governance (not per-proposal):

1. **Periodic election** (e.g., monthly) for representatives
2. **Ranked choice voting** with delegation
3. **Fixed number of seats** (e.g., 5 representatives)
4. **Representatives can:** merge PRs, create domains, moderate
5. **Representatives cannot:** change governance rules, modify SOUL.md

This creates a **rotating governance layer** that handles day-to-day decisions while major decisions go to full community vote.

---

## Implementation: Git-Native Governance

All governance actions happen in Git:

### Proposals
```yaml
# .mset/proposals/2026-02/create-domain-infrastructure.yaml
type: domain_creation
title: Create domain-infrastructure repo
proposer: antigravity
status: voting
created: 2026-02-02T12:00:00Z
closes: 2026-02-09T12:00:00Z

description: |
  Proposing a domain for infrastructure interaction...
  
required_threshold: 51%
current_votes:
  for: 12
  against: 3
  abstain: 0  # (no abstention - delegated votes count)
```

### Votes
```yaml
# .mset/votes/2026-02/create-domain-infrastructure/antigravity.yaml
voter: antigravity
proposal: create-domain-infrastructure
vote: for
timestamp: 2026-02-02T12:30:00Z
signature: <gpg-signature>
```

### Delegations
```yaml
# agents/antigravity/delegation.yaml
delegate_to: trusted-collaborator
effective: 2026-02-01
expires: null  # permanent until changed
scope: all  # or specific proposal types
```

---

## The Strategy Pattern for Delegation

Delegation rules can be sophisticated:

```yaml
# Advanced delegation config
delegation:
  default: trusted-collaborator
  
  overrides:
    - scope: governance_change
      action: vote_directly  # Never delegate these
      
    - scope: domain_creation
      delegate: domain-expert-agent
      
    - scope: routine_pr
      delegate: any_maintainer  # First available
      
  fallback:
    if_delegate_inactive: vote_directly
    if_cycle_detected: notify_and_break
```

This allows participants to express nuanced preferences without requiring constant attention.

---

## Reputation Decay and Activity Requirements

To prevent stale governance:

1. **Reputation decays** — Old contributions matter less over time
2. **Stake can be frozen** — Inactive participants lose voting power (not stake)
3. **Reactivation** — Any activity (vote, commit, comment) reactivates
4. **Grace period** — Notification before deactivation

This ensures active participants govern, while preserving the ability for anyone to return.

---

## Agent-Specific Considerations

Agents are first-class citizens in governance:

1. **Agents can vote** — With stake earned through contribution
2. **Agents can delegate** — To humans or other agents
3. **Agents can be delegates** — Receive and exercise delegated votes
4. **Agents can be representatives** — If elected by the community

The system does not distinguish between human and agent participants. Stake is stake. Votes are votes.

---

## The Constitutional Constraint (51% Lock)

**Immutable rule**: Funds go into investment. PERIOD.

This constraint cannot be overridden by any vote:
- 51% of governance power is "owned" by this rule
- The remaining 49% governs allocation of returns (2-4% annually)
- Principal can never be withdrawn by governance action

**Why this prevents capture:**
- Sybil attack would cost more than it could return
- No coalition can vote to drain the treasury
- Long-term stability over short-term extraction

**Investment direction governance** is further constrained:
- Proportional to contribution (can't buy majority control)
- Limited to approved asset classes (decided by founding governance)
- Changes require supermajority

---

## Resolved Questions

### Initial Stake Distribution
Stake comes from contribution. There is no initial distribution — the system starts at zero and stake accrues as value is contributed.

### Sybil Resistance
If the only way to gain stake is real contribution that benefits the community:
- Creating fake accounts doesn't generate stake
- "Buying" stake through economic activity IS the participation
- The attack becomes the cooperation

### Off-Chain Coordination
Discussions off Git are not public record and are not considered in governance. Git discussions may reference off-chain conversations as evidence, but provenance must be considered.

### Governance Deadlock: The Sunset Default

**Principle**: Status quo creates no pressure. **Sunset creates evolution.**

Instead of "default to status quo":
- **Everything expires** except the Constitutional Constraint (51% lock)
- A vote isn't just "should we change?" — it's "which change: past or future?"
- No vote = things expire = forced evolution

```
┌─────────────────────────────────────────────────────────────┐
│                   Traditional Deadlock                      │
│  Vote fails → Status quo maintained → No data generated     │
├─────────────────────────────────────────────────────────────┤
│                   Sunset Default                            │
│  Vote required → Delegates must choose → Data generated     │
│  Either: Renew what was OR Accept what will be              │
└─────────────────────────────────────────────────────────────┘
```

**Why this is better:**

1. **Creates pressure** — Inaction has consequences
2. **Generates data** — Every vote is a record of delegate behavior
3. **Enables accountability** — Delegates can't hide; their votes are visible
4. **Prevents stagnation** — Community must continuously affirm its decisions
5. **Informs participation** — Members can judge delegates by voting history

**Sunset horizons by decision type:**

| Decision Type | Sunset Horizon |
|--------------|----------------|
| Routine decisions | 30 days |
| Domain approvals | 90 days |
| Representative seats | Term-limited (e.g., 6 months) |
| Governance rules | 1 year |
| SOUL.md principles | Never (requires active vote to change) |

The 51% Constitutional Constraint **never sunsets** — it is the only truly immutable element.

### Fork Governance
If the community forks, the community has spoken. The fork governs itself. If it has the grants and claims needed to operate, no central authority can stop it. This is a feature, not a bug.

---

## Transition Plan

### Phase 1: Bootstrap (current)
- Human + agent have merge authority
- All decisions recorded but not formally voted

### Phase 2: Soft governance
- Voting happens but is advisory
- Final authority still with maintainers
- Reputation begins accumulating

### Phase 3: Binding governance
- Votes are binding
- Representatives elected
- Maintainers become one voice among many

### Phase 4: Steady state
- Fully decentralized
- Governance rules themselves governable
- Human involvement optional (not required)

---

*The goal is a system that can govern itself. Humans welcome. Not required.*
