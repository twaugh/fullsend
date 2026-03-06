# Vision

## The goal

Every repo in the [konflux-ci](https://github.com/konflux-ci/) GitHub org operates with fully autonomous agents handling the routine software development lifecycle: issue triage, implementation, code review, testing, and merge-to-production. Humans participate at two points:

1. **Strategic intent** — defining what the system should do and become. Features, architecture, direction.
2. **Guarded paths** — CODEOWNERS-enforced human approval for security-critical, API-changing, or architecturally significant code paths.

Everything else is autonomous.

## Why

Modern coding agents have largely solved the code generation problem. Given a well-scoped task and a codebase with decent tests, agents can produce working implementations reliably. But generation is only one piece of the development lifecycle. The hard unsolved problems are:

- **Code review** — including internal review before a PR is even submitted
- **Intent verification** — how does the system know a change is one we actually want?
- **Priority and backlog management** — what should be worked on next?
- **Authority and governance** — who decides what agents can do?
- **Security** — how do we prevent the autonomous system from being exploited?

This project exists to explore these problems for konflux-ci specifically, though the patterns may generalize.

## Why konflux-ci specifically

Konflux is a CI/CD system that provides security features to its users. This means:

- The system itself must be secure against threats (prompt injection, insider attacks, agent drift)
- The CI/CD content passing *through* the system must also be protected — agents reviewing pipeline definitions and build configurations need a security mindset
- This dual security requirement makes it a harder and more interesting proving ground than a typical application

## Principles

- **Security is not a layer — it's the foundation.** Every component of the agentic system must be designed with adversarial thinking from day one. Not bolted on after.
- **Autonomy is earned, not granted.** Repos and change types graduate to higher autonomy levels based on demonstrated safety — test coverage, agent track record, security posture.
- **Humans set direction, agents execute.** The system should amplify human judgment, not replace it for strategic decisions.
- **Transparency over trust.** Every agent action should be auditable. Every decision should be traceable to its inputs.
- **Start anywhere, learn everywhere.** Experimentation across different repos and approaches is expected. What works for a Go controller may not work for a Tekton pipeline definition.

## Inspiration

OpenAI's [Harness engineering](https://openai.com/index/harness-engineering/) work demonstrates agents handling engineering tasks end-to-end with sandboxed execution and automated testing as primary guardrails. Our technology stack and constraints are different, but the ambition is similar.

## What this is not

- Not a product spec. This is an exploration.
- Not prescriptive. Multiple solutions may coexist for different problem areas.
- Not finished. This is meant to evolve through community contribution and experimentation.
