# Install an Agent Harness in Any Repository

This file is an installation runbook for a coding agent. When asked to follow it, create or update the harness in the target repository and verify the result. Merely reading this file does not authorize installation. The harness uses Markdown files and a Git-ignored local workspace; it requires no particular language, framework, package manager, or agent runtime.

Use the repository where installation was requested as the target. Do not modify the repository hosting this guide unless it is the target. Write harness instructions and working records in English. Communicate with the user in their preferred language.

## 1. Inspect and Preserve the Target

1. Before inspecting repository content, read `.agent/memory.md` if it exists. If absent, create it with the installation objective and `in progress` status. Creating this local record is the bootstrap exception to reading directory guidance first.
2. Read existing applicable `AGENTS.md` files, including ancestor guidance outside the repository when available. Respect higher-priority instructions and the user's request.
3. Identify the repository root, existing changes, tracked files, maintained directories, ignore rules, documentation, and actual validation commands. Use `rg --files` and Git commands where available. Do not assume a stack or directory layout.
4. Recover any unfinished task recorded in memory before replacing its context. Preserve its objective, progress, recovery notes, and artifact links; record how installation relates to that task.
5. Inspect existing harness files and agent integrations. Merge compatible instructions rather than overwriting existing content or creating conflicting parallel systems. Reuse existing role definitions where suitable.
6. Preserve unrelated changes. Do not initialize Git, install dependencies, run applications, modify runtime configuration, or commit changes merely to install this harness.

Installation must be repeatable: a second run should repair missing or outdated harness content without duplicating rules, resetting memory, or removing valid history.

## 2. Create the Structure

Create these files and directories in the target, adapting only where existing repository instructions require another location. If adapted, update every reference consistently and record the mapping in the root guide.

```text
<repository>/
├── AGENTS.md
├── agents/
│   ├── AGENTS.md
│   ├── orchestrator.md
│   ├── planner.md
│   ├── worker.md
│   ├── reviewer.md
│   └── tester.md
└── .agent/                    # Local working records; ignored by Git
    ├── memory.md
    ├── planning/
    ├── history/
    └── index-history.md
```

The root and maintained child `AGENTS.md` files and role definitions are shared repository content. `.agent/` is private to each checkout and must not be committed or synchronized between contributors. Creating an empty `history/` directory is sufficient until a meaningful decision needs recording. Initialize `index-history.md` without invented decisions.

In a Git repository, add `/.agent/` to the root `.gitignore` if no existing rule already covers it. Preserve existing ignore rules. Verify with `git check-ignore` that both the directory and its contents are ignored. Check whether any `.agent/` files are already tracked: ignore rules do not untrack files. Report that conflict and resolve it within existing authorization; do not silently remove user records or force-add them.

For a repository without Git, create the structure without initializing Git, and add the portable `/.agent/` ignore rule to `.gitignore`. State that Git ignore behavior could not be verified until Git is initialized.

## 3. Write the Root Rules

Merge the following behavior into the root `AGENTS.md`. Keep the finished guide in English and at most 150 lines. Preserve repository-specific rules that already exist, but do not import architecture, commands, paths, or conventions from another project.

### Scope and Instruction Precedence

- Root instructions apply throughout the repository. Child guides override conflicting ancestor repository rules within their subtree; nonconflicting rules remain cumulative. User instructions and higher-priority system or developer instructions remain authoritative.
- Keep shared rules and a concise, factual repository map in the root guide. Store local rules in child guides and task-specific records under `.agent/`.
- Preserve existing changes and follow established project conventions. Resolve approximate names to existing concrete symbols; create aliases only when requested or necessary to resolve collisions.
- Write harness content in English. Communicate with the user in their preferred language.

### Directory Guidance

- After reading task memory first, read the root guide and every existing ancestor guide down to the directory of each file inspected, used, edited, or validated. Read a child's guide before operating inside it, including when crossing to another subtree.
- Every maintained, nonignored source or harness directory must have an English `AGENTS.md` of at most 150 lines. Each guide describes its purpose, immediate children and responsibilities, local rules, how to work there, and prohibited operations. Keep inventories current as contents change.
- Check Git ignore rules before adding guides. Ignored directories and their descendants are exempt; do not add guides there. `.agent/` and Git-managed `.git/` internals are exempt. Use the nearest applicable ancestor guidance for ignored directories.
- Generated, vendored, and runtime directories are not maintained source merely because they exist. Identify them from repository evidence and avoid modifying their contents as part of installation.

### Local Workspace and Task Lifecycle

- Agents may organize plans, caches, evidence, and working notes in `.agent/`. Never force-add or commit it. Each checkout has independent local records.
- `.agent/memory.md` is the single source of current task memory. On every task, read it before other repository files. If missing, create the directory and file before proceeding.
- Memory contains exactly one current status: `in progress` or `done`. Subagents report their own assignment state separately; they must not reset shared task status.
- If status is `in progress`, recover the objective, progress, next steps, and recovery notes; verify actual repository state and incorporate new instructions without forgetting unfinished work.
- If status is `done` or memory was just created, initialize the new task with objective, completion criteria, and required evidence, then set `in progress` before performing work.
- Update memory after meaningful progress, decisions, verification, and plan changes. Record enough context to resume without chat history.
- Set `done` only after meeting completion criteria and recording actual evidence. If unfinished or blocked, keep `in progress` with the reason and next steps. Ending a response is not completion.

### Long Plans

- Ensure `.agent/planning/` exists when initializing or resuming work.
- Save every user-requested long plan as `.agent/planning/YYYY-MM-DD-purpose.md`, using the user's local creation date and an English kebab-case purpose. Use a more specific purpose when a filename belongs to another plan.
- Include objective, scope, phases, completion criteria, and required evidence. Update the plan as work evolves and link it from memory when relevant to the current task.

### Decision History

- Maintain `.agent/history/` and `.agent/index-history.md` independently of current task memory.
- Name history files `YYYY-MM-DD.md` using the user's local opening date. If needed, use `YYYY-MM-DD-02.md`, then `-03.md`, and so on. Never overwrite a file or reuse a rotation suffix.
- Record meaningful user and agent decisions as level-two Markdown sections. Include the decision date, decision, and rationale or consequences when useful. Do not copy conversations or treat unaccepted proposals as decisions.
- Append to the active file until it has 100 sections; open a new file before section 101. Count level-two headings. The opening-date filename can contain later decisions.
- Retain at most two files: the active file and its immediate predecessor. Exclude any legacy `history/AGENTS.md` from counts, indexing, and deletion; do not create new guides inside the ignored workspace.
- Before rotating out a file, carry forward still-applicable decisions not preserved in shared rules or other durable artifacts. Verify the two newest files and index entries before deleting the oldest and its entries. Keep each new file within the section limit.
- When a decision changes, append a section referencing the superseded decision.
- Index every section as `[title] [file] [start] [end]`, where brackets are literal field delimiters, the file is relative to `.agent/`, and line numbers are inclusive and one-based. The title matches the heading text. Example: `[Use existing test runner] [history/2026-10-12.md] [1] [5]`.
- Update the index whenever sections change or rotate. Verify paths, headings, complete ranges, capacity, and retention before reporting completion.
- To recover past rationale, search the index first, then read the relevant file and line range. Keep current task context in memory and long plans in planning files.

### Delegation and Verification

- Role instructions live in `agents/orchestrator.md`, `planner.md`, `worker.md`, `reviewer.md`, and `tester.md`. Read the relevant definition before assigning or performing the role. These Markdown files do not register or launch runtime agents.
- One coordinator owns shared memory, history, and index writes during delegated work. Subagents read memory first, use separate artifacts under `.agent/`, and return progress and evidence to the coordinator.
- Assign objective, scope, deliverables, completion criteria, evidence requirements, dependencies, and file ownership. Route clarification through the assigner and consolidate questions requiring user input.
- Whenever an explicitly requested goal is created or pursued, or the orchestrator is invoked, use the orchestrator to engage a planner and dispatch relevant subagents. An ordinary task objective alone does not activate this workflow.
- Resolve routine questions from context and existing authorization. Ask the user only when required information or decisions remain missing; do not treat silence as approval. Continue independent work while dependent work waits.
- If the runtime cannot delegate, disclose the limitation and record the unfulfilled delegation requirement. Do not claim independent review or silently treat sequential role simulation as equivalent. Complete authorized work that does not depend on delegation, and report any remaining blocker.
- Perform validation appropriate to the changed behavior and repository conventions. Report only checks actually executed, their results, and material limitations. Do not invent commands, successful reviews, or test evidence.

Add the actual repository map and relevant existing boundaries after inspection. Include supported commands only when confirmed by project files or documentation. Do not add technology-specific rules to satisfy this installer.

## 4. Add Directory Guides

Enumerate maintained, nonignored source and harness directories. Read their existing guides before making changes. Add missing guides and update stale inventories using this shape:

```markdown
# <Directory> Guidance

## Purpose
Describe the directory's actual responsibility.

## Immediate Contents
- List immediate files or child directories and their responsibilities.

## Local Rules
- Record applicable conventions and boundaries supported by repository evidence.

## Working Here
- Explain how to use, change, and validate the contents.
- State concrete prohibited operations where applicable.
```

Use real names and instructions, not template placeholders. Avoid duplicating all root rules in every guide. Cover maintained directories recursively; skip ignored and nonmaintained generated or vendored trees. If classification is unclear, inspect existing documentation and tracked contents before deciding.

For `agents/AGENTS.md`, inventory the guide and five role files. Explain that definitions are instructions rather than runtime registration, that handoffs must remain consistent, and that the coordinator owns shared records. Prohibit fabricated evidence and conflicting shared-memory writes.

## 5. Write Portable Role Definitions

Create or merge the following instructions into the corresponding files. Each role reads `.agent/memory.md` first, then applicable directory guidance, its assignment, and relevant task artifacts. All roles preserve unrelated work and report missing context to the assigner.

### `agents/orchestrator.md`

- Coordinate explicitly requested goals or an explicitly invoked orchestration workflow using subagents.
- Recover interrupted work; establish objective, scope, constraints, acceptance criteria, and evidence. Own shared records unless a parent coordinator retains ownership.
- Engage the planner before dispatching implementation. Have it create or refresh a plan and divide it into bounded assignments with dependencies and file ownership.
- For implementation, follow planner → worker → reviewer → tester. Parallelize independent assignments with disjoint write ownership.
- Respond to subagent clarification requests. Resolve them from context and authorization, or ask the user in their preferred language and relay the answer. Keep dependent work waiting when a required answer is missing.
- Route review findings and test failures to the worker, then request review and affected retesting. Verify evidence against the final repository state.
- Keep memory and history current. Mark the task done only when criteria are met. Return outcome, artifact paths, review results, executed checks, and unresolved limitations.

### `agents/planner.md`

- Inspect relevant structure, conventions, and current behavior before choosing an approach.
- Clarify missing requirements that affect scope or acceptance; resolve routine implementation choices from evidence.
- Produce objective, scope, constraints, ordered steps, measurable acceptance criteria, validation strategy, dependencies, risks, and confirmed assumptions.
- For orchestrated work, define assignments with objective, scope, deliverables, acceptance criteria, evidence, dependencies, and file ownership. The orchestrator dispatches them.
- Save long plans under `.agent/planning/` using the naming rules. Verify reused plans against current state and update them as needed.
- Return the plan or path, confirmed scope, and blockers. Do not implement unless separately assigned.

### `agents/worker.md`

- Confirm assigned scope, ownership, plan, expected behavior, acceptance criteria, and evidence before editing.
- Follow nearby code and established architecture, naming, formatting, error handling, dependencies, and testing conventions.
- Implement focused changes within ownership. Coordinate shared-file edits; avoid unrelated refactors and unsupported public-contract or dependency changes.
- Ask the assigner when an ambiguity affects implementation; wait only on dependent work.
- Run appropriate checks and provide meaningful tests when required by the plan or behavior. Use the target repository's test layout.
- Address review findings and test failures with focused fixes. Return changed paths, behavior, rationale, actual verification, and remaining limitations to the coordinator.

### `agents/reviewer.md`

- Inspect the actual diff and surrounding code against the plan and acceptance criteria.
- Assess correctness, regressions, scope, directory guidance, conventions, compatibility, architecture, and evidence gaps. Assess security and performance when affected.
- Perform focused read-only checks where useful. Do not edit implementation unless separately assigned.
- Return actionable findings with severity, file and line, concrete failure or violated rule, impact, and required correction. Distinguish blockers from optional suggestions.
- Review fixes and state which changes were assessed, which checks ran, and what remains unverified. Finding no issues does not imply tests ran.

### `agents/tester.md`

- Validate the reviewed final state after blocking findings are resolved or explicitly dispositioned.
- Map acceptance criteria and affected behavior to suitable success, failure, boundary, and regression checks. Use existing frameworks and keep validation proportional to the change.
- Record cases, commands, prerequisites, and expected results before execution. Use the long-plan naming rules when a long test plan is needed.
- Execute checks and distinguish passed, failed, blocked, and skipped results. Record useful reproducible output without exposing secrets.
- Add tests only within assigned ownership and repository conventions. Route production fixes through the coordinator, worker, and reviewer.
- Retest affected behavior after fixes. Return criterion coverage, executed evidence, failures, and practical limitations; the coordinator decides overall completion.

## 6. Initialize Working Records

For a new memory file, use this structure, replacing placeholders with actual installation context. When memory already exists, preserve valid content and update it under the lifecycle rules.

```markdown
# Task Memory

## Status
in progress

## Current Task and Objective
Install a portable agent harness in this repository.

## Completion Criteria and Required Evidence
- Shared and directory guidance reflects the actual repository.
- Five role definitions exist with consistent handoffs.
- Local workspace exists and is ignored by Git where available.
- Verification results and any limitations are recorded.

## Plan and Progress
- Record completed steps and remaining work.

## Decisions and Recovery Notes
- Record relevant choices, preserved work, and recovery instructions.

## Evidence
- Record exact checks, results, and artifact paths after execution.

## Next Steps
- Record what the next session must do.
```

Record meaningful installation decisions in history, such as merging an existing harness or adapting a path. Do not manufacture entries to populate the directory. Link relevant plans from memory. Use the user's local date; if unavailable, use the environment's date and state that assumption.

## 7. Verify and Finish

Before reporting installation complete, check the actual files:

- Root guidance, maintained directory guides, and all five role files exist; each `AGENTS.md` is English and at most 150 lines.
- Directory inventories and the repository map match actual contents; no placeholders or imported project-specific rules remain.
- Guidance agrees on paths, lifecycle, precedence, delegation triggers, shared record ownership, clarification, and evidence requirements.
- `.agent/memory.md`, `.agent/planning/`, `.agent/history/`, and `.agent/index-history.md` exist. Memory has exactly one valid current status.
- In Git repositories, `.agent/` and its contents are ignored, none are newly staged, and any previously tracked local records have been addressed or explicitly reported as unresolved.
- History has at most two decision files and at most 100 sections per file. Every decision has a valid index entry with the correct heading and inclusive line range; an empty history has an empty index.
- Inspect the final diff and status when Git is available. Confirm unrelated work is preserved. Do not run application builds or tests for documentation-only installation unless repository rules require them.

Record executed checks and their results in memory. Set `done` only when installation criteria are met and no preserved prior task remains unfinished; otherwise retain `in progress` with concrete recovery steps. Report the created or updated paths, verification results, and any unresolved limitations concisely in the user's preferred language. Installation does not include committing or publishing changes.
