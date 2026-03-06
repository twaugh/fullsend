# Code Review

How do agents review code effectively, including catching security issues, before and after PR submission?

## Why code review is the hardest problem

Code generation is largely solved — given a well-scoped task, modern agents produce working implementations reliably. But reviewing code is fundamentally different:

- **Generation is convergent** — there's a clear goal (make the tests pass, implement the spec)
- **Review is divergent** — you're looking for problems you don't know exist yet
- Review requires understanding not just what the code does, but what it *should* do (intent)
- Review requires understanding what the code *doesn't* do (missing error handling, edge cases, security implications)
- Review requires context that may not be in the diff (how does this interact with other systems?)

## Two phases of review

### Pre-PR review (internal review)

Before an implementation agent even opens a PR, can we catch problems early?

- Does the implementation match the intent of the issue?
- Are there obvious bugs, missing edge cases, or security issues?
- Does it follow the repo's patterns and conventions?
- Are the tests adequate?

This is potentially more impactful than post-PR review because it catches problems before they consume reviewer attention.

### Post-PR review (formal review)

The PR is open. Review agents evaluate it.

- Correctness: does it do what it claims?
- Security: does it introduce vulnerabilities?
- Intent alignment: does it address the right problem?
- Test coverage: are the changes tested?
- Style/conventions: does it follow the repo's patterns?

## Security-focused sub-agents

Given Konflux's dual security responsibility (protecting the platform AND protecting user content), code review should include specialized security perspectives:

### Platform security agent

Reviews changes for threats to Konflux itself:
- RBAC and authorization changes
- Authentication flows
- Data exposure risks
- Privilege escalation paths

### Content security agent

Reviews changes that affect the CI/CD content passing through Konflux:
- Pipeline definition handling — can a user's pipeline definition escape its sandbox?
- Build configuration — can build parameters be manipulated?
- Release policy — can release gates be bypassed?
- Artifact integrity — can artifacts be tampered with?

### Injection defense agent

Specifically looks for prompt injection patterns in:
- PR descriptions and commit messages
- Code comments
- String literals
- Configuration files
- Test data

## The confidence problem

A human reviewer can say "I'm not sure about this, let me think" or "I need someone else to look at this." Agents need equivalent mechanisms:

- When should a review agent escalate to a human?
- How does an agent express uncertainty? Confidence scores? Explicit "I don't know" signals?
- Should there be a minimum number of agent reviewers that agree before auto-merge?

## Open questions

- Can we quantify review quality? How do we know if an agent's review is as good as a human's?
- How do we handle the case where an agent approves a PR that a human would have caught? (Learning from mistakes.)
- Should review agents have access to the full repo context, or just the diff? Full context is more accurate but more expensive and more vulnerable to injection from existing code.
- How do we prevent review agents from being "rubber stamps" — always approving because they're optimizing for throughput?
- What's the right interface for review feedback? GitHub PR comments? A structured report? Both?
- How do we handle multi-repo changes where the review needs to consider changes across repos together?
