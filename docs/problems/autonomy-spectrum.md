# Autonomy Spectrum

When should agents auto-merge, and when should they escalate to humans?

## The model: binary with CODEOWNERS

The autonomy model is **binary per-repo** with **CODEOWNERS as the escape hatch**:

- A repo is either "agent-autonomous" or it isn't
- Within an autonomous repo, specific file paths can require human approval via CODEOWNERS
- Repos graduate to autonomous mode once they meet readiness criteria

This avoids the complexity of fine-grained per-change-type rules while still protecting critical paths.

## CODEOWNERS as the control mechanism

CODEOWNERS is already a well-understood GitHub mechanism. In the agentic context, it means:

- **Agent-owned paths** — agents can review and merge without human approval
- **Human-owned paths** — changes here always require human approval, regardless of what the agent thinks

### Likely candidates for human-owned paths

- Security policies and RBAC configuration
- API surface (CRD schemas, REST endpoints, protobuf definitions)
- Deployment manifests and release configuration
- Agent configuration and system prompts (critical — agents must not modify their own guardrails)
- **CODEOWNERS files themselves** — always human-owned, never agent-modifiable. This is a hard rule, not a suggestion. If agents could modify CODEOWNERS, they could remove their own guardrails.
- Cross-repo interface contracts
- UX-facing components

### How CODEOWNERS interacts with agents

When a PR touches both agent-owned and human-owned paths:
- The human-owned paths block auto-merge
- The agent can still review the entire PR and provide feedback
- The human approves the guarded paths; the agent's review covers the rest

## Graduation criteria

What does a repo need before agents can be trusted to merge autonomously?

Possible criteria (all TBD — this needs experimentation):

- Minimum test coverage threshold (what number? per-package or overall?)
- CI pipeline includes integration/e2e tests, not just unit tests
- Linting and formatting enforced in CI
- CODEOWNERS file covers all security-sensitive paths
- History of successful agent-reviewed PRs (agents review but don't merge, humans validate the agent's judgment)
- No recent security incidents attributable to missed review

## The probationary period

Before flipping a repo to full autonomy, run agents in "shadow mode":

1. Agents review PRs and produce recommendations
2. Humans still approve and merge
3. Compare agent decisions to human decisions over time
4. When confidence in alignment is high, graduate to autonomous mode

This builds trust incrementally and provides data on agent reliability.

## Open questions

- Who decides when a repo is ready for autonomy? (See [governance.md](governance.md))
- Can autonomy be revoked? Under what circumstances? Automatically if a bad merge is detected?
- How do we handle repos with poor test coverage today? Do agents help improve coverage as a prerequisite to their own autonomy?
- Is per-repo binary too coarse? Should there be sub-repo zones of autonomy beyond what CODEOWNERS provides?
- What about cross-repo changes? If a change spans an autonomous repo and a non-autonomous one, which rules apply?
- How do we handle the CODEOWNERS bootstrap — who decides the initial set of guarded paths?
