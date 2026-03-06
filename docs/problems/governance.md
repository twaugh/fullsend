# Governance

Who controls the agents, their configuration, and their authority?

## The core tension

The agentic system's configuration is itself a security-critical attack surface. If someone can modify what agents are allowed to do, they can effectively bypass all other controls. But the configuration also needs to evolve as we learn and as repos mature.

## Open questions (this is the least developed problem area)

### Decision authority

- Who decides to turn on agent autonomy for a given repo?
- Who sets the CODEOWNERS boundaries that define human-required paths?
- Who can modify the agent's rules, system prompts, and policies?
- Is this centralized (a small team), federated (each repo's maintainers), or something else?

### Configuration security

- Agent configuration must be protected from modification through the same channels agents operate on (PRs, issues, comments)
- Changes to agent policy should require a higher level of approval than changes to code
- How do we audit changes to agent configuration?
- Should agent policy be stored in the repos it governs, or in a separate policy repo?

### Accountability

- When an agent makes a bad decision, who is responsible?
- How do we trace an agent action back to the policy that authorized it?
- What's the escalation path when something goes wrong?

### Org-wide vs. per-repo policy

- Should there be org-wide guardrails that individual repos can't override?
- Can individual repos add stricter controls but not loosen org-wide ones?
- How do we handle repos with different maturity levels?

### Community input

- How does the broader konflux-ci community participate in governance decisions?
- Is there a process for proposing changes to agent policy?
- How do we balance speed of experimentation with community consensus?

## Possible models

These are sketches, not proposals. Each needs significant development.

### Centralized control

A small team manages all agent policy. Repos can request autonomy, but the central team decides.

### Federated with guardrails

Org-wide minimum standards (security requirements, audit requirements). Individual repo maintainers decide their own autonomy level within those bounds.

### Progressive delegation

Start centralized. As the system proves itself and governance patterns emerge, delegate more control to repo maintainers.
