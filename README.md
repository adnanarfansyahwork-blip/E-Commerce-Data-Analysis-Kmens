# E-Commerce-Data-Analysis-Kmens

Berikut adalah penjelasan lengkap mengenai proses Data Mining yang terdapat dalam dokumen notebook [UAS_Datamining.ipynb](cci:7://file:///c:/Users/User/Downloads/UAS_Datamining.ipynb:0:0-0:0) Anda, disusun sesuai dengan struktur laporan yang Anda minta.

### 1. Latar Belakang dan Tujuan Pemilihan Masalah
*   **Latar Belakang:** Dalam industri *e-commerce*, volume data transaksi sangat besar. Memahami pola transaksi dan perilaku penjualan sangat penting untuk strategi bisnis. Data mentah sulit diinterpretasikan secara langsung.
*   **Tujuan:** Menerapkan teknik *Data Mining* (penggalian data) untuk mengelompokkan (clustering) data transaksi penjualan. Tujuannya adalah untuk mengidentifikasi segmen transaksi yang berbeda (misalnya transaksi bernilai rendah, menengah, dan tinggi) guna memahami performa penjualan dan profitabilitas produk.

### 2. Deskripsi Dataset yang Digunakan
*   **Nama File:** `ecommerce_sales_data.csv`
*   **Ukuran Data:** 3.500 entri (baris).
*   **Atribut (Kolom):** Dataset terdiri dari 7 kolom, yaitu:
    *   **Kategorikal/Tanggal:** `Order Date` (Tanggal Pesanan), `Product Name` (Nama Produk), `Category` (Kategori), `Region` (Wilayah).
    *   **Numerik:** `Quantity` (Jumlah Barang), `Sales` (Total Penjualan), `Profit` (Keuntungan).
*   **Kondisi Data:** Dataset bersih, tidak ditemukan nilai yang hilang (*missing values*) pada ke-3500 entri.

### 3. Tahapan Pra-pemrosesan Data (Data Preprocessing)
Sebelum pemodelan, dilakukan beberapa tahapan persiapan data:
1.  **Pengecekan Data:** Memeriksa informasi dataset (`df.info()`) dan statistik deskriptif (`df.describe()`) untuk memahami distribusi data.
2.  **Pengecekan *Missing Values*:** Memastikan tidak ada data kosong menggunakan `df.isnull().sum()`. Hasilnya adalah 0 *missing value* untuk semua kolom.
3.  **Seleksi Fitur (*Feature Selection*):** Memilih atribut numerik yang relevan untuk proses *clustering*, yaitu: `'Quantity'`, `'Sales'`, dan `'Profit'`.
4.  **Normalisasi Data (*Scaling*):** Menggunakan `StandardScaler` dari pustaka Scikit-learn.
    *   **Alasan:** Algoritma K-Means menggunakan perhitungan jarak (Euclidean distance). Fitur 'Sales' (ribuan) memiliki skala yang jauh lebih besar daripada 'Quantity' (satuan). Tanpa normalisasi, fitur 'Sales' akan mendominasi perhitungan jarak, sehingga hasil *clustering* menjadi bias. *Scaling* mengubah semua fitur ke skala yang sama (rata-rata 0, standar deviasi 1).

### 4. Algoritma Data Mining yang Dipilih
*   **Algoritma:** **K-Means Clustering**.
*   **Jenis:** *Unsupervised Learning* (Pembelajaran tanpa pengawasan/label).
*   **Alasan Pemilihan:**
    *   K-Means adalah algoritma yang efisien dan cepat untuk mengelompokkan data numerik dalam jumlah yang cukup besar.
    *   Mudah diinterpretasikan untuk membagi data ke dalam kelompok-kelompok yang memiliki karakteristik serupa (homogen dalam klaster, heterogen antar klaster).
    *   Cocok untuk tujuan segmentasi transaksi berdasarkan pola jumlah, penjualan, dan keuntungan.

### 5. Proses Implementasi Algoritma menggunakan Python
Implementasi dilakukan dengan langkah-langkah berikut menggunakan bahasa Python:
1.  **Impor Pustaka:** Menggunakan `pandas` untuk manipulasi data, `sklearn` (Scikit-learn) untuk *modeling* (KMeans, StandardScaler, silhouette_score), serta `matplotlib` dan `seaborn` untuk visualisasi.
2.  **Penentuan Jumlah Klaster Optimal (K):**
    *   Menggunakan **Metode Elbow** (*Elbow Method*).
    *   Menghitung nilai *Sum of Squared Errors* (SSE) untuk K=1 hingga K=10.
    *   Memplot grafik SSE vs Jumlah Klaster. Dari grafik, terlihat "siku" (perubahan penurunan yang melandai) pada titik **K=3**.
3.  **Pemodelan:**
    *   Membangun model K-Means dengan `n_clusters=3`.
    *   Melatih model (`fit_predict`) menggunakan data yang sudah dinormalisasi (`X_scaled`).
    *   Menambahkan hasil label klaster ke dalam dataset utama (kolom `'Cluster'`).

### 6. Hasil Pengujian dan Evaluasi Model
*   **Jumlah Klaster:** Terbentuk 3 klaster (Cluster 0, 1, dan 2).
*   **Evaluasi Kuantitatif:** Nilai **Silhouette Score** adalah **0.418**. Nilai ini menunjukkan bahwa klaster yang terbentuk terpisah dengan cukup baik dan memiliki kohesi yang wajar.
*   **Analisis Karakteristik Klaster:**
    *   **Cluster 0 (Grup Menengah - "Standard"):** Jumlah transaksi sekitar 1.359. Karakteristik: Kuantitas barang sedang (~6), Penjualan menengah (~3.145), dan Profit menengah (~481).
    *   **Cluster 1 (Grup Tinggi - "High Performers"):** Jumlah transaksi paling sedikit (~668), namun memiliki nilai rata-rata tertinggi pada semua aspek: Kuantitas tinggi (~7.6), Penjualan sangat tinggi (~6.955), dan Profit sangat besar (~1.343).
    *   **Cluster 2 (Grup Rendah - "Small Scale"):** Jumlah transaksi terbanyak (~1.473). Karakteristik: Kuantitas kecil (~2.4), Penjualan rendah (~1.185), dan Profit kecil (~199).
*   **Visualisasi:** *Scatter plot* menunjukkan pembagian yang jelas antara ketiga klaster berdasarkan 'Sales', 'Quantity', dan 'Profit'.

### 7. Kesimpulan dan Saran Pengembangan Selanjutnya
*   **Kesimpulan:**
    Proses *data mining* berhasil mengelompokkan data transaksi penjualan menjadi tiga segmen yang jelas: Transaksi Skala Kecil (Cluster 2), Transaksi Standar (Cluster 0), dan Transaksi Bernilai Tinggi (Cluster 1). Pola ini membantu bisnis untuk fokus pada mempertahankan segmen bernilai tinggi atau meningkatkan segmen volume tinggi namun bernilai rendah.

*   **Saran Pengembangan:**
    *   **Analisis Lanjutan:** Melakukan analisis mendalam pada **Cluster 1** (High Profit) untuk melihat Produk atau Wilayah mana yang mendominasi.
    *   **Penambahan Fitur:** Menggunakan teknik seperti *One-Hot Encoding* untuk menyertakan data kategorial (seperti Kategori Produk atau Wilayah) agar *clustering* lebih kaya informasi.
    *   **Algoritma Lain:** Mencoba algoritma lain seperti *Hierarchical Clustering* atau *DBSCAN* untuk melihat apakah ada pola bentuk klaster yang non-spherical (tidak membulat).
