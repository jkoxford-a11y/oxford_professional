# Faculty document pipeline — build brief for Claude Code

## Goal
Maintain Jon Oxford's CV, annual self-evaluation, and promotion dossier from one private GitHub repo, updated periodically from logs Jon already keeps, rendered via Quarto into polished HTML/PDF.

## Critical constraint — write this into CLAUDE.md before scaffolding anything
A GitHub Pages site published from a private repo is still reachable by anyone with the URL. There is no real access-control option for an individual GitHub account — restricting a Pages site to authorized viewers only is a GitHub Enterprise feature, not available here.

Consequences:
- The CV can publish through Pages — it's meant to be public, same as Jon's existing personal CV site.
- The annual self-evaluation and promotion dossier are confidential personnel documents and must NEVER be built or deployed through any Pages workflow. They render locally to PDF/HTML and stay inside the private repo only.
- Keep the public CV and the private documents as two entirely separate Quarto projects — separate `_quarto.yml` files, no shared navigation or sitemap — so a private page can never accidentally surface in the public build.

## Build order

1. **CLAUDE.md first.** Before creating folders, write the constraint above, the folder layout below, and the update cadence (periodic/batch, reviewed and committed by Jon — not real-time, not autonomous) into CLAUDE.md so every future session enforces it without re-explanation.

2. **Repo + folder scaffold.** A private repo with:
   - `source/` — raw inputs: Jon's existing logs, his criteria/examples for annual evals and promotion dossiers, and his current CV content.
   - `cv/` — public-facing Quarto project. Builds and deploys to Pages via a GitHub Actions workflow scoped only to this folder.
   - `private/` — Quarto projects for the annual self-evaluation and promotion dossier. Local render only. No Actions deploy step touches this folder.

3. **Jon adds source documents.** Logs, criteria, examples, and current CV go into `source/` unedited.

4. **Build templates.** Convert the source material into working Quarto templates for each of the three outputs, matching the structure of Jon's existing examples rather than inventing new formats — his criteria already make these predictable.

5. **Wire rendering.** GitHub Action: `cv/` → Pages on push. Plain `quarto render` command for `private/` outputs, run locally, never deployed.

6. **Dry run.** Push one real, already-known milestone through the full pipeline end to end before relying on it for anything that matters.

7. **Ongoing cadence.** Periodically (not real-time): hand Claude Code what's accumulated in the logs since last time, review what it drafts, commit, re-render.

## Notes for Claude Code
- Jon already has criteria and examples that make eval and promotion formatting predictable. Match that structure — don't invent new structure.
- Jon updates periodically, not in real time. No automation is needed for the logging step itself, only for the build/render step.
- If anything in `source/` is ambiguous, ask rather than filling gaps from general disciplinary or institutional knowledge.
