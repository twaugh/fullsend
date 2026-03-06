# Intent Representation

How do we capture, verify, and enforce what changes are wanted — so that agents can distinguish a desired change from a non-feature?

## Why this is hard

Today, intent lives in human judgment. A product manager says "we need feature X." A lead engineer evaluates whether a PR aligns with that direction. This judgment is contextual, nuanced, and hard to formalize.

In an autonomous system, agents need a way to answer: "Is this change something we actually want?" at review time. Without this, agents might:

- Implement changes that are technically correct but strategically wrong
- Merge features nobody asked for
- Optimize for the wrong thing (e.g., adding complexity where simplicity was needed)

## The spectrum of intent

Not all changes require the same level of intent verification:

| Change type | Intent signal needed | Example |
|---|---|---|
| Bug fix (clear reproduction) | Low — the bug report itself is the intent | "Fix nil pointer in controller when X is nil" |
| Small improvement | Low-moderate — linked issue sufficient | "Add retry logic for flaky API call" |
| New feature | High — needs explicit authorization | "Add support for multi-arch builds" |
| API change | High — affects consumers | "Change Snapshot CRD schema" |
| UX change | High — affects users | "Redesign pipeline visualization" |
| Architectural change | Very high — affects multiple repos | "Migrate from X to Y across the org" |
| New component | Very high — creates ongoing maintenance | "Add a new notification service" |

## Possible approaches

### 1. Intent lives in issues/specs

Human-authored GitHub issues or design documents are the source of truth. Agents can only work on explicitly created issues. A reviewing agent checks a PR against the linked issue's acceptance criteria.

**Pros:** Simple, familiar, low overhead for small changes. Humans retain clear control over what gets worked on.

**Cons:** Issues are unstructured — hard for agents to evaluate programmatically. Acceptance criteria quality varies wildly. Doesn't scale to the strategic level (how does an agent know if a feature aligns with product direction?).

### 2. Structured machine-readable backlog

Something more formal than GitHub issues — a priority list with structured acceptance criteria, tagged by change type and scope, that agents can evaluate against programmatically.

**Pros:** Agents can make reliable decisions. Clear traceability. Supports prioritization.

**Cons:** Overhead of maintaining the structured backlog. Risk of the backlog becoming stale or disconnected from reality. Someone still has to author and maintain it.

### 3. Declarative intent in the codebase

Repos contain descriptions of desired behavior — like ADRs, or an `INTENT.md` that describes what the system should and shouldn't do. Agents evaluate changes against these declarations.

**Pros:** Intent travels with the code. Version-controlled. Agents can read it at review time.

**Cons:** Hard to keep current. Doesn't capture strategic/cross-repo intent well. Easy to game (an attacker could modify the intent document as part of a malicious PR).

### 4. Layered intent

Different layers for different scopes:

- **Tactical** (bug reports, small improvements) — a linked issue is sufficient
- **Strategic** (features, API changes, architecture) — requires a design doc or RFC with explicit human approval
- **Cross-cutting** (security policies, UX consistency, coding standards) — standing rules that apply to all changes, maintained centrally

**Pros:** Matches the natural spectrum of change significance. Local autonomy for small things, broader approval for big things.

**Cons:** Complexity of maintaining multiple layers. Boundary between layers is fuzzy — who decides if something is "tactical" vs. "strategic"?

## Open questions

- How do agents classify a change's scope/significance reliably? Can this be automated, or does a human need to label it?
- How do we handle emergent changes — a "small bug fix" that reveals a deeper architectural issue?
- Can intent be composed? If three small changes together constitute a feature, who notices?
- How do we prevent intent documents from becoming stale?
- What's the relationship between intent and CODEOWNERS? Are guarded paths a proxy for "this area requires strategic intent"?
- Cross-repo intent: when a change spans multiple repos (e.g., a CRD change that affects both the controller and the UI), how is intent represented and tracked?
