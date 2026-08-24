# Ruang Peta Revenue

Dashboard laporan mingguan penjualan Ruang Peta — revenue Scalev vs biaya (iklan Meta incl. PPN 11% + biaya manual), rekap untung/rugi per minggu, dan laporan performa iklan.

**Buka:** https://alfaleeindonesia-hash.github.io/ruang-peta-revenue/

## Cara pakai

1. Export data dari sumbernya:
   - **Order** — Scalev → Order → Export → *Order with Product (CSV)*
   - **Biaya iklan** — Meta Ads Manager → Reports → export dengan breakdown **per hari** (ada kolom "Amount spent"); angka dianggap belum PPN, dashboard menambahkan PPN 11% otomatis
   - **Biaya / revenue manual** — CSV sederhana `tgl,deskripsi,nilai[,kategori]`
2. Drag & drop file CSV ke dashboard. Data **bertambah** setiap upload (tidak menimpa); duplikat terdeteksi dan dikonfirmasi dulu.
3. Tab **Ringkasan** untuk laporan mingguan, tab **Laporan Iklan** untuk performa per iklan.

## Dua mode

- **Mode lokal** (default, selama `FIREBASE_CONFIG` di `index.html` belum diisi): tanpa login, data tersimpan di localStorage browser masing-masing pengguna.
- **Mode cloud** (setelah config Firebase diisi): wajib masuk — **Masuk dengan Google** atau **Masuk sebagai Tamu**. Data bersama tersimpan di Firestore dan tersinkron real-time ke semua yang membuka dashboard. Hanya akun `alfaleeindonesia@gmail.com` yang bisa meng-upload / mengubah data; Tamu dan akun Google lain hanya bisa melihat.

## Setup Firebase (sekali saja, ±5 menit)

1. Buka [console.firebase.google.com](https://console.firebase.google.com) → **Add project** (nama bebas, mis. `ruang-peta-revenue`; Analytics boleh dimatikan).
2. **Build → Authentication → Get started → Sign-in method**: aktifkan **Google** dan **Anonymous**.
3. **Authentication → Settings → Authorized domains**: tambahkan `alfaleeindonesia-hash.github.io`.
4. **Build → Firestore Database → Create database** (production mode, lokasi `asia-southeast2` / Jakarta).
5. Di tab **Rules**, ganti seluruh isinya dengan:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /dashboard/{docId} {
         allow read: if request.auth != null;
         allow write: if request.auth != null
                      && request.auth.token.email == 'alfaleeindonesia@gmail.com';
       }
     }
   }
   ```
   lalu **Publish**. Aturan inilah yang benar-benar memblokir upload dari akun lain (bukan sekadar tombol yang disembunyikan).
6. **Project settings (⚙) → Your apps → Add app → Web (</>)** → daftarkan app → salin nilai `apiKey`, `authDomain`, `projectId`.
7. Tempel ketiga nilai itu ke blok `window.FIREBASE_CONFIG = {...}` di bagian bawah `index.html`, commit & push.

Saat pertama kali login sebagai `alfaleeindonesia@gmail.com`, dashboard akan menawarkan memindahkan data lama dari browser ke cloud.

## Privasi & penyimpanan data

File di repo ini **hanya kode dashboard** — tidak berisi data penjualan atau data pelanggan apa pun.
Mode lokal: CSV diproses dan disimpan hanya di browser pengguna. Mode cloud: data tersimpan di Firestore proyek Firebase milikmu dan hanya bisa dibaca oleh pengguna yang login (termasuk Tamu); yang bisa menulis hanya akun resmi di atas.
