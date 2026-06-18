# oxford_professional — CLAUDE.md

## What this repo is

Jon Oxford's faculty document pipeline. One private GitHub repo maintains three outputs:

1. **CV** — public-facing, builds and deploys to GitHub Pages via GitHub Actions
2. **Annual self-evaluation** — confidential, local render only, never deployed
3. **Promotion dossier** — confidential, local render only, never deployed

## Critical access-control constraint

A GitHub Pages site published from a private repo is still publicly reachable by anyone with the URL. GitHub Enterprise is required for viewer-restricted Pages — that is not available here.

Consequences that must be enforced in every session:

- The CV may publish through Pages — it is meant to be public.
- The annual self-evaluation and promotion dossier are confidential personnel documents and **must never** be built or deployed through any Pages workflow.
- They render locally to PDF/HTML and stay inside the private repo only.
- The public CV and the private documents are two entirely separate Quarto projects — separate `_quarto.yml` files, no shared navigation or sitemap — so a private page can never accidentally surface in the public build.

If any proposed change would cause a private document to appear in the Pages build or be referenced from `cv/_quarto.yml`, refuse it and explain why.

## Folder layout

```
source/      # Raw inputs: Jon's logs, criteria/examples for evals and dossiers, current CV content
             # Files go in here unedited — do not restructure Jon's source material
cv/          # Public Quarto project → GitHub Pages
             # GitHub Actions workflow scoped to this folder only
private/     # Quarto projects for annual self-evaluation and promotion dossier
             # Local render only — no Actions deploy step ever touches this folder
```

## Update cadence

Jon updates periodically, not in real time. The workflow is:

1. Jon accumulates entries in logs between sessions.
2. At update time, he hands Claude Code what has accumulated since last time.
3. Claude Code drafts additions; Jon reviews before committing.
4. Jon runs `quarto render` locally for private documents; GitHub Actions handles the CV on push.

No automation is needed for the logging step itself.

## Authoring rules

- Match Jon's existing structure from criteria and examples — do not invent new formats.
- If anything in `source/` is ambiguous, ask rather than filling gaps from general disciplinary or institutional knowledge.
- Never commit rendered PDF/HTML output — render targets are in `.gitignore`.
- Never add `private/` paths to `cv/_quarto.yml` navigation or includes.
