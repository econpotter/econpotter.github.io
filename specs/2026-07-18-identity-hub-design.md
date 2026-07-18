# Identity hub — site rebuild spec

Date: 2026-07-18. Status: approved by operator 2026-07-18.

## Job of the site

econpotter.github.io is a professional identity hub: a single page that lets a
first-time professional visitor learn who Nicholas Potter is, what he does,
and where the proof lives — in under a minute. It is not a blog, an academic
CV shell, or a company site. Publishing belongs to future spokes (Substack);
company/product pages get their own homes later.

Identity architecture (ratified 2026-07-18 in conversation): byline
**Nicholas Potter** (publishes academically as Nicholas A. Potter); handle
**econpotter** across domain and platforms; company branding is a separate,
deferred decision.

## Platform: rebuild in Quarto

Replace the Jekyll/academicpages site with a fresh Quarto website in this
repository. Rationale (operator-confirmed): the site's footprint after content
decisions is one page plus a CV PDF and a projects list — a clean Quarto
rebuild is less work than gutting the academic theme, and Quarto is the
operator's native toolchain (R, .qmd, existing render workflow).

- Keep the repository and branch (`master`); replace site source.
- Deploy via GitHub Pages (gh-pages branch or GitHub Action, implementer's
  choice; document the choice in the repo README).
- Preserve in git history (no force-push); old Jekyll source is deleted in the
  rebuild commit, not archived in the working tree.
- Custom domain: when the operator has purchased econpotter.com, add the
  `CNAME` file. Do not add before.
- Keep the existing portrait image and CV PDF. Drop the resume PDF from the
  site (CV is canonical; resumes are tailored per application, off-site).

## Page structure (single page)

1. **Identity block** — portrait + this ratified paragraph, verbatim:

   > I'm an economist working on water, climate, and agriculture in the
   > western United States. I'm a research economist at USDA's Economic
   > Research Service, where my work focuses on water supplies, drought, and
   > irrigated agriculture. Outside of that role I build tools that make
   > water and climate risk easier to understand: parsing water-right records
   > and their case histories, forecasting seasonal water supply, and
   > estimating the regional economic consequences of change in agriculture
   > and natural resources. I publish academic work as Nicholas A. Potter.

   The three tool clauses become links (AIDD, water-forecast, agio) only as
   each goes public; plain text until then.

2. **Selected publications** — 3-4 peer-reviewed papers, DOI/publisher links,
   populated from the operator's real record (ORCID 0000-0002-3410-3732 /
   Google Scholar). One closing line: "Full list on ORCID and Google
   Scholar." No separate publications page.

3. **Projects** — public repositories only, one line each (what it is + link):
   - **rnassqs** — R package for the USDA NASS Quick Stats API; on CRAN since
     2019. (CRAN + GitHub links.)
   - **herdr** (fork) — one line on what the operator's fork adds.
   - Section grows as further repos become public (orchestra, openbrain,
     agentic-researcher, agio-engine at its release). Never list a private or
     placeholder repo.

4. **Link rail** — GitHub (econpotter) · Google Scholar · ORCID (corrected
   URL) · LinkedIn · CV (PDF). Spokes (Substack, X, YouTube, product pages)
   are added only when live.

No nav bar; a single page with anchors at most. No "Now" section (everything
is alpha; the identity paragraph carries the generic work summary).

## Constraints

- **No departure language.** The site must not say or imply the operator is
  leaving USDA. USDA role and independent tool-building are both stated, as
  in the ratified paragraph.
- No dead links, placeholder sections, or empty collections anywhere.
- Match the operator's writing style (`~/.config/agents/references/
  writing-style.md`) for any connective copy beyond the ratified paragraph;
  keep such copy minimal and present it for review before merge.

## Out of scope

- Making orchestra/openbrain/agentic-researcher public (separate per-repo
  tasks: secrets/path scan, README, license, then flip; each task proposed
  individually).
- Substack, X, YouTube account creation; domain purchase; company pages.
- New articles or blog machinery.
- The new professional photo (TODO item) — launch with the existing portrait,
  swap later.

## Acceptance

- Quarto site builds cleanly (`quarto render`) and deploys on GitHub Pages.
- Single page matching the structure above; ratified paragraph verbatim.
- Every link resolves; ORCID link corrected; no Jekyll remnants served.
- Projects lists exactly rnassqs and herdr at launch.
- No departure language.
- Operator reviews rendered site before it replaces the live one.
