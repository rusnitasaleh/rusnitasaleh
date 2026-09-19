# Kas Toko

Aplikasi untuk mencatat uang masuk dan uang keluar di kasir toko, dengan dukungan **2 shift kerja** per hari. Data disimpan bersama di cloud (Firebase) sehingga **semua kasir di perangkat manapun (HP atau komputer) melihat data yang sama secara langsung**, dan hanya akun yang Anda buat sendiri yang bisa membuka aplikasi ini.

## Kenapa perlu setup dulu?

Versi sebelumnya menyimpan data hanya di satu browser/perangkat (localStorage), jadi kalau kasir memakai HP masing-masing, catatan mereka tidak saling terlihat. Versi ini memakai **Firebase** (layanan Google, gratis untuk skala toko kecil) sebagai database bersama, dilindungi login supaya hanya orang yang Anda izinkan yang bisa masuk. Setup awal butuh ~15 menit, sekali saja.

## Langkah Setup (sekali saja)

### 1. Buat proyek Firebase (gratis, tanpa kartu kredit)

1. Buka **https://console.firebase.google.com** dan masuk dengan akun Google Anda.
2. Klik **Add project** / **Tambah proyek**, beri nama misalnya `kas-toko-anda`, lanjutkan sampai selesai (Google Analytics boleh dimatikan, tidak perlu).

### 2. Aktifkan Firestore Database

1. Di menu kiri, buka **Build > Firestore Database**.
2. Klik **Create database**.
3. Pilih lokasi server terdekat (misalnya `asia-southeast2` untuk Indonesia).
4. Pilih mode **Production mode**, lalu **Enable**.

### 3. Pasang aturan keamanan (supaya hanya akun yang login yang bisa akses)

1. Di halaman Firestore, buka tab **Rules**.
2. Ganti isinya dengan ini, lalu klik **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

Ini berarti: siapapun yang **belum login tidak bisa membaca atau menulis apapun** — hanya akun yang Anda buat di langkah berikut yang bisa.

### 4. Aktifkan login Email/Password

1. Di menu kiri, buka **Build > Authentication**.
2. Klik **Get started**.
3. Pilih provider **Email/Password**, aktifkan, **Save**.

### 5. Tambahkan akun untuk Anda dan setiap kasir

1. Masih di **Authentication**, buka tab **Users**.
2. Klik **Add user** untuk setiap orang yang boleh pakai aplikasi (pemilik toko + tiap kasir). Isi email (boleh email asli mereka, atau alamat buatan seperti `kasir1@tokoanda.com`) dan buat kata sandi untuk masing-masing.
3. Bagikan email + kata sandi itu langsung ke orangnya (lewat WhatsApp pribadi, bukan ditulis di tempat umum).
4. **Hanya orang dengan akun yang Anda buat di sini yang bisa masuk ke aplikasi** — tidak ada pendaftaran sendiri di dalam aplikasi.

Untuk menambah atau menghapus akses seseorang nanti, kembali ke halaman **Users** ini kapan saja.

### 6. Ambil konfigurasi aplikasi web

1. Klik ikon gerigi di pojok kiri atas > **Project settings**.
2. Gulir ke bagian **Your apps**, klik ikon web `</>`.
3. Beri nama app (misalnya "Kas Toko"), klik **Register app** (tidak perlu centang hosting).
4. Anda akan melihat kode berisi objek `firebaseConfig` seperti ini:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "kas-toko-anda.firebaseapp.com",
  projectId: "kas-toko-anda",
  storageBucket: "kas-toko-anda.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abcdef123456"
};
```

5. Buka file **`firebase-config.js`** di folder aplikasi ini, dan ganti nilai-nilai di dalamnya dengan nilai asli dari proyek Anda persis seperti di atas.

### 7. Host aplikasinya

Setelah `firebase-config.js` diisi, upload **seluruh folder** `kas-toko` (bukan cuma `index.html` saja — `firebase-config.js` harus ikut) ke hosting statis gratis pilihan Anda, misalnya:

- **Netlify Drop**: buka https://app.netlify.com/drop, seret folder `kas-toko` ke halaman itu, dapat link langsung.
- **GitHub Pages**: aktifkan dari Settings > Pages di repo ini (repo publik, gratis).

Link itulah yang dibagikan ke semua kasir untuk dibookmark.

## Cara Pakai Sehari-hari

1. Buka link aplikasinya, **masuk (login)** dengan email + kata sandi yang diberikan pemilik toko.
2. Di halaman **Kasir**, pilih Shift 1 atau Shift 2, isi nama kasir dan modal awal kas, lalu **Buka Shift**. (Sistem mencegah dua shift aktif dibuka bersamaan, dari perangkat manapun.)
3. Catat setiap transaksi lewat tombol **Uang Masuk** / **Uang Keluar** — otomatis muncul di semua perangkat lain yang sedang login, secara langsung.
4. Saat selesai jaga, tekan **Tutup Shift**, masukkan hasil hitung kas fisik — aplikasi menunjukkan apakah kas sesuai, lebih, atau kurang.
5. Tab **Riwayat**: semua shift dari semua kasir & semua perangkat, dengan rincian transaksi, cetak laporan, dan ekspor ke CSV.
6. Tab **Statistik**: peringkat kasir berdasarkan total uang masuk yang mereka catat.
7. Tab **Pengaturan**: nama toko, tema, keluar dari akun (logout), atau hapus semua data (hanya untuk situasi darurat — ini menghapus data milik semua kasir).

## Tentang Data & Akses

- Data disimpan di Firestore (Google Cloud), bukan lagi hanya di satu browser — semua perangkat yang login melihat data yang sama secara real-time.
- Hanya akun yang Anda buat sendiri di Firebase Authentication yang bisa login — tidak ada pendaftaran akun sendiri di dalam aplikasi.
- Butuh koneksi internet untuk menyimpan/melihat data terbaru. Aplikasi menyimpan sedikit data sementara di perangkat agar tetap bisa dipakai sebentar saat koneksi terputus, dan akan menyinkronkan otomatis begitu online kembali — tapi ini bukan pengganti koneksi internet yang stabil.
- Cadangkan data secara berkala lewat **Ekspor CSV** di halaman Riwayat.
- Firebase gratis (paket Spark) sudah lebih dari cukup untuk skala satu toko kecil.

## Fitur

- Dua shift per hari (Shift 1 / Shift 2), dengan penguncian agar tidak ada dua shift aktif bersamaan.
- Login terbatas untuk akun yang diizinkan pemilik toko.
- Sinkronisasi langsung (real-time) antar semua perangkat yang login.
- Pencatatan uang masuk & keluar dengan kategori dan keterangan, tercatat siapa yang mencatat.
- Ringkasan otomatis: total masuk, total keluar, saldo kas berjalan.
- Penutupan shift dengan pencocokan kas fisik vs saldo sistem (deteksi selisih).
- Riwayat semua shift dari semua kasir, dengan rincian transaksi dan opsi cetak laporan.
- Statistik peringkat kasir berdasarkan uang masuk.
- Ekspor riwayat ke CSV.
- Tampilan mobile-friendly, mendukung tema terang/gelap.
