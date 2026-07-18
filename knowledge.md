# Project Knowledge: Template-LaTeX-D3RPLA

## What this is

A repository of **LaTeX templates** for the **D3 Rekayasa Perangkat Lunak Aplikasi (D3RPLA)** program at **Fakultas Ilmu Terapan (FIT), Universitas Telkom, Bandung, Indonesia**.

The LaTeX (`.tex`) files were converted from the original Microsoft Word (`.docx`) templates. The `.docx` source files were obtained from <https://projects.d3ifcool.org/dokumen-pa> and are the property of **D3 Rekayasa Perangkat Lunak Aplikasi (D3RPLA), Telkom University**. They are kept locally as **reference only** and are **gitignored**; they are NOT pushed to the remote repository.

The Tugas Akhir (TA / final project) has **three tracks**, each with its own template:

## Key files (all in project root)

### Tracked in git (pushed to remote)

| File | Purpose |
|------|---------|
| `Template CV.tex` | Curriculum Vitae template for PA (Proyek Akhir) D3RPLA students: identity, SKS summary, projects, certifications, competitions, activities. |
| `Template TA Reguler.tex` | Final Project, **Jalur Reguler** (Regular track). 2 pembimbing (Pembimbing 1 + Reviewer). |
| `Template TA Madusem.tex` | Final Project, **Jalur Magang Dua Semester** (Two-Semester Internship track). 1 pembimbing. |
| `Template TA Publikasi.tex` | Final Project, **Jalur Publikasi** (Publication track). 2 pembimbing. |
| `Template Jurnal.tex` | Journal article template (IEEE-style `IEEEtran` class, with abstract + keywords) for the publication track. |
| `README.md` | Project documentation: template descriptions, compilation instructions, conventions. |
| `.gitignore` | Ignores `*.docx`, LaTeX build artifacts, `.extract/`. |
| `knowledge.md` | This file. |

### Reference only (gitignored, local only; do NOT push)

> **Atribusi:** Semua dokumen `.docx` referensi diambil dari <https://projects.d3ifcool.org/dokumen-pa> dan merupakan milik **D3 Rekayasa Perangkat Lunak Aplikasi (D3RPLA), Telkom University**. File ini hanya dipakai sebagai acuan konversi dan tidak dipush ke repo.

| File | Notes |
|------|-------|
| `Template CV_3.docx` | Original Word source for `Template CV.tex`. |
| `Template TA Reguler_4.docx` | Original Word source (largest, ~5.6 MB, 50 internal parts). |
| `Template TA Madusem_5.docx` | Original Word source. |
| `Template TA Publikasi_6.docx` | Original Word source. |
| `Template Jurnal_7.docx` | Original Word source. |
| `.extract/` | Temporary directory of extracted docx text used during conversion. |

## Build / compile commands

There are no install/dev/test/lint commands; this is a LaTeX document pack. To compile the templates you need a TeX distribution (TeX Live, MiKTeX, or MacTeX). Note: **no LaTeX compiler is installed on this machine** (`pdflatex`/`xelatex`/`latexmk` are absent), so compilation must be done locally by the user.

```bash
# Compile a TA template (two passes needed for TOC + cross-references)
pdflatex "Template TA Reguler.tex"
pdflatex "Template TA Reguler.tex"

# Or use latexmk (handles multi-pass automatically)
latexmk -pdf "Template TA Reguler.tex"

# Template Jurnal.tex uses the IEEEtran class (usually bundled with TeX Live / MiKTeX)
pdflatex "Template Jurnal.tex"
```

## Git repository

- **Remote:** https://github.com/fathahnoor/Template-LaTeX-D3RPLA.git
- **Branch:** `main` (tracks `origin/main`)
- **Author:** fathahnoor <fathah.noor@yahoo.com>
- **Initial commit:** "Initial commit: LaTeX conversion of D3RPLA templates"

```bash
# Stage, commit, push
git add .
git commit -m "Describe change"
git push
```

## Inspecting the .docx reference files

`unzip`, `pandoc`, `libreoffice`, and `docx2txt` are **NOT installed**; only `python3` is available. A `.docx` is a standard ZIP of XML parts; use Python's `zipfile` to inspect it:

```bash
# List internal parts of a .docx
python3 -c "import zipfile; print(zipfile.ZipFile('Template CV_3.docx').namelist())"

# Extract visible text from a .docx (paragraph-aware)
python3 -c "
import zipfile, re, html
z = zipfile.ZipFile('FILE.docx')
d = z.read('word/document.xml').decode('utf-8','ignore')
d = d.replace('</w:p>', '\n')
text = re.sub(r'<[^>]+>', '', d)
print(html.unescape(text))
"
```

## LaTeX conventions & structure

- **Document class:** `report` for the three TA templates; `article` for CV; `IEEEtran` (conference style) for Jurnal.
- **TA front-matter order (matches original docx, differs per track):**
  - Madusem: Cover → Persembahan → Pengesahan → **Pernyataan → Kata Pengantar** → Abstrak → Abstract → TOC
  - Publikasi & Reguler: Cover → Persembahan → Pengesahan → **Kata Pengantar → Pernyataan** → Abstrak → Abstract → TOC
- **Shared front matter:** Cover → Lembar Persembahan → Lembar Pengesahan → (Pernyataan/Kata Pengantar) → Abstrak → Abstract → Daftar Isi/Gambar/Tabel/Lampiran → Bab I–V → Daftar Pustaka → Lampiran.
- **Penomoran:** Gambar/Tabel numbered per chapter (`\counterwithin` from `chngcntr`), e.g. `Gambar 3.1`, `Tabel 2.1`.
- **References:** IEEE style (`thebibliography` with `\bibitem`).
- **Spacing:** Body uses `setspace` `onehalfspacing` (1.5); Abstrak/Abstract use spacing 1.
- **Margins:** `geometry` with 3cm margins (TA templates); 2.5cm (CV).
- **Images:** Placeholder figures use `\fbox{...}`. To use real images, drop files in `assets/` (`\graphicspath{{assets/}}` is set) and replace `\fbox{...}` with `\includegraphics`.

## D3RPLA content conventions

- **Language:** Templates are written in **Bahasa Indonesia** (Indonesian). Section names like *Lembar Pengesahan*, *Kata Pengantar*, *Pernyataan*, *Abstrak* are in Indonesian; titles are bilingual (Indonesian + English). Match this when editing.
- **Institution branding:** Cover pages reference `PROGRAM STUDI D3 REKAYASA PERANGKAT LUNAK APLIKASI`, `FAKULTAS ILMU TERAPAN`, `UNIVERSITAS TELKOM BANDUNG`. Preserve this exact wording and capitalization.
- **NIM format:** D3RPLA NIMs begin with `6706...` / `60706...`. Pembimbing NIPs begin with `078...` / `098...`.
- **Date convention:** *Tanggal Pengesahan* shown as `31 Juli 2025`; update per cohort.
- **Three TA tracks are distinct documents**; do not merge them. Each track has its own cover, signature blocks, and required sections.

## Gotchas

- **`.docx` files are gitignored**; they belong to D3RPLA, Telkom University (sumber: <https://projects.d3ifcool.org/dokumen-pa>) and are reference only. Do not force-add them.
- **No LaTeX compiler on this machine**; cannot validate `.tex` files by compiling here; review LaTeX syntax statically instead.
- **File names contain spaces** (e.g. `Template TA Reguler.tex`). Always quote paths in shell commands.
- **`Template TA Reguler.tex`** requires `amssymb` (for `\checkmark`) and wraps `lstlisting` in a `minipage` inside a tabular cell; both are non-obvious but necessary for compilation.
- **`Template Jurnal.tex`** uses the `IEEEtran` class; ensure it's installed in your TeX distribution.
- **`unzip` is not installed**; use Python's `zipfile` module instead for inspecting `.docx` files.
- **The "_3"…" _7" suffixes** on `.docx` files are version numbers; keep them when adding new reference versions to avoid overwriting prior revisions.
