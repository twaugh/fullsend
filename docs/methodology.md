# Methodology

How this exploration progresses. Not a rigid process — a description of the phases we expect to pass through and what characterizes each one.

## Phase 1: Divergent exploration (current)

We are here. The goal is to expand the problem space — identify as many distinct problem areas as possible, articulate them clearly, and resist the temptation to converge on solutions too early.

Each problem gets its own document in `docs/problems/`. Documents should present the problem, its dimensions, its relationship to other problems, and its open questions. They should not prescribe solutions. The point is to understand the terrain before choosing a path through it.

The right instinct in this phase is "what are we missing?" not "how do we solve this?"

## Phase 2: Solution modeling

For each problem, develop multiple possible solution models. Not one answer — several, with trade-offs made explicit. Each solution model may have multiple implementation options.

Where possible, devise experiments to build confidence in a solution. Experiments should test both the positive case (does this approach work when things go right?) and the negative case (does it fail safely when things go wrong? does it catch the thing it's supposed to catch?). Confidence comes from seeing a solution hold up under both conditions.

Experiments are documented in `docs/experiments/`. The existing experiment format may evolve, but the core discipline is: state what you expect to learn, describe what you did, and record what actually happened.

Not every solution can be experimentally validated — some are organizational or procedural. For those, the trade-off analysis in the problem document is the primary tool.

## Phase 3: Convergence

The path from validated solutions to adoption depends on how we resolve the [governance](problems/governance.md) problem. Governance determines who has authority to make binding decisions, how the community participates in those decisions, and what process turns an explored solution into an adopted one.

Until governance is resolved, phases 1 and 2 can proceed independently — we can explore problems and test solutions without yet deciding how decisions get made. But convergence requires it.

What phase 3 looks like is deliberately left open.
