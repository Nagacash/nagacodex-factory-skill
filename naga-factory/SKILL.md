---
name: naga-factory
description: Naga Codex factory workflow — 4-beat isolate/build/prove/ship pipeline orchestrating new-feature, code-structure, evidence-driven-testing, before-and-after, greploop/greploop-apps, and unslop. Use when starting any new feature, fixing a bug, or shipping a PR; when the user says "factory", "naga workflow", "start new task", or wants the full Isolate-Build-Prove-Ship cycle with before/after proof and 5/5 Greptile.
---

# Naga Codex Factory

Master skill that ties the 7-piece factory pipeline into the 4-beat workflow from `AGENTS.md`. Each beat delegates to a focused sub-skill.

## Workflow — 4 beats

1. **Isolate — `/new-feature`** Every task starts in a fresh Git worktree branched from `origin/main`. Never build on `main`. Scope-check open PRs (`gh pr list` + `gh pr diff <n> --name-only`) before creating the branch.

2. **Build — `/code-structure`** Enforce two-layer separation: actions/boundaries orchestrate the "why/when" (business rules, auth, state transitions), service layer owns the reusable "how" (provider SDKs, command execution, health checks) with explicit inputs and structured returns.

3. **Prove — `/evidence-driven-testing`** Verify with repo checks plus runtime evidence. Record with `scripts/evidence.py` (or headless `before-and-after` / Playwright). Capture the **before** failure before fixing, then the **after** success. Output `evidence.mp4`, `report.md`, `manifest.json`.

4. **Ship — `/before-and-after` then `/greploop`** Embed before/after proof in the PR description (`before-and-after <before> <after> --markdown` + upload adapter). Run `/greploop` (or `/greploop-apps` for huge PRs over Greptile file limit) until **5/5 with zero unresolved comments**. Present PR URL.

Apply `/unslop` to every human-facing text (commits, PR title/body, docs, comments, replies) before posting.

## Multi-agent rules

- Never commit directly to `main`; one worktree + one branch per task/agent.
- Never reuse another agent's worktree/branch/uncommitted work.
- Only `--force-with-lease` on your own branch; never force-push main.
- Regenerate lockfiles, don't hand-merge.
- Worktrees don't isolate ports/DBs — verify `lsof -i :<port>` serves your process.

## Completing a task

1. Keep changes scoped to the assigned task.
2. Run repo checks.
3. Assemble before/after pairs from evidence.
4. Commit with clear message, rebase onto `origin/main`, rerun checks.
5. Push (`git push -u origin <branch>`; `--force-with-lease` after rebase).
6. Open PR with what/how-tested/before-after/risks, run title/body through `/unslop`.
7. Run `/greploop` until 5/5.
8. Present PR URL. Do not merge unless explicitly instructed; keep worktree until merged/closed.

## Skill sources

| Skill | Source |
|---|---|
| `new-feature`, `code-structure`, `evidence-driven-testing` | this factory |
| `before-and-after` | vendored vercel-labs/before-and-after (`@vercel/before-and-after`) |
| `greploop` | vendored greptileai/skills |
| `greploop-apps` | local variant of greploop for huge PRs |
| `unslop` | vendored cursor/plugins (pstack) — frontmatter edited for auto-invocation |

## Sub-skills bundled

Invoke individually or let this orchestrator delegate:

- `/new-feature` — isolated worktree from `origin/main`
- `/code-structure` — service-layer architecture
- `/evidence-driven-testing` — annotated screen recording + headless path
- `/before-and-after` — visual diff + markdown upload
- `/greploop` — loop until Greptile 5/5
- `/greploop-apps` — same but with `@greptile-apps` for oversized PRs
- `/unslop` — strip AI tells from human-facing prose

See `AGENTS.md` for the canonical workflow text and `README.md` for per-skill details. Drop `AGENTS.md` into any repo to enforce the workflow.

