VERSI 2 - FIX DATA KOSONG SAAT DIBUKA LANGSUNG DARI FILE://

EXECUTIVE DASHBOARD KAPRODI - TRANSKRIP SISTEM INFORMASI
=========================================================

Tujuan
------
Dashboard ini dibuat untuk Kaprodi Sistem Informasi dan membaca data LIVE dari Google Sheet:
https://docs.google.com/spreadsheets/d/1SJkCe4owOm5uWN3-51BZau3-d8DjUsW3wK5042UeRvE/edit?usp=sharing

Sheet yang dibaca
-----------------
1. DataMahasiswa
2. Detilnilai
3. Mapping

Header minimum DataMahasiswa
----------------------------
NIM | Nama | Total SKS | IPK

Dashboard juga mengenali alias seperti SKS Lulus, SKS, TotalSKS.

Header minimum Detilnilai
-------------------------
NIM | Nama | No | Mata Kuliah / Course Title | SKS | Nilai

Dashboard juga mengenali alias Mata Kuliah, Nama Mata Kuliah, Grade, HM/G.

Header Mapping yang disarankan
------------------------------
KodeMK | NamaMataKuliah | SKS | Semester | Jenis

Gunakan file mapping_template.csv sebagai template awal lalu import/copy ke sheet baru bernama "Mapping".

Fitur
-----
- KPI jumlah mahasiswa sesuai filter
- Kandidat cumlaude: seluruh nilai minimal B (nilai yang diizinkan A, A-, B+, B)
- Mahasiswa total SKS < 144, diurutkan berdasarkan kekurangan SKS terbesar
- Mahasiswa yang masih memiliki nilai D
- Mahasiswa yang masih memiliki nilai E
- Mata kuliah yang belum memiliki mapping kode pada sheet Mapping
- Tampilan awal setiap daftar hanya Top 5
- Tombol "Lihat Semua" membuka seluruh daftar
- Klik NIM membuka detail transkrip
- Pada daftar D/E, klik NIM hanya menampilkan mata kuliah bernilai D/E
- Klik nama mata kuliah belum mapping untuk melihat mahasiswa terdampak
- Filter Tahun Masuk dari 4 digit awal NIM
- Filter NIM atau Nama
- Sync Now
- Auto-sync setiap 30 detik
- Tombol Open Google Sheet

Cara mapping nama mata kuliah
-----------------------------
Transkrip berisi judul bilingual, contoh:
  "Agama / Study of Religion"
Dashboard membandingkan bagian Indonesia sebelum tanda "/" dengan kolom NamaMataKuliah pada sheet Mapping.
Perbandingan tidak sensitif huruf besar/kecil dan mengabaikan variasi spasi/tanda baca sederhana.

Catatan penting akses Google Sheet
----------------------------------
Agar GitHub Pages dapat membaca sheet tanpa login, Google Sheet harus dapat dilihat melalui link publik (misalnya "Anyone with the link - Viewer").
Bila Google Sheet tidak dapat dibaca, dashboard otomatis menggunakan data_fallback.json sebagai snapshot 61 mahasiswa dan menampilkan status "Snapshot fallback".

Cara menjalankan lokal
----------------------
Jangan double-click index.html jika browser memblokir fetch file lokal.
Jalankan web server sederhana dari folder ini, misalnya:
  python -m http.server 8000
Lalu buka:
  http://localhost:8000

Cara publish ke GitHub Pages
----------------------------
1. Buat repository baru, misalnya kaprodi-transkrip-dashboard.
2. Upload semua file pada folder ini ke ROOT repository:
   - index.html
   - config.js
   - data_fallback.json
   - mapping_template.csv
   - README.txt
3. GitHub > Settings > Pages.
4. Source: Deploy from a branch.
5. Branch: main, folder /(root).
6. Save.
7. Setelah beberapa saat GitHub akan memberikan URL dashboard.

Konfigurasi
-----------
Semua pengaturan utama ada di config.js:
- GOOGLE_SHEET_ID
- nama sheet
- TARGET_SKS = 144
- AUTO_SYNC_MS = 30000
- TOP_LIMIT = 5

Catatan definisi cumlaude
-------------------------
Untuk dashboard ini, definisi "cumlaude" mengikuti permintaan: tidak ada nilai di bawah B.
Ini adalah indikator kandidat berbasis nilai saja, bukan penetapan yudisium resmi. Jika ada syarat IPK, masa studi, pengulangan mata kuliah, atau aturan akademik lain, logikanya dapat ditambahkan.


Catatan V2:
- data_fallback.js dimuat langsung sehingga 61 mahasiswa langsung muncul walaupun index.html dibuka dengan double-click.
- Setelah data snapshot tampil, dashboard mencoba sinkronisasi Google Sheet.
- Untuk sinkronisasi live yang paling stabil, gunakan GitHub Pages atau local web server, bukan file://.
