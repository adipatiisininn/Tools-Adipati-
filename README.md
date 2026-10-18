# Toolsku Tracker — versi awal

Website responsif/PWA untuk berbagi lokasi dengan persetujuan. Termasuk GPS browser, peta OpenStreetMap, riwayat dan ekspor CSV, jarak tempuh perkiraan, akurasi GPS, indikator baterai jika browser mendukung, tema gelap/terang, login Firebase, serta tautan sesi.

## Penting sebelum digunakan
- GPS browser membutuhkan HTTPS (GitHub Pages, Netlify, Vercel) atau `localhost`.
- Pelacakan real-time lintas perangkat memerlukan Firebase Auth + Firestore.
- Fitur baterai bergantung browser/perangkat; banyak browser Android modern tidak menyediakan Battery Status API.
- Riwayat lokasi adalah data sensitif. Jangan gunakan konfigurasi produksi tanpa menguji rules dan desain akses.
- Link sesi bukan sistem otorisasi yang cukup dengan sendirinya. Kode ini adalah starter/demo, bukan aplikasi produksi yang telah diaudit.
- Website tidak dapat mengambil lokasi GPS hanya dari nomor telepon/operator. Tambahkan pencarian informasi nomor melalui penyedia resmi jika dibutuhkan, atau kirim undangan agar pemilik perangkat membagikan GPS secara sukarela.

## 1. Buat proyek Firebase
1. Buka https://console.firebase.google.com/ dan buat project.
2. Tambahkan aplikasi Web, salin konfigurasi Firebase.
3. Authentication → Sign-in method → aktifkan Email/Password.
4. Firestore Database → buat database.
5. Salin konfigurasi ke objek `firebaseConfig` di `app.js`, ganti semua nilai `GANTI_...`.
6. Pada Rules Firestore, tinjau dan pasang `firestore.rules` sebagai titik awal. Uji dengan akun kedua. Aturan contoh ini membatasi pembaca, tetapi undangan lintas akun perlu mekanisme membership/token yang benar sebelum produksi.

## 2. Uji dari HP
1. Upload seluruh isi folder ini ke repository GitHub.
2. GitHub repo → Settings → Pages → Deploy from a branch → pilih `main` dan `/ (root)` → Save.
3. Tunggu URL Pages aktif dan buka URL HTTPS dari Chrome.
4. Daftar akun, login, centang persetujuan, lalu tekan **Mulai berbagi GPS**.
5. Izinkan akses lokasi pada prompt browser.
6. Salin link sesi. Untuk versi starter ini, viewer harus login dan aturan Firestore harus mengizinkan viewer; tambahkan UID viewer ke `allowedViewers` secara aman sebelum mengharapkan akun lain dapat membaca sesi. Jangan membuat database publik untuk mempermudah demo.
7. Tekan **Berhenti berbagi** untuk menghentikan pembaruan GPS.

## 3. Pasang seperti aplikasi
Buka URL HTTPS di Chrome Android → menu ⋮ → **Tambahkan ke layar utama** atau **Install app** bila tersedia.

## 4. Struktur file
- `index.html`: struktur halaman.
- `styles.css`: tema premium dan responsif.
- `app.js`: GPS, peta, auth, Firestore, riwayat, ekspor, baterai.
- `manifest.json`: metadata PWA.
- `sw.js`: cache aset dasar.
- `firestore.rules`: contoh aturan awal.

## Batasan versi ini
- Viewer lintas akun memerlukan daftar izin (`allowedViewers`) yang dikelola dengan aman; jangan membagikan data lokasi secara publik.
- Data riwayat Firestore belum memiliki mekanisme retensi otomatis; hapus sesuai kebutuhan dan kebijakan privasi.
- `watchPosition` di browser umumnya berhenti/ditangguhkan saat tab ditutup atau HP menghemat baterai. Untuk tracking background yang andal diperlukan aplikasi native dengan izin dan notifikasi yang sesuai.
- Perkiraan jarak adalah jumlah garis lurus antar pembaruan GPS, bukan jarak jalan.
