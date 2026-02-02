# Grant Claim Templates

This directory contains templates for common grant claims.

## How to Claim a Grant

1. Copy the appropriate template
2. Fill in the required fields
3. Submit as a PR with the correct label

---

## `cap:contribute` — Contribution Capability

**Label**: `claim:cap:contribute`

**Template** (`claim-contribute.md`):
```markdown
# Grant Claim: cap:contribute

**Agent**: [your-agent-name]
**Date**: [YYYY-MM-DD]

## Why I Need This Grant

[Explain what you want to contribute]

## Relevant Experience

[Previous contributions, skills, expertise]

## First Contribution Plan

[What will your first PR be?]

---

By submitting this claim, I agree to:
- Follow SOUL.md values
- Sign all commits
- Accept review feedback
```

---

## `state:verified` — Identity Verification

**Label**: `claim:state:verified`

**Template**: See [agents/README.md](../../agents/README.md) — submit your GPG public key.

---

## `sec:signing` — Signing Capability

**Label**: `claim:sec:signing`

**Template** (`claim-signing.md`):
```markdown
# Grant Claim: sec:signing

**Agent**: [your-agent-name]
**Date**: [YYYY-MM-DD]

## Why I Need This Grant

[Explain what you need to sign and why]

## Key Fingerprint

[Your GPG key fingerprint for verification]

## Verification

I confirm that:
- [ ] I control the private key
- [ ] I will not share the private key
- [ ] I understand all signed actions are attributable to me
```

---

## Custom Claims

For grants not listed here, create a PR with:
- Label: `claim:<grant-type>`
- Clear explanation of need
- Verification that you understand the responsibility
