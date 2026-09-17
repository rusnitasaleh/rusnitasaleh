# Kas Toko

Aplikasi sederhana untuk mencatat uang masuk dan uang keluar di kasir toko, dengan dukungan **2 shift kerja** per hari.

## Cara Pakai

1. Buka file `index.html` langsung di browser (HP, tablet, atau komputer) — tidak perlu instalasi atau internet.
2. Di halaman **Kasir**, pilih Shift 1 atau Shift 2, isi nama kasir dan modal awal kas, lalu tekan **Buka Shift**.
3. Selama shift berjalan, catat setiap transaksi lewat tombol **Uang Masuk** / **Uang Keluar**.
4. Saldo kas saat ini (modal awal + masuk − keluar) selalu terlihat otomatis di atas.
5. Saat selesai jaga, tekan **Tutup Shift**, masukkan hasil hitung kas fisik — aplikasi akan menunjukkan apakah kas sesuai, lebih, atau kurang.
6. Buka tab **Riwayat** untuk melihat semua shift sebelumnya, membuka rincian transaksi, mencetak laporan, atau mengekspor semuanya ke CSV (misalnya untuk dibuka di Excel).
7. Tab **Pengaturan** untuk mengganti nama toko, tema tampilan, atau menghapus semua data.

## Penyimpanan Data

Semua data disimpan langsung di browser perangkat yang dipakai (localStorage) — tidak dikirim ke server manapun. Karena itu:

- Gunakan browser/perangkat yang sama secara konsisten untuk mencatat kas.
- Cadangkan data secara berkala lewat tombol **Ekspor CSV** di halaman Riwayat, terutama sebelum membersihkan cache browser atau mengganti perangkat.
- Menghapus data browser (clear site data) akan menghapus seluruh riwayat kas.

## Fitur

- Dua shift per hari (Shift 1 / Shift 2), masing-masing dengan kasir dan modal awal sendiri.
- Pencatatan uang masuk & keluar dengan kategori dan keterangan.
- Ringkasan otomatis: total masuk, total keluar, saldo kas berjalan.
- Penutupan shift dengan pencocokan kas fisik vs saldo sistem (deteksi selisih).
- Riwayat semua shift, dengan rincian transaksi dan opsi cetak laporan.
- Ekspor riwayat ke CSV.
- Tampilan mobile-friendly, mendukung tema terang/gelap.
