# Advanced Aerospace Systems Engineering Lab

Dashboard belajar interaktif berbasis materi. Unggah satu file PDF materi kuliah, lalu lab mengolahnya menjadi ringkasan, kalkulator, peta relasi, studi kasus, kuis, dan glosarium. Semuanya satu file HTML, tanpa build.

Materi contoh bawaan: **TD25602 Bab 3, Requirement Analysis and Systems Architecture** (requirement capture, decomposition, allocation, verification criteria, traceability).

## Fitur

| Tab | Isi |
|---|---|
| Dashboard | Statistik, progres keseluruhan, roadmap belajar per bagian (status tersimpan di browser) |
| Ringkasan | Poin-poin tiap bagian materi dan tabel pendukung |
| Lab Hitung | Kalkulator dari rumus di materi, dihitung JavaScript secara langsung |
| Peta Relasi | Klik satu elemen untuk melihat elemen lain yang terdampak bila elemen itu berubah |
| Studi Kasus | Contoh soal beserta langkah penyelesaian |
| Kuis | Soal pilihan ganda dengan penjelasan |
| Glosarium | Istilah dengan pencarian |
| Input Materi | Unggah PDF/.txt/.md, proses dengan Claude, ekspor/impor hasil (JSON) |

## Cara pakai

1. Buka tab **Input Materi**.
2. Pilih file PDF (teksnya dibaca otomatis) atau tempel teks manual.
3. Klik **Proses materi** dan tunggu 1-2 menit.
4. Semua tab terisi dari materi tersebut.

Batasan: PDF hasil scan (gambar) tidak terbaca, dan teks lebih dari 60.000 karakter dipotong.

### Mode pemrosesan

- **Di claude.ai** (lewat link artifact): tidak perlu API key.
- **Di GitHub Pages / file lokal**: isi **API key Anthropic** di tab Input Materi. Key dikirim langsung dari browser ke `api.anthropic.com` dan disimpan di `localStorage` browser sendiri.
- **Tanpa API key**: gunakan **Impor hasil** untuk memuat file `.json` yang sudah diproses orang lain.

## Perpustakaan materi (tanpa API key)

Lab di GitHub Pages otomatis membaca folder `materi/` dan menampilkan dropdown **Pilih materi** di bagian atas. Tidak perlu upload dan tidak perlu API key.

Menambah materi baru:

1. Proses materi di link claude.ai artifact (gratis lewat akun Claude lo), lalu klik **Ekspor hasil (JSON)**.
2. Taruh file JSON di folder `materi/`, mis. `bab-4-xxx.json`.
3. Tambahkan satu baris di `materi/index.json`:

```json
[
  { "nama": "Bab 3 - Requirement Analysis and Systems Architecture", "file": "bab-3-requirement-analysis.json" },
  { "nama": "Bab 4 - ...", "file": "bab-4-xxx.json" }
]
```

4. Commit dan push. Dropdown akan menampilkan materi baru.

Jika `materi/index.json` tidak ada (mis. di claude.ai), dropdown disembunyikan dan lab memakai materi contoh bawaan.

## Deploy ke GitHub Pages

1. Buat repo baru, mis. `aerospace-se-lab`.
2. Unggah file HTML dengan nama `index.html`, plus `README.md` ini.
3. Settings → Pages → Source: *Deploy from a branch* → branch `main`, folder `/ (root)` → Save.
4. Buka `https://<username>.github.io/aerospace-se-lab/`.

Struktur repo:

```
aerospace-se-lab/
├── index.html
├── README.md
├── materi/
│   ├── index.json
│   └── bab-3-requirement-analysis.json
```

## Keamanan API key

- Jangan menulis API key di dalam file HTML atau meng-commit-nya ke repo.
- Gunakan key khusus dengan batas pemakaian, dan jangan dipakai di komputer umum.
- Setiap pemrosesan materi berbayar sesuai pemakaian API.

## Format file hasil (JSON)

File hasil ekspor memakai struktur berikut (semua bagian opsional kecuali `bagian`):

```json
{
  "judul": "string",
  "sub": "string",
  "bagian": [{ "judul": "string", "poin": ["string"] }],
  "tabel": [{ "judul": "string", "kolom": ["string"], "baris": [["string"]] }],
  "calc": [{ "nama": "string", "in": [["id", "label", 0]], "out": [["label", "ekspresi"]] }],
  "relasi": [{ "id": "string", "to": ["string"] }],
  "contoh": [{ "judul": "string", "kasus": "string", "langkah": ["string"], "kes": "string" }],
  "kuis": [{ "q": "string", "o": ["a", "b", "c"], "a": 0, "e": "string" }],
  "gl": [["istilah", "definisi"]]
}
```

Ekspresi di `calc.out` adalah aritmetika JavaScript yang memakai `id` input, boleh memakai `?:` dan fungsi `Math`. Hanya karakter aritmetika yang diizinkan.

## Teknologi

HTML, CSS, dan JavaScript murni. PDF dibaca dengan [pdf.js](https://mozilla.github.io/pdf.js/) dari cdnjs. Pemrosesan materi memakai Claude (Anthropic API).

## Catatan

- Kalkulator "Dampak perubahan durasi" pada materi contoh adalah ilustrasi (angka daya, efisiensi, dan cadangan hanyalah contoh), bukan rumus dari materi.
- Hasil olahan AI perlu diperiksa terhadap materi aslinya sebelum dipakai belajar atau dikutip.
