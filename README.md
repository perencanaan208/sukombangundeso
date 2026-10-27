# SUKO mBANGUN DESO — Penampil Vercel

Repo ini **hanya berisi halaman penampil (iframe)**. Seluruh aplikasi, data, login, dan foto
tetap berjalan di **Google Apps Script (GAS)**. Setiap perubahan aplikasi cukup dilakukan di GAS;
repo ini hampir tidak pernah perlu diubah lagi.

```
vercel-suko-mbangun-deso/
├── index.html     ← halaman penampil (ubah GAS_URL sekali saja)
├── vercel.json    ← header keamanan
├── .gitignore
└── README.md      ← panduan ini
```

---

## Bagian 1 — Siapkan Apps Script

1. Buka proyek Apps Script Anda (berisi `Code.gs` dan `Index.html`).
2. Pastikan `doGet()` di `Code.gs` memakai `.setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)`.
   (Sudah ada di kode SUKO mBANGUN DESO. Tanpa ini, Google menolak ditampilkan di iframe.)
3. Klik **Deploy → Kelola deployment** (atau **Deploy baru** jika belum pernah):
   - Jenis: **Aplikasi web**
   - Jalankan sebagai: **Saya**
   - Yang memiliki akses: **Siapa saja** (bukan "Siapa saja dengan akun Google")
4. Salin **URL aplikasi web**. Bentuknya:
   `https://script.google.com/macros/s/AKfycb.../exec`
   Harus berakhiran **`/exec`** (bukan `/dev`).

## Bagian 2 — Isi URL di `index.html`

> URL deployment Anda **sudah terisi** di `index.html`. Bagian ini hanya perlu dilakukan jika URL deployment berubah.

Buka `index.html`, cari baris ini lalu ganti dengan URL baru:

```js
var GAS_URL = 'https://script.google.com/macros/s/GANTI_DENGAN_ID_DEPLOYMENT_ANDA/exec';
```

## Bagian 3 — Unggah ke GitHub

Cara paling mudah (tanpa perintah):
1. Masuk ke github.com → **New repository** → nama, misalnya `suko-mbangun-deso` → **Create repository**.
2. Klik **uploading an existing file**, tarik semua isi folder ini (`index.html`, `vercel.json`,
   `.gitignore`, `README.md`) → **Commit changes**.

Atau lewat terminal (sudah terpasang Git):

```bash
cd vercel-suko-mbangun-deso
git init
git add .
git commit -m "Penampil SUKO mBANGUN DESO"
git branch -M main
git remote add origin https://github.com/USERNAME_ANDA/suko-mbangun-deso.git
git push -u origin main
```

## Bagian 4 — Hubungkan ke Vercel

1. Masuk ke vercel.com (boleh dengan akun GitHub) → **Add New… → Project**.
2. Pilih repo `suko-mbangun-deso` → **Import**.
3. Pengaturan:
   - Framework Preset: **Other**
   - Build Command: kosongkan
   - Output Directory: kosongkan
4. Klik **Deploy**. Dalam beberapa detik Anda mendapat alamat seperti
   `https://suko-mbangun-deso.vercel.app`.
5. (Opsional) Domain sendiri: **Settings → Domains → Add**, lalu ikuti petunjuk DNS.

## Bagian 5 — Cara memperbarui aplikasi (penting)

| Yang diubah | Di mana | Perlu redeploy Vercel? |
|---|---|---|
| Tampilan, fitur, aturan, kode | Apps Script (`Code.gs` / `Index.html`) | **Tidak** |
| Alamat deployment GAS berubah | `GAS_URL` di `index.html` | Ya (otomatis setelah commit) |

Langkah memperbarui di Apps Script:

1. Edit kode, simpan.
2. **Deploy → Kelola deployment → ikon pensil (Edit) → Versi: Versi baru → Deploy.**
3. Muat ulang alamat Vercel. Perubahan langsung tampil.

> Jangan pilih **Deploy baru** setiap kali memperbarui: itu menghasilkan URL baru dan
> `GAS_URL` harus diganti. Selalu **edit deployment yang ada** supaya URL tetap sama.

## Pemecahan masalah

| Gejala | Penyebab dan solusi |
|---|---|
| Kotak kosong / "script.google.com refused to connect" | `doGet()` belum `ALLOWALL`, atau memakai URL `/dev`. Pakai URL `/exec` dan deploy versi baru. |
| Diminta login akun Google | Akses deployment bukan **Siapa saja**. Ubah di Kelola deployment. |
| Muncul banner "Aplikasi ini dibuat oleh pengguna Google Apps Script" | Batasan Google untuk akun Gmail biasa; tidak bisa dihilangkan dari sisi Vercel. Akun Google Workspace tidak menampilkannya. |
| Login hilang setiap muat ulang halaman | Browser membatasi penyimpanan di dalam iframe. Aplikasi tetap berjalan; pengguna cukup masuk lagi. Jika ingin sesi bertahan, buka URL GAS langsung (tombol "Buka langsung"). |
| Halaman berubah tapi Vercel masih versi lama | Anda belum membuat **Versi baru** di Kelola deployment GAS. |
| Foto tidak tampil | Pastikan akun Drive tidak memblokir berbagi tautan; lihat catatan foto di aplikasi. |

## Catatan keamanan

- Vercel hanya menampilkan halaman; **login dan data tetap diatur oleh Apps Script**.
- Jangan menaruh kata sandi, token, atau ID Spreadsheet di repo ini. Repo hanya berisi URL publik aplikasi.
- `vercel.json` membatasi iframe hanya ke domain Google dan menambah header keamanan dasar.
