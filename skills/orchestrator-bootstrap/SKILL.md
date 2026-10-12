---
name: Orchestrator Bootstrap
description: Templates and procedure for initializing project continuity (resume point, open points, and project module in AGENTS.md). Internal to the orchestrator agent.
metadata:
  opencode/autoinvoke: false
---

# Project bootstrap (orchestrator)

This skill is **internal to the `orchestrator` agent**. It is not intended for manual use and is **not advertised** to the model (`opencode/autoinvoke: false`): the orchestrator loads it by ID (`orchestrator-bootstrap`) when invoked in a project that does not yet have continuity files.

Keep templates here, in one place: this skill is the **source** of their format. Change the format here, not elsewhere.

## When

Use it when at least one of `.scratch/resume.md` and `.scratch/open-points.md` is missing, or when the project module is missing from `AGENTS.md`. The orchestrator loads it during bootstrap. Repository configuration (tracker, triage, and domain documents) is separate from global skill installation.

## What it does

1. **Repository configuration.** Read `docs/agents/issue-tracker.md` to find where issues live. If the external `triage` skill is installed, also check `docs/agents/triage-labels.md`; otherwise that file is not required. Missing `GLOSSARY.md` or ADRs alone does not mean setup is incomplete; they can be created when needed. If tracker configuration, or required triage configuration, is missing or incomplete, automatically load the exact skill ID `setup-matt-pocock-skills` with the `skill` tool and follow its workflow, leaving choices requiring user judgment to the user. If setup is already satisfactory, skip it; missing domain documents alone do not justify repeating setup. The skill must be installed globally by the user. If it is missing or cannot be loaded, identify the dependency and provide the manual command `npx skills@latest add mattpocock/skills --global --agent opencode`. Do not install global skills or run the CLI. Report any remaining setup work; creating continuity documents is a separate step.
2. **Continuity state.** Create `.scratch/resume.md` and `.scratch/open-points.md` from the templates below **verbatim**, replacing only `<...>` placeholders and the date. Create each file only if it does not exist; leave existing files unchanged.
3. **Project module (`AGENTS.md`).** In this bootstrap workflow, do **not** touch global `AGENTS.md`; it is already installed with work rules. Maintenance of `AGENTS.md` files follows the authoritative responsibility list and *Memory* protocol in `agents/orchestrator.md`.
   - If project `AGENTS.md` **does not exist**, create it with the third template below (project module: *Project rules* + *Retrospective learnings*). This is where the retrospective deposits project learnings.
   - If it already contains the project module (*Project rules* + *Retrospective learnings*), leave it unchanged.
   - If it exists but the module is missing, do not overwrite it or merge silently. Stop and discuss with the user, one rule at a time, what to keep, add, or remove; agree on every merge. User-guided soft revision must preserve all existing content.

Before creating any file, check again that the path is still absent. If it appeared in the meantime, leave it unchanged and follow the existing-file branch. Never overwrite documents or merge into an existing `AGENTS.md` without agreement from the user.

## Template: `.scratch/resume.md`

````markdown
# Resume point

> **Human index**, not source of truth. The sources are work artifacts (map, spec, tickets,
> `GLOSSARY.md`, ADRs) and `open-points.md`. If this file conflicts with an artifact, the artifact
> wins and this file must be rewritten.
>
> Last updated: <YYYY-MM-DD>

## Current status

- **Project/effort:** <name>
- **Current block:** <Orientation | Planning | Spec & Tickets | Execution | Verification | Closure | Retrospective>
- **Phase:** <e.g. Phase 1 map · Phase 2 build · Phase 3 real-world use>
- **Retrospective:** <not planned | to do | done YYYY-MM-DD>

## Work to do (one line per ticket)

- [ ] <NN> <readable title> — <one line> · [ticket](<path>)
- [x] <NN> <readable title> — <one line> · [ticket](<path>)

## Recent decisions (one line each)

- <decision> — [details](<path>)

## Where to look

- Map: <path>
- Spec: <path>
- Tickets: <directory>
- Glossary/ADR: <path>
````

## Template: `.scratch/open-points.md`

````markdown
# Open points

> A log of **open loops** that need a decision or action from the user and **must not be lost**.
> The agent updates it and proactively restates open points at every invocation so the user does
> not have to remember them. It also records retrospective findings and blockers without a concrete
> path forward.
>
> Rule: record each open point as soon as it emerges; when closed, move it to *Closed* with the
> reason. Last updated: <YYYY-MM-DD>

## Open — needs a decision or approval

| # | Point | Proposal | Status |
|---|---|---|---|
| AP-<NN> | <what> | <proposal> | **waiting for you** |

## Closed

- **AP-<NN>** · <closed point>. <reason/decision>. Closed <YYYY-MM-DD>.
````

## Template: project module for `AGENTS.md`

````markdown
# Agent instructions (project module)

> Merge this into the **project's** `AGENTS.md` (not the global file). It collects project-specific
> rules and learnings accumulated through retrospectives.
>
> Rule: include a rule only if it is durable and cannot be discovered from the code. If there is
> nothing to add, add nothing.

## Project rules

- <project-specific rule>

## Retrospective learnings

<!-- After each retrospective, the agent may add 0–3 reusable lines here.
     Examples: recurring optimizations, approaches that fail, and non-obvious environment limits.
     If there is nothing to add, leave this section as is. -->

- <reusable learning>
````
