# Senior Dev Team — Multi-AI Engineering Instructions

Portable senior engineering workflow for Codex, Claude Code, Cursor, GitHub Copilot, Gemini CLI, and tools that read `AGENTS.md`.

## Capabilities

- Senior Product / Business Analyst: requirements, business rules, user stories, acceptance criteria, and traceability
- Senior UX/UI Product Designer: journeys, flows, design systems, interaction states, accessibility, and responsive behavior
- Senior Tech Lead / Architect
- Senior Frontend: Nuxt 3, Vue 3, TypeScript, Pinia, React, and Next.js
- Senior Backend: Node.js, NestJS, Fastify, Express, Nitro, PostgreSQL, Redis, Prisma, and Drizzle
- Senior Data / Database: modeling, integrity, indexes, migrations, query performance, retention, and recovery
- Senior QA / SDET: risk-based unit, component, contract, integration, E2E, regression, accessibility, and resilience testing
- Senior DevOps / SRE: Docker, CI/CD, observability, rollback, backup, and cloud deployment
- Senior Security: secure design and review, trust boundaries, secrets, dependency risk, and OWASP-oriented controls

The Tech Lead selects only the roles needed for a task. Actual parallel agents depend on the AI tool and its enabled capabilities.

## Repository layout

```text
core/senior-dev.md                         Canonical, vendor-neutral instructions
skills/senior-dev/                         Codex-compatible install path
skills/senior-dev/references/              Kanban branching and generic CI/CD workflow references
adapters/codex/skills/senior-dev/          Codex adapter copy
adapters/claude-code/.claude/skills/       Claude Code Agent Skill
adapters/cursor/.cursor/rules/              Cursor Project Rule
adapters/github-copilot/.github/            GitHub Copilot instructions
adapters/gemini-cli/GEMINI.md               Gemini CLI context
adapters/generic/AGENTS.md                  Generic agent instructions
```

## Install

Repository: `https://github.com/arituch-ke/senior-dev-skill-ai`

### Codex

Ask Codex:

```text
Install the senior-dev skill from:
https://github.com/arituch-ke/senior-dev-skill-ai/tree/main/skills/senior-dev
```

Manual user-level install:

```bash
git clone https://github.com/arituch-ke/senior-dev-skill-ai.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R senior-dev-skill-ai/skills/senior-dev "${CODEX_HOME:-$HOME/.codex}/skills/senior-dev"
```

Use it on the next turn with `$senior-dev`.

### Claude Code

Claude Code supports Agent Skills. Copy the skill into a project's `.claude/skills` directory:

```bash
mkdir -p .claude/skills
cp -R PATH_TO_THIS_REPO/adapters/claude-code/.claude/skills/senior-dev .claude/skills/senior-dev
```

Invoke it with `/senior-dev` or ask Claude to use the senior-dev skill. Claude Code can delegate to its Agent tool when appropriate.

### Cursor

Copy the project rule:

```bash
mkdir -p .cursor/rules
cp PATH_TO_THIS_REPO/adapters/cursor/.cursor/rules/senior-dev.mdc .cursor/rules/senior-dev.mdc
```

The rule is agent-requested. Ask Cursor to use the `senior-dev` rule, or select it from the context picker.

### GitHub Copilot

If the target repository does not already have custom instructions:

```bash
mkdir -p .github
cp PATH_TO_THIS_REPO/adapters/github-copilot/.github/copilot-instructions.md .github/copilot-instructions.md
```

If `.github/copilot-instructions.md` already exists, merge the contents instead of overwriting it. Copilot's agent features vary by product surface; the instructions still provide role selection and engineering gates.

### Gemini CLI

If the target repository does not already have a `GEMINI.md`:

```bash
cp PATH_TO_THIS_REPO/adapters/gemini-cli/GEMINI.md GEMINI.md
```

If `GEMINI.md` already exists, merge the contents or import a copied module using Gemini CLI's `@file.md` syntax. Run `/memory refresh` after changing context files.

### Generic `AGENTS.md` tools

Copy `adapters/generic/AGENTS.md` to the target repository root. Merge it with an existing `AGENTS.md` instead of overwriting project-specific instructions.

## Usage examples

```text
Use the Senior Dev Team to implement this Nuxt 3 feature. Select only the necessary roles, delegate independent work when supported, run appropriate tests, and integrate the result as Tech Lead.
```

```text
Review this repository as the Senior Dev Team. Prioritize correctness, maintainability, security, accessibility, observability, and production readiness.
```

```text
Productionize this API using the Senior Dev Team. Do not deploy or change production secrets without my explicit approval.
```

```text
Use the Senior Dev Team to define acceptance criteria, design the UX flow, implement the feature, and have QA verify it independently before release readiness review.
```

## Compatibility notes

- The canonical workflow is vendor-neutral.
- Codex and Claude Code can load the `SKILL.md` form on demand.
- Cursor, Copilot, and Gemini adapters are persistent project instructions and may consume context on each relevant interaction.
- An instruction file cannot create multi-agent capability where the host product does not provide it.
- Permissions and approval policies of the host AI always take precedence.

## Publish to GitHub

1. Create an empty GitHub repository.
2. Upload this directory's contents at the repository root.
3. Commit and push.
4. Share the repository URL, or the `skills/senior-dev` tree URL for direct Codex installation.

No license is included. Add the license you want before public distribution or external contributions.
