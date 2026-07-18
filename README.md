# Template-LaTeX-D3RPLA

Template LaTeX untuk dokumen **Program Studi D3 Rekayasa Perangkat Lunak Aplikasi (D3RPLA)**, Fakultas Ilmu Terapan, Universitas Telkom, Bandung.

> ⚠️ **Disclaimer & Atribusi:** Semua dokumen referensi (`.docx`) diambil dari [https://projects.d3ifcool.org/dokumen-pa](https://projects.d3ifcool.org/dokumen-pa) dan merupakan milik **D3 Rekayasa Perangkat Lunak Aplikasi (D3RPLA), Telkom University**. File `.docx` **tidak disertakan** di repositori ini dan hanya digunakan sebagai referensi untuk konversi. Repositori ini berisi hasil konversi ke format LaTeX (`.tex`). Versi LaTeX dibuat untuk kemudahan penulisan dan tidak menggantikan pedoman resmi prodi.

## Daftar Template

| File LaTeX | Kegunaan |
|------------|----------|
| `Template CV.tex` | Curriculum Vitae untuk Proyek Akhir (PA): identitas, SKS, project, sertifikasi, lomba, kegiatan. |
| `Template TA Reguler.tex` | Laporan Tugas Akhir **Jalur Reguler** (2 pembimbing: Pembimbing + Reviewer). |
| `Template TA Madusem.tex` | Laporan Tugas Akhir **Jalur Magang Dua Semester** (1 pembimbing). |
| `Template TA Publikasi.tex` | Laporan Tugas Akhir **Jalur Publikasi** (2 pembimbing). |
| `Template Jurnal.tex` | Artikel jurnal gaya IEEE untuk jalur Publikasi. |

## Cara Menggunakan

### Kebutuhan
- Distribusi LaTeX, mis. [TeX Live](https://www.tug.org/texlive/), [MiKTeX](https://miktex.org/), atau [MacTeX](https://www.tug.org/mactex/).
- Untuk `Template Jurnal.tex` diperlukan kelas `IEEEtran` (umumnya sudah termasuk dalam TeX Live / MiKTeX).

### Kompilasi

```bash
# Contoh untuk TA Reguler (butuh dua kali pass untuk TOC & referensi)
pdflatex "Template TA Reguler.tex"
pdflatex "Template TA Reguler.tex"

# Untuk Template Jurnal (gaya IEEE)
pdflatex "Template Jurnal.tex"

# Atau gunakan latexmk (otomatis multi-pass)
latexmk -pdf "Template TA Reguler.tex"
```

### Struktur Dokumen TA

Ketiga template TA (`Reguler`, `Madusem`, `Publikasi`) memiliki struktur front matter yang serupa:

1. **Halaman Cover**: judul (Indonesia + Inggris), jalur, nama, NIM, prodi, tahun.
2. **Lembar Persembahan**
3. **Lembar Pengesahan**: dengan NIM (`670…` / `60706…`) dan NIP (`078…` / `098…`).
4. **Kata Pengantar** / **Pernyataan** *(urutan bervariasi per jalur, sesuai sumber asli)*
5. **Abstrak** & **Abstract**: abstrak ≤ 250 kata, spacing 1.
6. **Daftar Isi / Gambar / Tabel / Lampiran**
7. **Bab I – V** (isi bab berbeda per jalur)
8. **Daftar Pustaka** (gaya IEEE)
9. **Lampiran**

### Konvensi Penulisan

- Bahasa utama: **Bahasa Indonesia baku**; judul dwibahasa (Indonesia + Inggris).
- Referensi menggunakan **standar IEEE**.
- Penomoran gambar/tabel per bab (mis. `Gambar 3.1`, `Tabel 2.1`).
- NIM D3RPLA diawali `6706…` / `60706…`; NIP pembimbing diawali `078…` / `098…`.
- Branding: `PROGRAM STUDI D3 REKAYASA PERANGKAT LUNAK APLIKASI`, `FAKULTAS ILMU TERAPAN`, `UNIVERSITAS TELKOM BANDUNG`.

## Gambar / Aset

Placeholder gambar pada template ditandai dengan `\fbox{...}`. Untuk menambahkan gambar asli:
1. Letakkan file gambar di folder `assets/` (path sudah diset via `\graphicspath{{assets/}}`).
2. Ganti blok `\fbox{...}` dengan `\includegraphics[width=\linewidth]{nama-file}`.

## Lisensi & Atribusi

**Semua dokumen referensi (`.docx`) diambil dari [https://projects.d3ifcool.org/dokumen-pa](https://projects.d3ifcool.org/dokumen-pa) dan merupakan milik D3 Rekayasa Perangkat Lunak Aplikasi (D3RPLA), Telkom University.**

File `.docx` sumber **tidak disertakan** di repositori ini (telah di-`gitignore`) dan hanya digunakan sebagai referensi untuk konversi ke LaTeX. Versi LaTeX (`.tex`) di repositori ini dibuat untuk kemudahan penulisan dan **tidak menggantikan** pedoman resmi prodi. Selalu rujuk pedoman terbaru dari prodi sebelum mengumpulkan tugas akhir.
