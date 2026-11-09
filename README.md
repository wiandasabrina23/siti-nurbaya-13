# SITI NURBAYA

**S**iswi **T**inggi **N**utrisi, **B**ebas **A**nemia, dan **Berdaya**

Website edukasi anemia dan gizi untuk remaja putri, dengan skrining mandiri dan kalender pemantauan tablet tambah darah (TTD) serta siklus haid.

Program Dokter Muda IKM Puskesmas Belimbing, periode 12 Oktober – 14 Desember 2026.

## Isi website

| Menu | Isinya |
| --- | --- |
| Beranda | Pengantar dan skala kadar Hb |
| Kenali | Pengertian, penyebab, tanda, dan tingkat anemia |
| Cegah | Makanan sumber zat besi, aturan minum TTD, mitos dan fakta |
| Aman TTD | Kapan TTD tidak boleh diminum |
| Isi Piringku | Porsi makan seimbang |
| Bahaya | Dampak anemia dan tanda bahaya |
| Skrining | 18 pernyataan untuk mengecek gejala sendiri |
| Pantau | Akun siswi, kalender TTD dan haid, pengingat minum TTD |

## Cara kerja

```
Siswi (HP/laptop) ──► index.html di GitHub Pages ──► Google Apps Script ──► Google Sheet milik petugas
```

- Siswi membuat akun dengan nama panggilan dan PIN 4–6 angka. Akun bisa dipakai di perangkat mana pun.
- PIN tidak dikirim ke server. Browser mengirim bukti PIN hasil pengolahan, dan server menyimpannya dengan hash tambahan.
- Catatan TTD, haid, dan pengaturan pengingat dienkripsi (AES-GCM) di perangkat siswi dengan kunci dari PIN, lalu disimpan sebagai teks terenkripsi di lembar `Catatan`.
- Petugas melihat nama panggilan, kelas, dan hasil skrining yang disimpan siswi di lembar `Akun`, `Skrining`, dan `Ringkasan`.
- Salah PIN 8 kali mengunci akun 15 menit.

Batas enkripsi: karena PIN hanya 4–6 angka, enkripsi mencegah catatan terbaca sekilas. Orang yang sengaja menebak PIN dengan alat khusus tetap mungkin membukanya.

## Isi repositori

```
index.html            Website (satu file, termasuk gambar maskot)
config.js             Alamat server Google Apps Script (diisi sendiri)
sw.js                 Service worker kecil untuk notifikasi pengingat
apps-script/Code.gs   Kode server untuk Google Sheet
.nojekyll             Agar GitHub Pages menyajikan file apa adanya
```

## Pemasangan

### 1. Siapkan server di Google Sheet

1. Buka [sheets.google.com](https://sheets.google.com) dan buat spreadsheet kosong, misalnya bernama `SITI NURBAYA`.
2. Klik **Ekstensi → Apps Script**. Hapus kode bawaan, lalu salin seluruh isi `apps-script/Code.gs` dan tempel di editor. Simpan.
3. Pilih fungsi **siapkan** di bagian atas editor, lalu klik **Jalankan**. Setujui izin yang diminta (Tinjau izin → pilih akun → Lanjutan → Buka proyek). Setelah selesai, lembar `Akun`, `Skrining`, dan `Ringkasan` muncul.
4. Klik **Terapkan → Penerapan baru**, pilih jenis **Aplikasi web**, isi:
   - Jalankan sebagai: **Saya**
   - Yang memiliki akses: **Siapa saja**
5. Klik **Terapkan** dan salin **URL aplikasi web** (berakhiran `/exec`).

### 2. Isi alamat server

Buka `config.js`, ganti `TEMPEL_ALAMAT_APPS_SCRIPT_DI_SINI` dengan URL tadi (tetap di antara tanda kutip), lalu simpan.

### 3. Pasang di GitHub Pages

1. Buat repositori baru di [github.com/new](https://github.com/new), misalnya `siti-nurbaya`. Pilih **Public** (GitHub Pages gratis memerlukan repositori publik).
2. Unggah semua file di folder ini (**Add file → Upload files**), lalu **Commit changes**.
3. Buka **Settings → Pages**. Pada **Build and deployment**, pilih **Deploy from a branch**, branch `main`, folder `/ (root)`, lalu **Save**.
4. Tunggu 1–2 menit. Alamat website muncul di halaman itu, bentuknya `https://NAMA-ANDA.github.io/siti-nurbaya/`.

### 4. Uji sebelum dibagikan

1. Buka alamat website di HP, buat akun uji, tandai TTD di kalender, isi skrining dan simpan hasilnya.
2. Masuk dari laptop dengan nama dan PIN yang sama. Catatan harus ikut muncul.
3. Cek lembar `Akun` dan `Skrining` di Google Sheet.
4. Hapus akun uji dari halaman Pantau.

## Memperbarui

- **Isi website berubah:** ganti `index.html` di GitHub (Add file → Upload files, atau ikon pensil). Website ikut diperbarui dalam 1–2 menit.
- **Code.gs berubah:** tempel kode baru di Apps Script, lalu **Terapkan → Kelola penerapan → ikon pensil → Versi baru → Terapkan**. URL `/exec` tetap sama.

## Mengelola data siswi

- **Siswi lupa PIN:** hapus barisnya di lembar `Akun`. Siswi mendaftar ulang dengan nama yang sama. Catatan lama terbuang otomatis, hasil skrining lama tetap ada.
- **Melihat hasil:** lembar `Skrining` berwarna menurut hasil (merah tebal = ada tanda bahaya). Lembar `Ringkasan` menghitung otomatis.
- **Lembar `Catatan`:** disembunyikan dan berisi data terenkripsi. Jangan dibuka atau diubah.
- **Setelah program:** hapus data siswi sesuai kebijakan puskesmas.

## Privasi dan etika

Data ini milik remaja, termasuk data haid.

- Pastikan sekolah dan puskesmas mengizinkan, sebaiknya dengan persetujuan orang tua.
- Jangan bagikan Google Sheet secara luas. Beri akses hanya kepada petugas yang perlu.
- Alamat server di `config.js` bersifat publik karena berada di repositori publik. Akun tetap terlindungi PIN dan penguncian, tetapi siapa pun yang tahu alamat itu bisa mencoba membuat akun. Pantau lembar `Akun` secara berkala.
- Hasil skrining bukan diagnosis. Daftar kapan TTD tidak boleh diminum bersifat umum dan perlu diperiksa dokter atau petugas puskesmas sebelum dipakai.

## Pengujian

Kode server dan website diuji dengan simulasi Google Apps Script di komputer: daftar, masuk dari perangkat lain, PIN salah, nama kembar, dua perangkat mengubah bersamaan, server putus, hapus akun, dan pengingat. Pengujian dengan Google asli dan notifikasi di HP asli perlu dilakukan sendiri lewat langkah 4 di atas.

## Rujukan isi

Kemenkes RI, Pedoman Pencegahan dan Penanggulangan Anemia pada Remaja Putri dan Wanita Usia Subur (2018); WHO (2011), Haemoglobin concentrations for the diagnosis of anaemia; Riskesdas 2018; Kemenkes RI, Isi Piringku (2018); Alomedika, Kontraindikasi dan Peringatan Ferrous Sulfate; Alodokter, Zat besi.

## Lisensi

Belum ditentukan. Tambahkan berkas `LICENSE` sesuai keputusan puskesmas dan pembimbing sebelum repositori dipublikasikan.
