# Identity Hub Quarto Rebuild Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the Jekyll/academicpages site with a single-page Quarto identity hub per `docs/specs/2026-07-18-identity-hub-design.md`.

**Architecture:** Fresh Quarto website in the existing repo on `master`; `quarto render` outputs to `docs/`; GitHub Pages source flips to `master:/docs` only at the operator-approved go-live. All commits stay local until the operator approves the rendered site — the push IS the live swap gate.

**Tech Stack:** Quarto (about-page template `trestles`), GitHub Pages, `gh` CLI.

## Global Constraints

- **No departure language** anywhere on the site: the operator is a USDA ERS research economist AND independently builds tools; never "leaving", "transitioning", "launching".
- The identity paragraph (Task 2) is ratified copy — use **verbatim**, no edits.
- No dead links, placeholder sections, or empty anything.
- Projects lists exactly **rnassqs** and **herdr** at launch. No private repos.
- Byline **Nicholas Potter**; academic-name line "I publish academic work as Nicholas A. Potter" appears in the identity paragraph.
- Do NOT push to origin at any point in Tasks 1-3. Push happens only in Task 4 after explicit operator approval.
- Do NOT create a `CNAME` file (domain not yet purchased).
- Any connective copy beyond the ratified paragraph: minimal, matches `~/.config/agents/references/writing-style.md`, flagged in the task report for operator review.

---

### Task 1: Install Quarto and replace Jekyll scaffold

**Files:**
- Delete: `_config.yml`, `_pages/`, `_data/`, `Gemfile` and any `Gemfile.lock`, `_includes/`, `_layouts/`, `_sass/`, `assets/` (Jekyll theme dirs — delete only those that exist; check with `ls -a`), `files/potter-resume.pdf` (spec drops the resume; git history preserves it)
- Keep: `images/` (all), `files/potter-cv.pdf`, `docs/specs/`, `docs/plans/`, `.git`, `README.md` if present
- Create: `_quarto.yml`, `styles.css` (empty placeholder is NOT allowed — omit the file entirely unless a rule is needed), `.nojekyll` at repo root

- [ ] **Step 1: Install Quarto CLI** (not preinstalled; sandbox note: only the
worktree and `~/.cache` are writable in orchestra workers — install under
`~/.cache`)

```bash
mkdir -p ~/.cache/quarto && cd ~/.cache/quarto
curl -fsSL -o quarto.tar.gz https://github.com/quarto-dev/quarto-cli/releases/download/v1.6.42/quarto-1.6.42-linux-amd64.tar.gz
tar xzf quarto.tar.gz && rm quarto.tar.gz
~/.cache/quarto/quarto-1.6.42/bin/quarto --version
```

Expected: `1.6.42`. Use the full path
`~/.cache/quarto/quarto-1.6.42/bin/quarto` everywhere quarto is invoked in
this plan.

- [ ] **Step 2: Inventory then delete Jekyll source**

```bash
cd /home/potterzot/workspace/projects/econpotter.github.io
ls -a
git rm -r _config.yml _pages _data 2>/dev/null
for d in _includes _layouts _sass assets Gemfile Gemfile.lock; do [ -e "$d" ] && git rm -r "$d"; done
git rm files/potter-resume.pdf
```

- [ ] **Step 3: Create `_quarto.yml`**

```yaml
project:
  type: website
  output-dir: docs

website:
  title: "Nicholas Potter"
  favicon: images/favicon.ico

format:
  html:
    theme: cosmo
    toc: false
```

- [ ] **Step 4: Create minimal `index.qmd` scaffold** (content filled in Task 2)

```markdown
---
title: "Nicholas Potter"
image: images/portrait.png
about:
  template: trestles
  links:
    - icon: github
      text: GitHub
      href: https://github.com/econpotter
---

Content in Task 2.
```

Note: check which portrait the old `_pages/about.md` / `_config.yml` referenced before deleting them (Step 2 — read first). Use that image (`images/portrait.png`, `portrait1.jpg`, or `avatar.jpg` — whichever the live site displays today).

- [ ] **Step 5: Verify the site renders**

```bash
touch .nojekyll
~/.cache/quarto/quarto-1.6.42/bin/quarto render
ls docs/index.html
```

Expected: render succeeds; `docs/index.html` exists.

- [ ] **Step 6: Commit (local only — do not push)**

```bash
git add -A
git commit -m "feat: replace Jekyll scaffold with Quarto site skeleton"
```

---

### Task 2: Page content — identity, publications, projects, link rail

**Files:**
- Modify: `index.qmd`

**Interfaces:**
- Consumes: Task 1's `index.qmd` scaffold and `_quarto.yml`.
- Produces: the complete single page; Task 3 only verifies links and render.

- [ ] **Step 1: Write the full `index.qmd`**

Front matter (verify Scholar and LinkedIn URLs against the old site's `_config.yml`/footer before Task 1 deletes it — record them in the task report; the Scholar user ID below MUST be replaced with the real one found there):

```markdown
---
title: "Nicholas Potter"
image: images/portrait.png
about:
  template: trestles
  links:
    - icon: github
      text: GitHub
      href: https://github.com/econpotter
    - text: "Google Scholar"
      href: <SCHOLAR-URL-FROM-OLD-CONFIG>
    - text: ORCID
      href: https://orcid.org/0000-0002-3410-3732
    - icon: linkedin
      text: LinkedIn
      href: <LINKEDIN-URL-FROM-OLD-CONFIG>
    - icon: file-earmark-text
      text: CV
      href: files/potter-cv.pdf
---
```

Body, exactly this structure (identity paragraph verbatim from the spec):

```markdown
I'm an economist working on water, climate, and agriculture in the western
United States. I'm a research economist at USDA's Economic Research Service,
where my work focuses on water supplies, drought, and irrigated agriculture.
Outside of that role I build tools that make water and climate risk easier to
understand: parsing water-right records and their case histories, forecasting
seasonal water supply, and estimating the regional economic consequences of
change in agriculture and natural resources. I publish academic work as
Nicholas A. Potter.

## Selected publications

- Potter, N.A. et al. (2024). Predicting the spatial variation in
  cost-efficiency for agricultural greenhouse gas mitigation programs in the
  U.S. *Carbon Balance and Management*.
  [doi:10.1186/s13021-024-00252-6](https://doi.org/10.1186/s13021-024-00252-6)
- Potter, N.A. et al. (2024). The Cost-Effectiveness of Irrigation Canal
  Lining and Piping in the Western United States. In *American Agriculture,
  Water Resources, and Climate Change* (University of Chicago Press).
  [doi:10.7208/chicago/9780226830629-005](https://doi.org/10.7208/chicago/9780226830629-005)
- Potter, N.A. (2025). Irrigation Organizations: Water Measurement and
  Pricing. *USDA Economic Research Service*.
  [doi:10.32747/2025.9015825.ers](https://doi.org/10.32747/2025.9015825.ers)
- Potter, N.A. (2019). rnassqs: An R package to access agricultural data via
  the USDA-NASS Quick Stats API. *Journal of Open Source Software*.
  [doi:10.21105/joss.01880](https://doi.org/10.21105/joss.01880)

Full list on [ORCID](https://orcid.org/0000-0002-3410-3732) and
[Google Scholar](<SCHOLAR-URL-FROM-OLD-CONFIG>).

## Projects

- [rnassqs](https://github.com/ropensci/rnassqs) — R package for the USDA
  NASS Quick Stats API; on [CRAN](https://cran.r-project.org/package=rnassqs)
  since 2019.
- [herdr](https://github.com/econpotter/herdr) — <ONE-LINE-FORK-DESCRIPTION>
```

Two required substitutions, no placeholders may survive:
1. `<SCHOLAR-URL-FROM-OLD-CONFIG>` / `<LINKEDIN-URL-FROM-OLD-CONFIG>`: exact URLs from the old Jekyll config (read before deletion in Task 1; recorded in Task 1's report).
2. `<ONE-LINE-FORK-DESCRIPTION>`: write one plain line describing what the operator's herdr fork is/adds. Derive from `git -C ../herdr log --oneline upstream/master..master 2>/dev/null | head` or the repo's own README; if the delta is unclear, write "terminal multiplexer I use daily, with my own patches" and FLAG it in the task report for operator copy review.
3. Verify the rnassqs repo URL: `gh repo view ropensci/rnassqs --json name 2>/dev/null || gh repo view potterzot/rnassqs --json name` — use whichever exists; keep the CRAN link either way.
4. Author lines on publications: verify author ordering/coauthors against the DOI landing pages; correct "et al." usage if a paper is solo or two-author. Exact titles above came from ORCID and are correct.

- [ ] **Step 2: Render and inspect**

```bash
~/.cache/quarto/quarto-1.6.42/bin/quarto render && ls docs/index.html
grep -c "I publish academic work as Nicholas A. Potter" docs/index.html
```

Expected: render succeeds; grep count ≥ 1.

- [ ] **Step 3: Verify no departure language**

```bash
grep -riE "leaving|departure|transition|launching" docs/index.html && echo "VIOLATION — fix before commit" || echo "clean"
```

Expected: `clean`.

- [ ] **Step 4: Commit (local only)**

```bash
git add index.qmd
git commit -m "feat: identity hub page content"
```

---

### Task 3: Link verification and operator review package

**Files:**
- Create: none (verification + report only)

**Interfaces:**
- Consumes: rendered `docs/` from Task 2.

- [ ] **Step 1: Check every external link resolves**

```bash
cd /home/potterzot/workspace/projects/econpotter.github.io
grep -oE 'href="https?://[^"]+"' docs/index.html | sed 's/href="//;s/"$//' | sort -u | while read -r url; do
  code=$(curl -s -o /dev/null -w "%{http_code}" -L --max-time 15 "$url")
  echo "$code $url"
done
```

Expected: every line 200 (or 403 from bot-hostile hosts like LinkedIn — record and manually spot-check those in a browser-equivalent fetch). Any 404: fix the link and re-render.

- [ ] **Step 2: Verify internal assets**

```bash
ls docs/files/potter-cv.pdf docs/images/ | head
test -f docs/files/potter-resume.pdf && echo "RESUME LEAKED — remove" || echo "resume absent, correct"
```

Expected: CV and images present in output; resume absent.

- [ ] **Step 3: Report for operator review — do not push**

Report to the controller: the local path `docs/index.html` for the operator to open, the Scholar/LinkedIn URLs used, the herdr one-liner (flagged if guessed), the link-check table, and confirmation that nothing was pushed. The operator reviews the rendered page; any copy edits loop back to Task 2.

---

### Task 4: Go-live (ONLY after explicit operator approval of the rendered site)

**Files:**
- Modify: none; push + GitHub Pages source flip.

- [ ] **Step 1: Push master**

```bash
git push origin master
```

- [ ] **Step 2: Point GitHub Pages at master:/docs**

```bash
gh api -X PUT repos/econpotter/econpotter.github.io/pages \
  -f "source[branch]=master" -f "source[path]=/docs"
```

If the API rejects (Pages config variants differ), fall back to:
`gh api repos/econpotter/econpotter.github.io/pages` to inspect current config and adjust the PUT accordingly; report exact commands used.

- [ ] **Step 3: Verify the live site**

```bash
sleep 90
curl -s https://econpotter.github.io | grep -c "I publish academic work as Nicholas A. Potter"
```

Expected: ≥ 1 (may need a retry while Pages rebuilds). Then the operator confirms visually.

- [ ] **Step 4: Document the deploy path in the repo README**

Add (or create `README.md` with) two lines: the site is built with
`quarto render` into `docs/`, and GitHub Pages serves `master:/docs`. Commit
and push:

```bash
git add README.md
git commit -m "docs: note quarto build and pages deploy path"
git push origin master
```

- [ ] **Step 5: Report** — live URL, Pages config used, any anomalies.
