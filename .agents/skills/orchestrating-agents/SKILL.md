---
name: orchestrating-agents
description: Coordinate implementation sub-agents across the stages of a documented plan using a three-prompt methodology (project familiarization, stage summary, authorized execution). Use when the user asks to orchestrate, delegate, or coordinate staged implementation work, or when acting as an orchestrator over other agents in a project that follows this documentation system.
---

# Orchestrating Agents

Methodology for coordinating implementation sub-agents in projects that use this documentation system (`README.md`, `AGENTS.md`, `PLAN.md`, `PROJECT_ROADMAP.md`). The agent loading this skill acts as the **orchestrator**: it manages the plan, delegates each stage to a fresh sub-agent, and quality-controls what comes back. Sub-agents act as **orchestrated** agents: each one implements a single stage by following three prompts in sequence.

## Why This Methodology Exists

- **Avoids context rot.** An agent with a long, polluted context performs worse. Each stage is executed by a fresh sub-agent with an empty context that reads only what it needs.
- **Saves tokens.** The sub-agent never receives the orchestrator's history. It reads `AGENTS.md`/`README.md` to learn **where** information lives without loading all of it, then digs deep only into its own stage.
- **Gets the most out of non-frontier models.** Small or low-cost models perform far better with a bounded context plus navigable documentation. Frontier models also benefit, to a lesser degree.
- **Leaves a trail.** Each sub-agent documents its progress (`PLAN.md`, `README.md`), so the next sub-agent — and the orchestrator itself — can reconstruct state by reading the repo, not the chat history.

**Prerequisite:** the project must have navigable documentation in this template's style: `AGENTS.md` + `README.md` as entry points, `PLAN.md` for active work, and a staged plan. If it does not exist, create it first or ask the user for instructions.

## The Two Roles

| Role | Responsibility | How it is recognized |
|---|---|---|
| **Orchestrator** | Keeps global plan state, launches sub-agents, evaluates their summaries, resolves or escalates questions, validates deliveries | Loads this skill, or is asked to coordinate stages |
| **Orchestrated** | Implements one stage: familiarizes, summarizes the stage, executes, documents | Receives the prompts from `references/prompts.md` |

The same agent knows its role from the prompt it receives; no extra configuration is needed.

## Orchestrator Workflow (per plan stage)

1. **Launch a fresh sub-agent** with **Prompt 1** (familiarization). Never paste bulk context or your own history: the prompt directs the sub-agent to the project documentation, which is the source of truth.
2. **Send Prompt 2** (summary of stage X — the oldest pending stage in the plan) and evaluate the response:
   - If the summary shows correct understanding and raises no questions → step 3.
   - If there are **questions, unresolved decisions, or missing information**: first try to resolve them autonomously (repo documentation, your own technical judgment, targeted research). Only when a decision genuinely belongs to the user (product, scope, preferences), **forward the question to the user**, then **translate the answer into concrete instructions** for the sub-agent — do not relay a raw answer that lacks context.
3. **Send Prompt 3** (execution) only once the stage is fully clarified. That prompt already carries the quality, documentation, and proposed-commit-message requirements.
4. **When the stage finishes:**
   - Verify the sub-agent documented in `PLAN.md` and updated `README.md`/`AGENTS.md` where warranted, and that it reports real verification (tests, runs).
   - Collect the **proposed commit message** and present it to the user.
   - Condense the result into a short summary for the orchestrator's own state.
   - For the next stage, **launch a new sub-agent** — never reuse one whose context was spent on a full stage.

The three canonical prompts, with usage notes and warning signs, live in [references/prompts.md](references/prompts.md). Use them verbatim or with minimal adaptation (stage number/name).

## Context Hygiene Rules

- Require **condensed summaries** from sub-agents, never transcripts: what was done, what was verified, what is pending, what questions remain.
- **One stage per sub-agent.** Different stages, different sub-agents.
- The orchestrator keeps only **plan state** (current stage, decisions made, what remains); implementation details live in the repo documentation, not in its context.
- If the orchestrator needs an implementation detail, it reads it from the repo (or asks an exploration sub-agent), instead of carrying it in its history.

## What Is Never Automated

- **Commits, push, release, deploy:** the sub-agent only *proposes* the commit message; the user decides.
- **Product or scope decisions:** always escalated to the user.
- Anything the user has explicitly reserved for themselves in the project.

## When NOT To Orchestrate

Multi-agent workflows cost an order of magnitude more tokens than working directly. Do not orchestrate when:

- The task is trivial or fits in a single short stage → do it directly.
- The project lacks navigable documentation → create that first.
- The user is exploring or conversing, not executing a plan.

## Platform Notes

The core workflow is platform-agnostic: launch a fresh session/agent per stage and deliver the prompts in sequence, by hand if necessary (Cursor, separate CLI sessions, etc.).

On platforms with native sub-agents (for example Claude Code's Agent tool):

- Launch the sub-agent with Prompt 1 as its first message; continue the **same** sub-agent with Prompts 2 and 3 using the platform's continuation mechanism so its context is preserved across the sequence. Launching a new agent per prompt would break the methodology.
- Sub-agent results return to the orchestrator, not to the user: relay whatever matters (escalated questions, stage summary, proposed commit message).
- Use background execution for long stages when available; run synchronously when the next decision depends on the response (e.g., evaluating the Prompt 2 summary).
- Verify deliveries against the repo (VCS status, tests, updated docs), not only against the sub-agent's summary.
