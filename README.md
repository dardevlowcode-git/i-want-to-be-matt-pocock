# I want to be Matt Pocock

> **Version 0.12.0** · Last updated: **2026-10-12**

A portable OpenCode package that guides a project workflow from idea to delivery by invoking external workflow skills and maintaining project continuity. The `orchestrator` agent coordinates the workflow and delegates by default to `orchestrator-implementer`, while handling only the activities in its authoritative responsibility list and respecting its limits and gates.

> **Unofficial.** This is a personal project, not affiliated with or endorsed by Matt Pocock. The method and skills are his; this repository packages only the orchestrator. See `NOTICE`.

## Package contents

| File | Purpose |
|---|---|
| `agents/orchestrator.md` | Primary agent: workflow routing, continuity stewardship, and dispatch. Its authoritative list includes direct `AGENTS.md` updates and ordinary Git publication; it delegates other work to `orchestrator-implementer`. |
| `agents/orchestrator-implementer.md` | Subagent for one task at a time: ticket with `tdd`, merge, one review axis, exploration, or research. Choose a model in the installed copy. |
| `skills/orchestrator-bootstrap/SKILL.md` | Internal skill to initialize a project and create continuity templates. |
| `skills/orchestrator-retro/SKILL.md` | Internal retrospective skill: 13 criteria, output format, and a soft preference for documenting learnings in the project. |
| `skills/orchestrator-runtime-diagnostics/SKILL.md` | Internal skill for read-only runtime loading diagnostics, only on explicit request. |
| `templates/AGENTS.md` | Minimal rules to reconcile with your global `AGENTS.md`; never overwrite it silently. |
| `LICENSE` · `NOTICE` | License, attribution, and non-affiliation notice. |

The three **internal** skills use `metadata.opencode/autoinvoke: false`: they are not advertised to the model, and the orchestrator loads them by ID when needed. This metadata is not an exclusive access control. Runtime diagnostics run only on explicit request, never at every start. The external workflow skills are not included; they are a separate dependency.

## Requirements

- OpenCode v2.
- Node.js/npm with `npx` and network access to install or update external skills.
- `git`; parallel execution uses `git worktree`.

## Manual installation

This one-time setup makes the two agents and three internal skills in this package available to OpenCode, in addition to the external workflow skills. It is an alternative to [agent-assisted installation](#agent-assisted-installation); choose one method. Both have the same result and requirements.

Before changing OpenCode configuration, make a backup outside the repository. Compare destination files with the package and ask the user before replacing existing files; preserve other settings. Treat `templates/AGENTS.md` separately: agree on any reconciliation with the global `AGENTS.md`; never overwrite content silently.

### 1. External workflow skills

From the user's terminal, install them globally for OpenCode with the official CLI:

```sh
npx skills@latest add mattpocock/skills --global --agent opencode
```

The official repository is [mattpocock/skills](https://github.com/mattpocock/skills). The agent must never run the CLI. If an external skill is missing, the agent reports its ID and reason and provides this command; the user performs the installation.

Select `ask-matt`, `implement-spec`, `to-spec`, `to-tickets`, `handoff`, `tdd`, `code-review`, and `setup-matt-pocock-skills`. The last skill configures a repository during bootstrap when needed; it is not one of the nine runtime IDs. Global installation is shared across projects: do not manually copy external skills or install a second project-local copy.

### 2. Package agents and internal skills

Copy the package files into the OpenCode configuration:

- `agents/orchestrator.md` and `agents/orchestrator-implementer.md` into `<config>/agents/`;
- the folders `skills/orchestrator-bootstrap/`, `skills/orchestrator-retro/`, and `skills/orchestrator-runtime-diagnostics/` into `<config>/skills/`.

Invoke `orchestrator` when needed; do **not** set it as `default_agent`.

Choose models in the installed copies of the agents. The package specifies no model or reasoning preset and makes no recommendation; preserve any other frontmatter and settings.

## Agent-assisted installation

This is another way to install the **same global package**, not a step after manual installation. Ask an agent with access to this package and the OpenCode configuration to follow the paths and safeguards above: back up outside the repository, compare destination files, and get agreement before replacing files or reconciling configuration. The agent copies the two agents and three internal skills, asks the user to choose models, and applies those choices only to the installed copies. Agree separately on any integration of `templates/AGENTS.md`.

The agent does **not** run the external CLI, copy external skills, or install global dependencies. It provides the official command for the user to run and waits for the user's confirmation. Global setup is complete only after the user confirms installation and explicitly chooses the models.

## Bootstrapping a new project

When `.scratch/resume.md`, `.scratch/open-points.md`, or the project module in `AGENTS.md` is missing, `orchestrator` automatically loads the internal [`orchestrator-bootstrap`](skills/orchestrator-bootstrap/SKILL.md) skill. It checks tracker and triage configuration; if repository setup is missing or incomplete, it loads `setup-matt-pocock-skills` with the `skill` tool and follows its workflow. If setup is already satisfactory, it skips that step; missing `GLOSSARY.md` or ADRs alone do not trigger setup. The workflow may require user decisions. If the external skill is not installed globally, bootstrap reports the dependency without installing it. Global installation remains manual and separate from repository setup.

The skill creates `.scratch/resume.md` and `.scratch/open-points.md` only when missing, leaving existing files unchanged. It creates the project module only if `AGENTS.md` is missing; if the module already exists, it leaves the file unchanged. If `AGENTS.md` exists but the module does not, it agrees on changes with the user before integration.

## Using the workflow

Invoke `orchestrator` to follow the workflow: it proposes a work block and explains why, proactively restates open points, uses `ask-matt` for routing, and guides grilling with the user. It then writes a spec and tickets when needed and delegates to `orchestrator-implementer`; independent tickets can run in parallel through `implement-spec`.

For work that builds or changes application software, human testing remains a gate before closure. For package maintenance (agents, skills, instructions, and documentation), it does not request testing confirmation or wait for production feedback: after applicable technical checks and Standards + Spec review, it closes and starts the retrospective. The maintainer checks the package in production and requests changes when needed. Grilling, product decisions, and security and authorization constraints remain unchanged. In either case, it stops after two attempts without a concrete path forward; evidence-based corrections continue under *Informed retries* in the [agent instructions](agents/orchestrator.md#execution).

## Read-only verification

Runtime loading diagnostics are documented in the internal [`orchestrator-runtime-diagnostics`](skills/orchestrator-runtime-diagnostics/SKILL.md) skill, which the orchestrator loads only on explicit request, never at every start. Diagnostics are separate from global installation and project bootstrap: they load instructions without running workflows and do not install or update skills, copy files, change configuration, or create or modify project state. The skill defines checks, incomplete results, separate remediation, and when an installation cannot be certified. `setup-matt-pocock-skills` is an external project-setup skill invoked by bootstrap when needed; it is not a runtime ID and is not loaded or run by diagnostics.

To verify package installation, also check that:

- [ ] runtime diagnostics completed according to the [internal skill](skills/orchestrator-runtime-diagnostics/SKILL.md);
- [ ] `orchestrator` is selectable and `orchestrator-implementer` is available as a subagent;
- [ ] `opencode debug agents` shows `*` **deny** and `orchestrator-implementer` **allow** for `orchestrator`'s `subagent` permission;
- [ ] the models are selected in both installed agents and `opencode debug agents` shows the resolved models, without unintended inheritance;
- [ ] all three internal skills are present but not advertised;
- [ ] global `AGENTS.md` contains package rules reconciled with the user's rules;
- [ ] no previous configuration requires recreating a legacy agent organization;
- [ ] the project has `resume.md` (including `Retrospective:`) and `open-points.md`;
- [ ] on invocation, the agent opens with `▶ <Block> · Proposal · Why` and restates open points.

Diagnostics succeed only when all checks in the internal skill pass. To declare package installation complete, also verify every item above; incomplete diagnostics do not certify installation.

## Updates and rollback

- To update globally installed external skills, the user can run `npx skills update --global` from their terminal.
- Before updating this package, back up the OpenCode configuration, compare the new version, and update agent and internal-skill copies while preserving the user's model choices, permissions, and content. Do not replace or merge `AGENTS.md` without agreement.
- To roll back, restore the configuration and `AGENTS.md` backup. Restore or remove agent and internal-skill copies only after checking what depends on them. External skills are global and independent; they may remain installed and should be removed only if the user decides to do so.
- Project `.scratch/` state belongs to the project; preserve it unless the user explicitly decides otherwise. Before removing worktrees, check `git worktree list` and verify they contain no work that should be preserved.

## License and attribution

This package is independent and unofficial. `NOTICE` records attribution, non-affiliation, and that external workflow skills are a separate dependency. The package is distributed under the MIT License (`LICENSE`).
