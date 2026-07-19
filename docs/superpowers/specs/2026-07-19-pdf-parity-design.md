# PDF Parity Design

## Goal

Make the PDFs compiled from the five root-level LaTeX templates visually match their corresponding reference PDFs in `PDF Exports/From DOCX/` while keeping the templates editable LaTeX documents.

## Reference Pairs

| LaTeX source | Canonical reference PDF |
|---|---|
| `Template CV.tex` | `PDF Exports/From DOCX/Template CV_3.pdf` |
| `Template Jurnal.tex` | `PDF Exports/From DOCX/Template Jurnal_7.pdf` |
| `Template TA Madusem.tex` | `PDF Exports/From DOCX/Template TA Madusem_5.pdf` |
| `Template TA Publikasi.tex` | `PDF Exports/From DOCX/Template TA Publikasi_6.pdf` |
| `Template TA Reguler.tex` | `PDF Exports/From DOCX/Template TA Reguler_4.pdf` |

## Approach

Start from the calibrated formatting recorded in the latest repository commit because the current local TEX changes remove precision settings for fonts, margins, headers, paragraph spacing, and image dimensions. Preserve editable LaTeX content and then iteratively refine formatting based on rendered page comparisons.

The implementation must not embed complete reference PDF pages as images. Reference DOCX and PDF files remain local comparison inputs and must not be committed.

## Formatting Scope

For each pair, align:

- paper size, margins, and text area;
- font family, size, weight, and line spacing;
- paragraph spacing, indentation, alignment, and page breaks;
- headers, footers, page numbering, and decorative bands;
- heading hierarchy and capitalization;
- tables, captions, lists, and code blocks;
- image selection, dimensions, aspect ratio, and placement;
- page count and content flow.

## Validation

Compile every TEX source with the appropriate available LaTeX engine and enough passes to stabilize tables of contents, references, and numbering. Compare each generated PDF against its canonical reference page by page using page metadata, rasterized visual differences, and targeted inspection of pages with substantial differences.

Success means all five documents compile without fatal errors, match the reference page structure and content placement, and have no unexplained material visual differences. Byte-for-byte PDF equality is not required because Word and LaTeX produce different PDF internals.

## Repository Hygiene

Commit only intended source, asset, documentation, and validation-support changes. Do not commit DOCX files, generated PDFs, LaTeX build artifacts, `.freebuff/`, or `.kinaleaf/`. Before pushing, inspect status and diff, run final compilation and comparison checks, and verify the pushed branch tracks `origin/main`.
