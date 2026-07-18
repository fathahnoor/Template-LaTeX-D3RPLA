# Project Knowledge — Template-LaTeX-D3RPLA

## What this is

A repository of **official document templates** for the **D3 Rekayasa Perangkat Lunak Aplikasi (D3RPLA)** program at **Fakultas Ilmu Terapan (FIT), Universitas Telkom, Bandung, Indonesia**.

> ⚠️ **Gotcha:** The directory name says "LaTeX", but the repository contains **Microsoft Word `.docx` files only** — there is no LaTeX source, no `.tex` files, and no build tooling. Treat this as a Word-document template pack, not a LaTeX project.

The Tugas Akhir (TA / final project) has **three tracks**, each with its own template:

## Key files (all in project root)

| File | Purpose |
|------|---------|
| `Template CV_3.docx` | Curriculum Vitae template for PA (Perwalian Akademik) D3RPLA students — identity, SKS summary, projects, certifications, competitions, activities. |
| `Template TA Reguler_4.docx` | Final Project — **Jalur Reguler** (Regular track). Largest file (~5.6 MB, 50 internal parts, many images). |
| `Template TA Madusem_5.docx` | Final Project — **Jalur Magang Dua Semester** (Two-Semester Internship track). |
| `Template TA Publikasi_6.docx` | Final Project — **Jalur Publikasi** (Publication track). |
| `Template Jurnal_7.docx` | Journal article template (IEEE-style, with abstract + keywords) for the publication track. |

All TA templates share common structure: Cover → Lembar Persembahan → Lembar Pengesahan → Pernyataan → Kata Pengantar → body. TA Reguler & Publikasi have two pembimbing (advisors); Madusem has one.

## Commands to run

There are **no install / dev / test / lint / build commands** — this is a document pack, not a codebase. There is no `package.json`, `Makefile`, `README`, or git repository.

Useful utilities if you need to inspect or convert the `.docx` files (note: `unzip` is **not** installed on this machine, but `python3` is available):

```bash
# List / extract internal parts of a .docx (docx == zip archive)
python3 -c "import zipfile; print(zipfile.ZipFile('Template CV_3.docx').namelist())"

# Extract visible text from a .docx
python3 -c "import zipfile,re; d=zipfile.ZipFile('FILE.docx').read('word/document.xml').decode('utf-8','ignore'); print(re.sub(r'\s+',' ',re.sub(r'<[^>]+>',' ',d)).strip())"

# Convert docx → other formats (if pandoc/libreoffice are installed)
pandoc "Template TA Reguler_4.docx" -o output.md
libreoffice --headless --convert-to pdf "Template Jurnal_7.docx"
```

## Conventions & constraints

- **Language:** Templates are written in **Bahasa Indonesia** (Indonesian). Section names like *Lembar Pengesahan*, *Kata Pengantar*, *Pernyataan*, *Abstrak* are in Indonesian; titles are bilingual (Indonesian + English). Match this when editing.
- **Institution branding:** Cover pages reference `PROGRAM STUDI D3 REKAYASA PERANGKAT LUNAK APLIKASI`, `FAKULTAS ILMU TERAPAN`, `UNIVERSITAS TELKOM BANDUNG`. Preserve this exact wording and capitalization.
- **NIM format:** D3RPLA NIMs begin with `6706...` / `60706...` (seen in templates). Pembimbing NIPs begin with `078...` / `098...`.
- **Date convention:** *Tanggal Pengesahan* shown as `31 Juli 2025` — update per cohort.
- **Embedded media:** TA templates embed `.png`, `.tif`, and `.emf` images (logos, signature placeholders, figure examples) under `word/media/`. Don't strip these when re-zipping a modified `.docx`.
- **File names contain spaces** (e.g. `Template TA Reguler_4.docx`). Always quote paths in shell commands.
- **Three TA tracks are distinct documents** — do not merge them. Each track has its own cover, signature blocks, and required sections.

## Gotchas

- **No git** — `git status` fails ("not a git repository"). Initialize one before versioning edits if needed.
- **`unzip` is not installed** — use Python's `zipfile` module instead (a `.docx` is a standard ZIP of XML parts).
- **Doc text-extraction tools** (`docx2txt`, `pandoc`) are **not installed** by default; only `python3` is guaranteed. Roll your own XML extraction (see snippets above).
- **Editing `.docx` programmatically** requires preserving the OOXML zip structure (`[Content_Types].xml`, `_rels`, `word/`). Editing `word/document.xml` in place and re-zipping with the same structure is the safest low-tech approach; `python-docx` is preferable if installable.
- **The "_3"…" _7" suffixes** are version numbers — keep them when adding new versions to avoid overwriting prior revisions.
