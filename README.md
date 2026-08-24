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

## Privasi & penyimpanan data

File di repo ini **hanya kode dashboard** — tidak berisi data penjualan atau data pelanggan apa pun.
Semua CSV yang di-upload diproses sepenuhnya di browser dan tersimpan di **localStorage browser masing-masing pengguna** (per perangkat, per browser, per alamat situs). Data tidak pernah dikirim ke server mana pun.
