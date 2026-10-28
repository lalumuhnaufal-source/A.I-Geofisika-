# Indonesia Rainfall Monitoring Dashboard

Dashboard untuk menjelajahi pola curah hujan di berbagai provinsi dan kota di Indonesia, tersedia dalam dua versi: **web (HTML)** dan **Excel**.

> **DATA SIMULASI — Dataset ini digunakan untuk pengembangan dan pengujian dashboard, bukan data observasi resmi BMKG.**
> Dashboard ini bukan pemantauan cuaca langsung, dan nilainya tidak boleh dipakai untuk menyimpulkan banjir, kekeringan, atau bencana lain.

---

## 1. Isi folder

```
Proyek_Dashboard_Curah_Hujan/
├── README.md                                 (file ini)
├── dashboard_web/
│   ├── index.html                            kerangka halaman
│   ├── style.css                             tampilan
│   └── script.js                             logika + data simulasi tertanam
└── excel/
    └── Dataset_Curah_Hujan_Indonesia_Web.xlsm   data + dashboard Excel + macro VBA
```

Data sumber: `Dataset_Curah_Hujan_Indonesia_Web.xlsx` (sheet `Data_Curah_Hujan`, `Panduan`, `Ringkasan_Wilayah`), 1.000 observasi simulasi, 18 stasiun contoh, Januari 2024 sampai Desember 2025.

---

## 2. Versi web (HTML)

### Cara membuka
1. Pastikan `index.html`, `style.css`, dan `script.js` berada di **satu folder yang sama**. Jangan ganti nama file, karena `index.html` memanggil dua file lain berdasarkan namanya.
2. Klik dua kali `index.html` (Chrome, Edge, atau Firefox).
3. Pakai koneksi internet. Chart.js (grafik) dan Leaflet (peta) dimuat dari CDN. Tanpa internet, grafik tidak tampil dan peta menampilkan pesan pengganti.

Tidak perlu server, instalasi, atau build.

### Cara menggunakan dashboard
- **Ringkasan (7 kartu):** total, rata-rata, maksimum, jumlah pengamatan, hari hujan, suhu, kelembapan. Semuanya berubah saat filter diubah.
- **Filter:** tanggal mulai/akhir, tahun, bulan, provinsi, kota (multi-pilih dengan Ctrl/Cmd), stasiun (ketik untuk mencari), kategori, dan batas curah hujan min/maks. Tombol **Reset Filter** mengembalikan semuanya.
- **Grafik 1, tren bulanan:** pilih Total, Rata-rata, atau Maksimum.
- **Grafik 2, provinsi:** klik batang untuk melihat stasiun di provinsi itu, klik lagi untuk kembali.
- **Grafik 4, peringkat stasiun:** pilih Rata-rata/Maksimum dan Top 10/Semua. Klik batang untuk memfilter ke stasiun tersebut.
- **Grafik 5–7:** hubungan hujan dengan kelembapan, suhu, dan angin, lengkap dengan korelasi Pearson (hanya jika minimal 30 catatan).
- **Peta:** marker per stasiun; klik marker untuk melihat detail.
- **Key Insights:** temuan dan pertanyaan tindak lanjut yang dihitung ulang dari data terfilter.
- **Tabel observasi:** klik judul kolom untuk mengurutkan; tombol **Ekspor CSV** menyimpan baris hasil filter.
- **Validasi data:** daftar catatan kualitas data di bagian bawah halaman.

### Mengganti data
Data ada di bagian paling atas `script.js`, pada baris yang diawali `const COLS=`. Untuk memakai data baru, ganti baris itu dengan data dari Excel. Nama dan urutan 16 kolom harus sama dengan dataset, dan tanggal ditulis `yyyy-mm-dd`. Makro `BukaDashboard` di Excel (bagian 3) menyusun baris ini otomatis untuk versi HTML satu file.

### Menaruh di GitHub Pages
1. Unggah `index.html`, `style.css`, dan `script.js` ke satu folder repositori.
2. Di GitHub: **Settings > Pages**, pilih branch dan folder tempat file berada.
3. Tunggu beberapa menit, lalu buka alamat yang diberikan GitHub.

---

## 3. Versi Excel

### Memasang makro
1. Buka `Dataset_Curah_Hujan_Indonesia_Web.xlsx` di Excel (Windows, Excel 2019 atau Microsoft 365).
2. Tekan `Alt+F11`, pilih **Insert > Module**, lalu tempel kode VBA dashboard.
3. Jalankan `BuatDashboardExcel` dengan `F5`. Sheet `Dashboard_Excel` dibuat otomatis, dan sheet bantu `Ref_Dashboard` disembunyikan.
4. Simpan sebagai **Excel Macro-Enabled Workbook (.xlsm)** agar makro ikut tersimpan.
5. Jika file berasal dari unduhan dan makro diblokir: klik kanan file di Explorer, **Properties**, centang **Unblock**, lalu buka ulang.

### Cara menggunakan
- Ubah dropdown di sel kuning **D6:D12** (tahun, bulan, provinsi, kategori, dan tiga pilihan metrik).
- KPI, grafik 1–4, dan insight berubah otomatis karena semuanya berupa rumus.
- Grafik 5–7 dan korelasi memakai seluruh data (tidak terfilter).
- Jika data diganti atau jumlah barisnya berubah, jalankan `BuatDashboardExcel` lagi.

---

## 4. Perbedaan versi web dan Excel

| Fitur | Web | Excel |
|---|---|---|
| 7 KPI | Ya | Ya |
| Tren bulanan, provinsi, kategori, top stasiun | Ya | Ya |
| Scatter hujan vs kelembapan/suhu/angin | Ya, ikut filter | Ya, seluruh data |
| Insight dinamis | 7 jenis | 6 jenis |
| Filter | 9 filter | 4 filter |
| Drill-down provinsi, klik stasiun untuk memfilter | Ya | Tidak |
| Peta, tabel rinci, ekspor CSV | Ya | Tidak |
| Butuh internet | Ya (CDN) | Tidak |

Angka utama kedua versi sama karena berasal dari data yang sama (contoh: total 9.639,2 mm; rata-rata 9,64 mm; maksimum 104,7 mm; 1.000 catatan).

---

## 5. Catatan ilmiah

- Rata-rata = total ÷ jumlah observasi, bukan rata-rata satu stasiun.
- Total bulanan lintas stasiun tidak setara dengan curah hujan di satu lokasi. Jumlah observasi (n) berbeda antar wilayah dan bulan, jadi jangan membandingkan total mentah.
- Kategori hujan memakai batas kerja dataset (0 mm; >0–20; >20–50; >50–100; >100 mm), bukan klaim klasifikasi resmi BMKG.
- Korelasi tidak membuktikan sebab-akibat.
- Untuk analisis ilmiah atau publik, ganti atau validasi data dengan observasi resmi BMKG dan cantumkan metadata stasiun, periode, dan metode pengukuran.

---

## 6. Pemecahan masalah

| Masalah | Penyebab dan solusi |
|---|---|
| Halaman tampil tanpa gaya | `style.css` tidak satu folder dengan `index.html`, atau namanya berbeda. |
| Halaman kosong atau grafik tidak muncul | Tidak ada internet, atau `script.js` tidak satu folder. Periksa koneksi dan nama file. |
| Peta abu-abu | Tile peta (OpenStreetMap) tidak terjangkau. Grafik lain tetap berfungsi. |
| Dropdown Excel tidak berubah | Pastikan perhitungan Excel *Automatic* (Formulas > Calculation Options). |
| Kolom tabel bantu Excel menampilkan `#NAME?` | Jalankan `BuatDashboardExcel` sekali lagi. |
| Makro tidak mau jalan | Simpan sebagai `.xlsm`, aktifkan makro, dan lakukan **Unblock** pada file. |
