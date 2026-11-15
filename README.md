# MARJAYUK — Mango Sticky Rice Sales Dashboard

Website dashboard untuk mengelola pesanan Mango Sticky Rice ukuran kecil (Rp5.000) dan besar (Rp10.000). Dibuat dengan HTML, CSS, JavaScript module, Supabase, dan Chart.js.

## Isi folder
- `index.html` — halaman website.
- `styles.css` — desain responsif.
- `app.js` — logika aplikasi, login, pesanan, grafik, laporan, dan realtime.
- `config.js.example` — contoh konfigurasi Supabase.
- `supabase-schema.sql` — SQL untuk membuat tabel, validasi, RLS, dan Realtime.

## 1. Siapkan Supabase
1. Buat project di https://supabase.com/.
2. Buka **SQL Editor** di dashboard Supabase.
3. Buka file `supabase-schema.sql`, salin seluruh isinya, lalu jalankan di SQL Editor.
4. Buka **Project Settings → API**.
5. Salin **Project URL** dan **anon/public key**. Jangan gunakan `service_role` key.

## 2. Buat konfigurasi website
1. Salin `config.js.example` dan ubah nama salinannya menjadi `config.js`.
2. Isi URL dan anon/public key dari project Supabase:

```js
export const SUPABASE_URL = "https://project-anda.supabase.co";
export const SUPABASE_ANON_KEY = "anon-public-key-anda";
```

3. Jangan mengunggah `config.js` ke tempat publik yang tidak diperlukan. Anon key memang dirancang untuk frontend, tetapi keamanan data bergantung pada RLS.

## 3. Buat akun admin
1. Di Supabase, buka **Authentication → Users**.
2. Pilih **Add user** dan buat email serta password admin.
3. Matikan **Allow new users to sign up** pada pengaturan Authentication/Providers jika tersedia.
4. Gunakan akun admin yang sama untuk login dari semua HP dan laptop.

## 4. Jalankan website
Website memakai JavaScript module, jadi jalankan melalui server lokal, bukan membuka `index.html` langsung dari file manager.

Cara mudah:
- Upload folder ini ke layanan hosting statis seperti Netlify, Vercel, atau Cloudflare Pages; atau
- Jalankan server lokal dengan VS Code Live Server.

Setelah deploy, buka URL website dan login memakai akun Supabase.

## 5. Sinkronisasi antarperangkat
Semua perangkat login menggunakan akun yang sama dan project Supabase yang sama. Data pesanan disimpan di PostgreSQL online. Supabase Realtime digunakan untuk memperbarui tampilan saat ada perubahan pesanan dari perangkat lain.

## 6. Perhitungan
- Kecil: jumlah × Rp5.000.
- Besar: jumlah × Rp10.000.
- Pendapatan dashboard: hanya pesanan berstatus `paid` dan tidak dibatalkan.
- Pesanan batal tidak dihitung sebagai penjualan.
- Harga satuan disimpan pada transaksi agar catatan harga lama tetap konsisten.

## Catatan keamanan penting
- Jangan pernah memasukkan `service_role` key ke `config.js` atau file frontend.
- SQL mengaktifkan RLS. Akses tabel hanya diberikan kepada pengguna yang sudah login.
- Policy saat ini mengizinkan semua pengguna `authenticated` dalam project untuk mengelola pesanan. Karena desain awal menggunakan satu akun bersama, gunakan satu akun saja dan nonaktifkan pendaftaran publik.
- Untuk bisnis dengan beberapa pegawai dan hak akses berbeda, kebijakan RLS perlu ditingkatkan menggunakan tabel profil/role dan policy berbasis user.
- Gunakan password kuat dan jangan bagikan akun kepada orang yang tidak dipercaya.
- Backup database secara berkala melalui fitur backup Supabase.

## Batasan versi awal
- Ekspor laporan tersedia dalam format CSV yang dapat dibuka di Excel/Google Sheets.
- Grafik menampilkan pendapatan yang sudah dibayar.
- Belum ada pembayaran online otomatis, katalog publik, atau struk PDF.
