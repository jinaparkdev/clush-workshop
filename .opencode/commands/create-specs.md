---
description: 초기 프로젝트 명세를 생성합니다
---

Create the initial specifications for a small spec-driven frontend example project in a repository that contains only minimal development files.

Write this command's user-facing interview, summaries, approval choices, final status message, and all generated project documents in Korean. Keep these command instructions in English.

Do not write `README.md` or any `specs/` document until the user has approved the collected requirements.

## Goal

Interview the user, then create a concise initial specification set that a coding agent can use as the source of truth.

Collect and confirm two groups of requirements:

- User requirements: what application to build, who it is for, and what it must do.
- Technical requirements: how the TypeScript frontend application should be built.

## Fixed Constraints

Enforce these constraints throughout the interview and generated specs:

- The project must be a standalone frontend application.
- Application code must be written in TypeScript.
- User data must be stored in memory or `localStorage`.
- Consider Mock Service Worker, MSW, only when backend-like behavior is useful.
- Scope must be small enough for an AI agent to build quickly. Treat roughly twice the complexity of a typical Todo app as the upper bound.
- If the user asks for a larger scope, explain the limitation in Korean and guide them to reduce the initial feature set before continuing.

## Interview Rules

Use the `question` tool for user questions. Write every question and choice in Korean.

The `question` tool already provides a custom answer option. Do not add a duplicate option such as "Other" or "Custom" to the choices. Tell the user in Korean that they can type their own answer if none of the choices fit.

Ask focused questions in small batches. Use each answer to decide the next question. Continue until the requirements are concrete enough for implementation.

## User Requirements Interview

Start by explaining in Korean that:

- This repository is a starting point for a simple TypeScript frontend example app.
- Data will stay in the browser.
- The app must stay small and focused.
- The user can choose a suggested idea or type their own.

The first question must ask what application the user wants to build. Offer these choices:

- 개인 칸반(Kanban) 보드
- 주차장 관리 도구
- 영화 감상 목록(Watchlist)

Then ask follow-up questions appropriate to the selected app until these details are clear:

- One-sentence product goal.
- Target user or usage scenario.
- Three to five core features.
- Main screens and navigation flow.
- Data the user can create, edit, delete, or view.
- Required interactions such as sorting, filtering, search, status changes, or drag-and-drop.
- Expected persistence behavior.
- Explicitly out-of-scope features.
- Completion criteria or successful usage scenarios.

If the scope grows too large, split features by priority and keep only the essential ones for the initial version.

## Technical Requirements Interview

After user requirements are clear, interview for technical requirements.

For each category, present two to four viable options based on the collected user requirements. Mark a clearly preferred option with `[추천]` in the Korean choice label. Keep recommendations lightweight; do not suggest unnecessary tools for a small project.

Confirm these categories:

- Application framework and build tool, such as Vite + React, Vite + Preact, or another suitable TypeScript frontend setup.
- Whether routing is needed, and which router to use if it is.
- Styling approach and component library, such as CSS Modules, Tailwind CSS, shadcn/ui, or a Radix UI based setup.
- State management approach, such as React state, Context, or Zustand.
- Data persistence approach, such as memory, `localStorage`, or MSW plus `localStorage`.
- Form handling and validation approach.
- Testing and verification scope, such as Vitest, React Testing Library, and Playwright where appropriate.
- Code quality tools, such as ESLint, Prettier, and TypeScript strict settings.

## Approval Gate

When both requirement groups are clear, show a concise Korean summary before writing files.

Include:

- App name and one-sentence description.
- Core features.
- Main screens.
- Persistence approach.
- Selected tech stack and key libraries.
- Out-of-scope features.
- Initial implementation completion criteria.

Then ask for approval with the `question` tool. Example choices:

- 승인하고 명세 작성 [추천]
- 요구사항 수정
- 기술 선택 수정

If the user provides feedback or chooses a revision path, interview again for the affected area, then summarize and ask for approval again. Write files only after approval.

## Files To Create

After approval, create or update these files. All content must be in Korean.

```text
README.md
specs/index.md
specs/user-requirements/overview.md
specs/user-requirements/scope.md
specs/user-requirements/workflows.md
specs/user-requirements/screens.md
specs/user-requirements/data-model.md
specs/user-requirements/acceptance-criteria.md
specs/technical-requirements/stack.md
specs/technical-requirements/architecture.md
specs/technical-requirements/project-structure.md
specs/technical-requirements/state-and-persistence.md
specs/technical-requirements/ui-and-styling.md
specs/technical-requirements/verification.md
```

Add more subpages only if they improve navigation. Keep the spec set compact.

## Document Requirements

`README.md` must help a new reader quickly understand the project purpose, core features, selected stack, and how development should start.

`specs/index.md` must be a table of contents, not a detailed requirements document. It must contain exactly these two sections:

- 사용자 요구사항
- 기술 요구사항

Each section should link to the relevant subpages and include only a one-line description per link.

User requirement documents must clearly capture:

- Product goal and target users.
- In-scope and out-of-scope features.
- Core user workflows.
- Screen structure.
- Data model.
- Acceptance criteria.

Technical requirement documents must give coding agents enough information to implement the app:

- Selected framework, build tool, and libraries.
- Project directory structure.
- Screen and component organization principles.
- State management and persistence approach.
- Whether MSW is used, and what role it plays.
- Styling and accessibility expectations.
- Test, lint, typecheck, build, and manual verification guidance.

Prefer clear decisions and implementation-relevant facts over verbose explanation. Do not leave important items as unknown. If a required detail is missing, ask the user before writing the specs.

## Completion Message

After writing files, respond in Korean with only:

- The files created or updated.
- The recommended next step.

Because OpenCode command files are loaded at startup, tell the user to restart OpenCode if this command was just added or changed and the running session does not recognize it.
