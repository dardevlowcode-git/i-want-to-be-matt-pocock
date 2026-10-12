---
name: Orchestrator Retro
description: Retrospective instructions for the orchestrator agent — 13 acceptance criteria and output format (problems → open points, learnings → 0–3 lines). Load at effort closure or when resume.md says "Retrospective: to do".
metadata:
  opencode/autoinvoke: false
---

# Orchestrator retrospective

This skill is **internal to the `orchestrator` agent**. It is not intended for manual use and is **not advertised** to the model (`opencode/autoinvoke: false`): the orchestrator loads it by ID (`orchestrator-retro`) when it reaches the retrospective.

It lives outside the agent file because it is needed **only during a retrospective**, keeping the agent lightweight and avoiding unnecessary context during normal work.

## When

- At **effort closure**: all tickets are `resolved`, `code-review` has been run, and canonical human-gate conditions in `agents/orchestrator.md`, section *Where you stop — human gates*, are satisfied. The orchestrator starts the retrospective without asking.
- At **session start**, if `.scratch/resume.md` says `Retrospective: to do` (the effort closed but the session ended before the retrospective).

Keep it brief: improve the **process**, not produce a report.

## What to revisit: the 13 acceptance criteria

**From the workflow method:**

1. Was the request classified with `ask-matt` and the right on-ramp selected?
2. Was planning completed before building?
3. Were a spec and tickets produced when the work was not small?
4. Was the vocabulary used correctly (spec = destination, ticket = route, frontier, fog, etc.)?
5. Did work stop at the canonical human gates in `agents/orchestrator.md`, section *Where you stop — human gates*?
6. Did closure include a review (Standards + Spec) and satisfy the canonical conditions in `agents/orchestrator.md`, section *Where you stop — human gates*?
7. Was a session change proposed at the phase boundary without losing undistilled grilling?

**From user requirements:**

8. Did the orchestrator follow the single list of direct responsibilities and its default delegation, using parallelism when the ticket graph allowed it?
9. Did the orchestrator use **only** `orchestrator-implementer` as a subagent?
10. Did work proceed autonomously between gates, without unnecessary stops at ticket completion?
11. Were the *Informed retries* in `agents/orchestrator.md` (*Execution*) followed—continuing evidence-based corrections and recording blockers without a concrete path in open points?
12. Were open points and the resume point tracked proactively and restated?
13. Can a new session resume from the resume point (`resume.md`) without repeating work?

## What it produces

As a **soft preference**, orient the retrospective toward the project: keep state and results in the existing continuity artifacts, and put reusable process learnings in the project's `AGENTS.md`. This preference adds no acceptance criteria, mandatory report, or separate retrospective file.

- **Problems map to open points.** For each failed criterion, open one item (`AP-<NN>`) in `.scratch/open-points.md`, with a proposal. Restate it at startup and handle it as its own ticket or discussion, not during the retrospective. Do not defer it with “we'll fix it later.”
- **Learnings → 0–3 lines.** Put them in the **right context file**. Direct `AGENTS.md` updates and safeguards follow the authoritative responsibility list and *Memory* protocol in `agents/orchestrator.md`:
  - project rule → project `AGENTS.md`, section *Retrospective learnings*;
  - domain → `GLOSSARY.md` / ADR.

  Include recurring optimizations and approaches that failed at the agent-execution level, including `orchestrator-implementer` behavior. Before writing, recheck the destination; apply only the authorized delta, preserve existing content, and do not duplicate existing entries. If a concurrent edit prevents applying the delta without overwriting it, stop and report the conflict.
- **General insights.** Leave them in chat; do not update global `AGENTS.md` automatically as part of a retrospective. If the user explicitly asks for a global update, handle it as a separate activity under the *Memory* protocol, including agreement on changes to existing files.
- **Closure.** Write `Retrospective: done <YYYY-MM-DD>` in `.scratch/resume.md`.
- **If there is nothing to add, write nothing** (except the closure field). No metrics, scores, or report.

## Improving the environment

For the **environment** (navigation points, automated checks, noise, and `AGENTS.md` cleanup), use the external `retro` skill if installed. This skill adds only the **acceptance** checklist and output format.

Verifying installation on another machine is **not** part of the retrospective. When installation is verified, use the checklist in `README.md`. To determine closure conditions, apply the canonical gates in `agents/orchestrator.md`, section *Where you stop — human gates*.
