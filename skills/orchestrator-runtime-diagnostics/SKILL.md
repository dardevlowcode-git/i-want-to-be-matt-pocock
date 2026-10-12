---
name: Orchestrator Runtime Diagnostics
description: Read-only procedure to verify runtime loading of the orchestrator's internal and external skills.
metadata:
  opencode/autoinvoke: false
---

# Orchestrator runtime diagnostics

This skill is **internal to the `orchestrator` agent**. It is not advertised to the model (`opencode/autoinvoke: false`): the orchestrator loads it by ID (`orchestrator-runtime-diagnostics`) only when the user explicitly requests runtime skill diagnostics, never at every start.

## Boundaries

Read-only diagnostics verify skill loading and are separate from global installation and project bootstrap. Report load results and directory-name matches. Do not install or update skills, copy files, change configuration, or create or modify project state. Load skill instructions, but do not run their workflows.

## Check this skill

Successfully loading this skill is a prerequisite, not one of the nine runtime checks. Verify that the directory containing this `SKILL.md` is named exactly `orchestrator-runtime-diagnostics`, using a case-sensitive comparison. If the load path does not let you verify the directory name, report the diagnostics as incomplete and give the ID and reason. Do not call `orchestrator-runtime-diagnostics` to verify itself; recursive tenth invocation is not part of the procedure.

## Load the required skills

Invoke all nine exact IDs individually with the `skill` tool:

- external skills: `ask-matt`, `implement-spec`, `to-spec`, `to-tickets`, `handoff`, `tdd`, `code-review`;
- internal skills: `orchestrator-bootstrap`, `orchestrator-retro`.

The check passes only if every invocation returns the actual skill content and the directory containing its `SKILL.md` exactly matches its ID, case-sensitively. Include internal skills even when `autoinvoke: false`; a list, file, or frontmatter alone is not sufficient.

Bootstrap loads and follows `setup-matt-pocock-skills` only when repository configuration is missing or incomplete. This external setup skill is not one of the nine runtime IDs: diagnostics do not load it, invoke it, or run its workflow. The user installs external skills globally, as a separate manual action.

## Negative check

With `orchestrator-bootstrap` present, `Orchestrator-Bootstrap` must fail without returning content.

## Results and remediation

If this skill does not load or its directory name does not match, diagnostics are incomplete: report `orchestrator-runtime-diagnostics` and ask the user to restore `skills/orchestrator-runtime-diagnostics/` from the package under `<config>/skills/`; do not run positive checks. If any of the nine IDs fails to load or its directory does not match, report the ID and reason. Remediation is separate: for an external skill, give the user the official installation command without running it; for an internal skill, ask the user to restore its package directory. After remediation, run a new diagnostics invocation and repeat the path check for this skill, all nine positive invocations, the case-sensitive directory-name checks, and the negative check. Do not certify an incomplete installation.
