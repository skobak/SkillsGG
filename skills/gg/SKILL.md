---
name: gg
description: "Codex-specific one-command workflow to ship a branch safely: read repository rules, run project quality gates, perform security-focused review, write a concise conventional commit, and create a clear PR. Use when the user asks for GG, ship this, commit and push, or create a PR from current changes."
---

# GG (Codex)

Use this skill only in Codex environments.

## What this does

Turn current local changes into a review-ready PR with safety checks and clear communication.

## Workflow

1. Read repo rules first.
- Load `AGENTS.md`, `README`, and CI workflow files.
- Follow project-specific rules over generic defaults.

2. Inspect git state.
- Run `git status --short` and `git rev-parse --abbrev-ref HEAD`.
- If no changes, stop.

3. Enforce branch safety.
- If on `main` (or protected trunk), request explicit permission before commit/push.

4. Build quality gates from the repo CI.
- Treat CI workflow files as source of truth (for example `.github/workflows/*` or equivalent in-repo CI config).
- Reuse the same commands locally before push whenever possible.
- If a CI job cannot run fully local (for example hosted services), run the closest local equivalent and note the gap.
- Default execution order: lint/static checks, typecheck, tests, build.

5. Run a security pass on the diff.
- Check for secrets, auth/authz regressions, and injection risks.
- If backend/auth/database code changed, do deeper review before commit.
- If dependencies changed, run available dependency audit checks.

6. Stage and validate locally before push.
- Run `git add -A`.
- Run CI-equivalent local quality gates.
- If auto-fix changes files, restage and rerun required checks.

7. Create commit message.
- Use `type(scope): short outcome`.
- Keep subject imperative, lowercase, concise.
- Add short body only when useful:
  - `business: <impact>`
  - `change: <main implementation>`
  - `risk: <low|medium|high + note>`

8. Commit.
- Commit once checks pass.

9. Run review passes.
- Pass A: Codex deep code review (bugs, regressions, security, missing tests).
- Pass B: quick-summary review in plain language.
- If `quick-review` skill exists, use it; otherwise produce an equivalent category summary.

10. Push and PR.
- Push branch.
- Reuse existing PR if already open.
- Otherwise create PR with title from commit subject.

## PR Body Standard

Include these sections:

- Summary
- Business outcome
- Key changes
- Security and risk notes
- Validation evidence (commands run + result)
- CI parity notes (what matched CI exactly, what was approximated)
- Reviewer guide (where to start, what to scrutinize)

## Stop Conditions

Stop and ask before pushing when:

- critical security issue is found
- quality gate failures remain unresolved
- branch protection expectations are unclear

## Guardrails

- Never force-push.
- Never bypass required checks without explicit user approval.
- Never claim checks passed if they were not run.
- Never push when required CI-equivalent local checks were not attempted.
