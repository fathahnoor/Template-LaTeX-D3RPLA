# PDF Parity Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make all five LaTeX templates compile to PDFs that visually match the canonical Word-exported PDFs.

**Architecture:** Treat each DOCX-exported PDF as an immutable visual baseline. Restore the calibrated LaTeX formatting removed by the current working-tree changes, compile every source, rasterize both PDFs with Ghostscript, and iteratively correct only pages with material visual differences.

**Tech Stack:** LaTeX (`pdflatex` or `xelatex`), Ghostscript 10.06, Python 3.14 with `pypdf`, Pillow, and NumPy, Git.

## Global Constraints

- Canonical references are the five PDFs under `PDF Exports/From DOCX/`.
- Keep all five outputs as editable LaTeX; do not embed complete reference pages.
- Byte-for-byte PDF equality is not required because Word and LaTeX use different PDF internals.
- DOCX files, generated PDFs, LaTeX artifacts, `.freebuff/`, and `.kinaleaf/` must not be committed.
- Preserve unrelated user changes unless they directly conflict with visual parity.

---

### Task 1: Establish the Render and Comparison Environment

**Files:**
- Inspect: `PDF Exports/From DOCX/*.pdf`
- Inspect: `PDF Exports/From TEX/*.pdf`
- Modify only if needed: `.gitignore`

**Interfaces:**
- Consumes: five canonical reference PDFs and five LaTeX sources.
- Produces: a working LaTeX compiler command and page-level raster comparisons.

- [ ] **Step 1: Confirm installed commands**

Run:

```powershell
$commands = 'pdflatex','xelatex','lualatex','latexmk','gswin64c','python'
foreach ($name in $commands) {
  $cmd = Get-Command $name -ErrorAction SilentlyContinue
  if ($cmd) { "$name=$($cmd.Source)" } else { "$name=MISSING" }
}
```

Expected: Ghostscript and Python resolve; install a LaTeX distribution if all LaTeX engines are missing.

- [ ] **Step 2: Ensure generated comparison material remains untracked**

The effective `.gitignore` must contain:

```gitignore
/*.pdf
PDF Exports/
.compare/
```

Expected: `git status --short --ignored` marks reference PDFs and generated comparison files as ignored while leaving the files available locally.

- [ ] **Step 3: Record baseline page counts**

Run:

```powershell
python -c "from pathlib import Path; from pypdf import PdfReader; [print(f'{p}: {len(PdfReader(str(p)).pages)}') for p in sorted(Path('PDF Exports').rglob('*.pdf'))]"
```

Expected canonical counts: CV 2, Jurnal 3, Madusem 24, Publikasi 20, Reguler 44.

- [ ] **Step 4: Verify rasterization**

Run:

```powershell
& 'C:\Program Files\gs\gs10.06.0\bin\gswin64c.exe' -dSAFER -dBATCH -dNOPAUSE -sDEVICE=png16m -r144 -dFirstPage=1 -dLastPage=1 -sOutputFile="$env:TEMP\pdf-parity-%03d.png" 'PDF Exports\From DOCX\Template CV_3.pdf'
```

Expected: Ghostscript exits 0 and creates `%TEMP%\pdf-parity-001.png`.

### Task 2: Restore the Calibrated Shared Formatting

**Files:**
- Modify: `Template CV.tex`
- Modify: `Template Jurnal.tex`
- Modify: `Template TA Madusem.tex`
- Modify: `Template TA Publikasi.tex`
- Modify: `Template TA Reguler.tex`

**Interfaces:**
- Consumes: calibrated values from commit `9226c45` and existing assets under `assets/`.
- Produces: five syntactically valid sources using the reference font, page geometry, spacing, headers, and image dimensions.

- [ ] **Step 1: Restore font and page geometry**

Apply these exact policies:

```latex
% TA and journal documents
\usepackage{iftex}
\ifPDFTeX
  \usepackage{tgtermes}
\else
  \usepackage{fontspec}
  \IfFontExistsTF{Times New Roman}{\setmainfont{Times New Roman}}{%
    \IfFontExistsTF{TeX Gyre Termes}{\setmainfont{TeX Gyre Termes}}{}}
\fi
```

Use `11pt` for the three TA documents, TA geometry `left=4cm,right=3cm,top=3cm,bottom=4cm`, and `\setstretch{1.15}` with zero paragraph indent and 10 pt paragraph skip. Use Calibri/Carlito/TeX Gyre Heros, 3 cm margins, `\setstretch{1.08}`, zero indent, and 8 pt paragraph skip for CV.

Expected: the preambles match the calibrated values in commit `9226c45` rather than the current generic 12 pt/3 cm/1.5-spacing settings.

- [ ] **Step 2: Restore TA header and heading calibration**

Use a full-width `#A5A5A5` header band ending 1.98 cm below the page top, with `header-logo` at 0.69 cm high and no running chapter text or header rule. Use 20/24 pt chapter titles, 16/19 pt sections, 14/17 pt subsections, and 12/14 pt subsubsections.

Expected: TA pages reproduce the Word gray header band and title hierarchy.

- [ ] **Step 3: Restore front-matter numbering and calibrated image dimensions**

Start Roman numbering before the title page, activate the header band after the title page, preserve title case used by the reference headings, and reset Arabic numbering explicitly at the first numbered chapter. Restore all explicit image widths and heights from commit `9226c45`, including the cover banners, CV photo, journal figures, TA diagrams, UI tables, IoT table, test image, and appendix photos.

Expected: no current generic values such as `height=3cm`, `width=0.8\textwidth`, or `height=7cm` remain where commit `9226c45` supplies a measured reference dimension.

- [ ] **Step 4: Run static checks**

Run:

```powershell
git diff --check -- '*.tex'
git diff --word-diff=porcelain 9226c45 -- '*.tex'
```

Expected: no whitespace errors; remaining differences from `9226c45` are intentional parity refinements only.

### Task 3: Compile and Correct Each Reference Pair

**Files:**
- Modify: `Template CV.tex`
- Modify: `Template Jurnal.tex`
- Modify: `Template TA Madusem.tex`
- Modify: `Template TA Publikasi.tex`
- Modify: `Template TA Reguler.tex`
- Generate, ignored: root-level `*.pdf`, `*.aux`, `*.log`, `*.toc`, `*.lof`, `*.lot`
- Generate, ignored: `.compare/`

**Interfaces:**
- Consumes: sources from Task 2 and canonical PDFs.
- Produces: five stable generated PDFs with matching page counts and minimized raster differences.

- [ ] **Step 1: Compile all documents to convergence**

Run the selected engine three times for each TA/CV source and twice for the journal source. With `pdflatex`:

```powershell
$sources = 'Template CV.tex','Template Jurnal.tex','Template TA Madusem.tex','Template TA Publikasi.tex','Template TA Reguler.tex'
foreach ($source in $sources) {
  1..3 | ForEach-Object { pdflatex -interaction=nonstopmode -halt-on-error $source }
}
```

Expected: all invocations exit 0; logs contain no fatal errors or undefined references after the final pass.

- [ ] **Step 2: Match page counts before pixel tuning**

Run the `pypdf` page-count command from Task 1 against root PDFs and reference PDFs.

Expected: CV 2, Jurnal 3, Madusem 24, Publikasi 20, Reguler 44. Correct pagination first by adjusting paragraph spacing, float dimensions, and explicit page breaks; do not shrink the whole document indiscriminately.

- [ ] **Step 3: Rasterize each pair at 144 DPI**

For each pair, run Ghostscript with `-sDEVICE=png16m -r144` into separate `.compare/<name>/reference/` and `.compare/<name>/actual/` directories.

Expected: both directories contain one same-sized PNG per PDF page.

- [ ] **Step 4: Calculate page-level visual error**

For each same-numbered PNG pair, calculate normalized mean absolute RGB error with Pillow and NumPy:

```python
from pathlib import Path
import numpy as np
from PIL import Image

reference = np.asarray(Image.open(reference_path).convert("RGB"), dtype=np.int16)
actual = np.asarray(Image.open(actual_path).convert("RGB"), dtype=np.int16)
assert reference.shape == actual.shape
mae = np.abs(reference - actual).mean() / 255
print(page_number, f"{mae:.6f}")
```

Expected: a ranked list identifies the pages needing correction; no page-size mismatch is accepted.

- [ ] **Step 5: Correct high-error pages iteratively**

Change the smallest responsible LaTeX setting in this order: page geometry/font, header/footer, explicit breaks, float dimensions, table widths, then local vertical spacing. Recompile the affected source and repeat Steps 2-4 after each correction.

Expected: page counts remain canonical and each correction reduces rather than increases visual error. Differences caused only by font rasterization or PDF antialiasing may remain.

### Task 4: Final Verification, Commit, and Push

**Files:**
- Verify: all five root-level TEX files
- Verify: `.gitignore`
- Verify: `README.md` only if compilation instructions changed
- Exclude: all reference and generated artifacts

**Interfaces:**
- Consumes: corrected templates and comparison results.
- Produces: reviewed commits pushed to `origin/main`.

- [ ] **Step 1: Run final verification from clean artifacts**

Delete only ignored LaTeX build artifacts and `.compare/`, rebuild all five documents, then repeat page-count and raster-error checks.

Expected: all five compile successfully, page counts match 2/3/24/20/44, and no material unexplained visual mismatch remains.

- [ ] **Step 2: Inspect repository state**

Run:

```powershell
git status --short
git diff --check
git diff --stat
git diff -- '*.tex' '.gitignore' 'README.md'
git log --oneline -10
```

Expected: only intended source/documentation changes are present; DOCX, PDF, `.freebuff/`, `.kinaleaf/`, and build artifacts are absent from the staged set.

- [ ] **Step 3: Commit intended files**

Stage explicit paths only and commit:

```powershell
git add -- '.gitignore' 'README.md' 'Template CV.tex' 'Template Jurnal.tex' 'Template TA Madusem.tex' 'Template TA Publikasi.tex' 'Template TA Reguler.tex' 'docs/superpowers/plans/2026-07-19-pdf-parity.md'
git diff --cached --check
git diff --cached --stat
git commit -m 'Samakan hasil PDF LaTeX dengan referensi DOCX'
```

Expected: commit succeeds without including local reference or generated files.

- [ ] **Step 4: Push and verify remote state**

Run:

```powershell
git push origin main
git status --short --branch
git log --oneline origin/main -3
```

Expected: push succeeds, local `main` is aligned with `origin/main`, and the new parity commit is the remote tip.
