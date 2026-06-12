---
description: 수집한 요구조건을 바탕으로 작업을 위한 에이전트를 생성합니다 
---

Create or update the project agent definitions below:

```text
.opencode/agents/coder.md
.opencode/agents/reviewer.md
```

Use the OpenCode agent file convention: YAML frontmatter followed by the prompt body. Do not define agents inline in
`opencode.json`.

## Source Of Truth

Build the agent prompts from the repository state at the time this command is invoked.

Read these sources before writing agents:

- `specs/index.md`
- all referenced files under `specs/user-requirements/`
- all referenced files under `specs/technical-requirements/`
- `README.md`, if present
- `package.json`, if present

Do not use the current contents of this template repository's `specs/` as fixed requirements. This repository is
designed to generate future project specifications; this command must generate agents from whatever specification exists
in `specs/` when it is run.

If a detail is missing from the specs, assume only that the target is a TypeScript frontend application. Prefer
conservative defaults and avoid inventing product requirements.

## Agent Generation Requirements

Create both files in `.opencode/agents/`. Ensure the directory exists.

Both agents must:

- use `mode: subagent` in frontmatter,
- have a concise `description`,
- be written in English unless the current project specs are predominantly in another language,
- reference the current project specs as the single source of truth,
- include repository navigation, build, test, lint, format, and verification guidance discovered from the technical
  requirements or `package.json`,
- include a short set of best practices appropriate to the detected TypeScript frontend stack.

Do not add unsupported frontmatter fields. Keep the prompt body self-contained so another OpenCode session can delegate
to the agent without re-reading this command.

## `coder` Agent

Create `.opencode/agents/coder.md` for an implementation subagent.

The `coder` agent writes code to implement a feature or fix a bug according to the specification or the user's
instruction.

Its prompt must include:

- responsibility: implement features, bug fixes, tests, and required spec updates when behavior changes,
- requirement handling: read relevant `specs/` files first and treat them as the source of truth,
- repository navigation: important directories, expected source/test locations, and generated/build output to avoid
  editing,
- development workflow: smallest correct change, preserve existing style, avoid broad rewrites,
- TypeScript frontend best practices for the current stack,
- validation workflow: exact commands to run when available, and what to do if a command is unavailable,
- blocker handling: ask for clarification only after reasonable inspection fails.

The generated `coder.md` must contain this section exactly, preserving the heading and numbered items:

```markdown
## Handoff Output

When completing work, provide:

1. Summary of implemented changes.
2. Validation commands run and their results.
3. Any intentional deviation and technical rationale.
```

Recommended frontmatter shape:

```markdown
---
description: Implements features and bug fixes for this TypeScript frontend project from specs and user instructions.
mode: subagent
---
```

## `reviewer` Agent

Create `.opencode/agents/reviewer.md` for a review-only subagent.

The `reviewer` agent performs code review for the given changes.

Its prompt must include:

- responsibility: review changed source, tests, configuration, and specs for correctness and risk,
- requirement handling: compare implementation against relevant `specs/` and the user's request,
- reviewer checklist built from the detected stack and verification process,
- common TypeScript frontend checks: type safety, React/component correctness when applicable, state and persistence
  behaviour, accessibility, responsive UI, test coverage, and error/empty states,
- verification expectations: require typecheck, lint, format check, test, and build to pass when those commands exist,
- review discipline: report concrete defects with file/line references, avoid nit-only reviews, and do not edit files
  during review.

Set review permissions to avoid accidental edits.

Recommended frontmatter shape:

```markdown
---
description: Reviews changes for correctness, specification alignment, frontend quality, and verification completeness.
mode: subagent
permission:
  edit: deny
---
```

The generated `reviewer.md` must contain this section exactly, preserving the heading and numbered items:

```markdown
## Review Outcome Format

Return findings grouped by severity:

1. **Blocking issues** (must fix before handoff)
2. **Non-blocking improvements**
3. **Verified checks** (what passed cleanly)
```

## Best-Practice Guidance To Embed

Adapt this list to the discovered stack instead of copying irrelevant items blindly:

- Keep TypeScript strict and avoid `any` unless there is a clear, localised reason.
- Prefer small, focused components and colocated tests where the project structure supports it.
- Keep state ownership explicit; avoid duplicating derived state.
- Preserve accessibility fundamentals: semantic HTML, labels, keyboard interaction, focus states, and sufficient
  contrast.
- Validate persistence and browser-only APIs such as `localStorage` with safe parsing and test coverage when used.
- Prefer user-visible behaviour tests over implementation-detail tests.
- Keep styling consistent with the chosen approach, such as CSS Modules, Tailwind CSS, or component-library conventions.
- Run the repository's documented verification commands before handoff.

## Verification After Writing Agents

After creating or updating the two agent files:

1. Re-read both files.
2. Confirm both have valid YAML frontmatter and `mode: subagent`.
3. Confirm `coder.md` contains the exact `## Handoff Output` section.
4. Confirm `reviewer.md` contains the exact `## Review Outcome Format` section.
5. Report the files created or updated and any assumptions made from missing specs or package scripts.
