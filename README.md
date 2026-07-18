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
- Untuk `Template Jurnal.tex` diperlukan kelas `IEEEtran` (paket `texlive-publishers`)
  serta paket `algorithmic` (paket `texlive-science` / bundle `algorithms`).
  Keduanya umumnya sudah termasuk dalam instalasi TeX Live / MiKTeX yang lengkap.

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

Semua gambar contoh telah diekstrak dari dokumen `.docx` referensi dan disediakan
di folder `assets/` sehingga PDF hasil kompilasi LaTeX sudah menampilkan gambar
yang sama persis dengan PDF dari Word. Struktur folder:

```
assets/
├── reguler/     # 23 gambar untuk Template TA Reguler (cover, header logo, 13 figure, tabel IoT, 4 foto lampiran)
├── madusem/     # 3 gambar untuk Template TA Madusem (cover, header logo, logo Telkom)
├── publikasi/   # 3 gambar untuk Template TA Publikasi (cover, header logo, logo Telkom)
├── jurnal/      # 4 gambar untuk Template Jurnal (arsitektur, struktur SQLite, 2 implementasi)
└── cv/          # 1 gambar untuk Template CV (pas foto)
```

Setiap template sudah men-set `\graphicspath` ke subfoldernya masing-masing, jadi
`\includegraphics{nama-file}` (tanpa path) langsung menemukan gambarnya. Untuk
mengganti gambar contoh dengan gambar kamu sendiri, cukup timpa file di folder
`assets/<template>/` yang sesuai dengan nama yang sama.

> **Catatan konversi:** Beberapa gambar pada `.docx` asli berformat EMF dan TIF
> (tidak didukung `pdflatex`). File-file tersebut telah dikonversi ke PNG dan
> diberi akhiran nama yang sama (tanpa label `_converted`). Semua gambar di
> folder `assets/` sudah dalam format yang didukung LaTeX (PNG/JPG).
>
> **Atribusi gambar:** Sama seperti file `.docx`, semua gambar di folder `assets/`
> **diambil dari dokumen referensi milik D3RPLA, Telkom University**
> (sumber: <https://projects.d3ifcool.org/dokumen-pa>) dan hanya disertakan
> sebagai **contoh** agar hasil kompilasi LaTeX menampilkan gambar yang sama
> dengan Word. Gambar-gambar ini wajib diganti dengan gambar milik kamu sendiri
> saat menyusun Tugas Akhir. Pencantuman gambar contoh di repo ini tidak
> menggantikan pedoman resmi prodi.

## Lisensi & Atribusi

**Semua dokumen referensi (`.docx`) diambil dari [https://projects.d3ifcool.org/dokumen-pa](https://projects.d3ifcool.org/dokumen-pa) dan merupakan milik D3 Rekayasa Perangkat Lunak Aplikasi (D3RPLA), Telkom University.**

File `.docx` sumber **tidak disertakan** di repositori ini (telah di-`gitignore`) dan hanya digunakan sebagai referensi untuk konversi ke LaTeX. Versi LaTeX (`.tex`) di repositori ini dibuat untuk kemudahan penulisan dan **tidak menggantikan** pedoman resmi prodi. Selalu rujuk pedoman terbaru dari prodi sebelum mengumpulkan tugas akhir.
