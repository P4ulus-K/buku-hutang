# Buku Hutang Piutang (versi mandiri): panduan pemasangan

Aplikasi ini berjalan sendiri di browser, tanpa claude.ai.

- **Catatan dan foto bukti** disimpan di iPhone (penyimpanan browser).
- **Scan bukti transfer** dibaca langsung di iPhone dengan OCR. Internet hanya dibutuhkan sekali, untuk mengunduh pembaca teks (±10 MB).
- **Google Drive** menyimpan salinan di folder `Buku Hutang Piutang` sebagai backup dan agar bisa dibuka di perangkat lain.
- **Kata sandi dan kode pemulihan** bekerja seperti di versi sebelumnya.

Isi folder:

| File | Gunanya |
|---|---|
| `index.html` | Aplikasinya |
| `sw.js` | Agar aplikasi tetap terbuka saat offline |
| `manifest.webmanifest`, `icon-*.png`, `apple-touch-icon.png` | Ikon dan tampilan saat dipasang di Layar Utama |

Pemasangan dilakukan sekali saja, sekitar 20–30 menit. Paling mudah dikerjakan dari laptop atau komputer.

---

## Langkah 1: Taruh aplikasi di internet (GitHub Pages, gratis)

Google hanya mengizinkan login dari alamat `https://`, jadi file-file ini perlu di-hosting.

1. Buat akun di **github.com** jika belum punya.
2. Klik **+** (kanan atas) → **New repository**.
   - Repository name: `buku-hutang`
   - Pilih **Public**. GitHub Pages gratis hanya untuk repo publik. Yang publik hanya kode aplikasinya; catatan dan foto Anda **tidak** ada di sana.
   - Klik **Create repository**.
3. Klik **uploading an existing file**, lalu seret **semua file** dari folder ini (index.html, sw.js, manifest.webmanifest, ketiga file .png). Klik **Commit changes**.
4. Buka **Settings** → **Pages**. Pada *Source* pilih **Deploy from a branch**, branch **main**, folder **/ (root)**, lalu **Save**.
5. Tunggu 1–2 menit. Alamat aplikasi Anda:
   `https://NAMA-AKUN-GITHUB.github.io/buku-hutang/`

Catat **origin**-nya, yaitu alamat tanpa path: `https://NAMA-AKUN-GITHUB.github.io`

## Langkah 2: Buat project Google Cloud dan aktifkan Drive API

1. Buka **console.cloud.google.com** dan login dengan akun Google tempat backup akan disimpan.
2. Buat project baru, misalnya `Buku Hutang`.
3. Menu **APIs & Services** → **Library** → cari **Google Drive API** → **Enable**.

## Langkah 3: Buat OAuth Client ID

Nama menu di Google Cloud kadang berubah sedikit. Cari menu **Google Auth Platform** (dulu bernama *OAuth consent screen*).

1. **Branding**: isi nama aplikasi (`Buku Hutang Piutang`) dan email dukungan (email Anda).
2. **Audience**: pilih **External**. Biarkan status **Testing**, lalu di **Test users** tambahkan email Google Anda sendiri.
3. **Clients** → **Create client**:
   - Application type: **Web application**
   - Name: `Buku Hutang web`
   - **Authorized JavaScript origins**: isi origin dari Langkah 1, misalnya `https://NAMA-AKUN-GITHUB.github.io`. Jangan tambahkan `/buku-hutang/` atau garis miring di akhir.
   - Klik **Create**.
4. Salin **Client ID**, bentuknya seperti `1234567890-abcdef….apps.googleusercontent.com`.

Client ID bukan rahasia, jadi aman dimasukkan ke aplikasi. Jangan bagikan **Client secret** (tidak dipakai di aplikasi ini).

## Langkah 4: Pasang di iPhone

1. Buka alamat dari Langkah 1 di **Safari**.
2. Tap tombol **Bagikan** (kotak dengan panah ke atas) → **Tambah ke Layar Utama** → **Tambah**.
3. Mulai sekarang, **selalu buka lewat ikon "Buku Hutang" di Layar Utama**, bukan lewat tab Safari. Keduanya memakai penyimpanan terpisah di iPhone. Data di tab Safari biasa juga bisa dihapus otomatis oleh iOS jika lama tidak dibuka.
4. Buat **kata sandi**, lalu simpan **kode pemulihan** (screenshot atau app Catatan).

## Langkah 5: Hubungkan Google Drive

1. Di aplikasi, tap tombol **Drive belum diatur** di bagian atas.
2. Tempel **Client ID** dari Langkah 3, lalu tap **Simpan Client ID**.
3. Tap **Hubungkan Google Drive**, pilih akun Google Anda, lalu izinkan akses.
   - Karena status aplikasi masih *Testing*, Google akan menampilkan peringatan "Google belum memverifikasi aplikasi ini". Tap **Lanjutkan**. Ini normal untuk aplikasi pribadi.
4. Tombol berubah menjadi **Tersinkron 14.30** (jam sinkron terakhir). Di Google Drive Anda akan muncul folder **Buku Hutang Piutang**.

---

## Penggunaan sehari-hari

- **Catat pinjaman**: tap **+ Catat pinjaman** → **Foto bukti** atau **Pilih dari galeri**. Nominal, tanggal, dan nama penerima terisi otomatis. **Periksa hasil scan**, terutama nama, karena OCR di HP tidak sepintar Claude. Atur bunga dan jangka waktu, lalu **Simpan**.
- **Catat pengembalian**: buka pinjaman → pilih foto bukti pembayaran. Nominal dan tanggal terisi otomatis → **Simpan pembayaran**.
- **Sinkron**: berjalan otomatis setelah setiap perubahan. Izin Google berlaku sekitar 1 jam. Jika tombol menampilkan **Drive: tap untuk sinkron**, tap tombolnya sekali.
- **Pengingat**: buka pinjaman → **Simpan pengingat (.ics)** → pilih **Tambahkan ke Kalender**. Pengingat muncul pukul 09.00 pada H-3, H-1, dan hari jatuh tempo.

## Pindah ke HP baru, atau HP hilang

1. Pasang aplikasi di HP baru seperti Langkah 4.
2. Di layar **Buat kata sandi**, tap **Sudah punya data di Google Drive? Pulihkan**.
3. Masukkan Client ID → **Hubungkan Google Drive**.
4. Setelah sinkron, masukkan **kata sandi lama** Anda. Semua catatan dan foto bukti akan muncul kembali.

## Hal yang perlu diketahui

- File di Google Drive **tidak dienkripsi**. Siapa pun yang bisa membuka Google Drive Anda bisa membaca catatannya. Jaga akun Google Anda dengan verifikasi 2 langkah.
- Kata sandi hanya disimpan dalam bentuk acak (hash). Jika kata sandi **dan** kode pemulihan sama-sama hilang, aplikasi tidak bisa dibuka. Data mentah masih ada di file `buku-hutang-piutang.json` di Drive.
- Jika dua perangkat mengubah **pinjaman yang sama** sebelum sempat sinkron, perubahan yang **terakhir** yang dipakai.
- Untuk memperbarui aplikasi nanti, cukup unggah ulang `index.html` ke repo GitHub yang sama.
