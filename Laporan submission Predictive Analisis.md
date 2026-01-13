# Domain Proyek
Kecelakaan kerja merupakan isu krusial dalam dunia industri yang berdampak pada keselamatan manusia dan efisiensi operasional. Berdasarkan data dari Occupational Safety and Health Administration (OSHA), ribuan insiden dilaporkan setiap tahun dengan tingkat keparahan yang bervariasi, mulai dari cedera ringan hingga amputasi dan rawat inap. Prediksi terhadap tingkat keparahan cedera (seperti apakah korban memerlukan rawat inap atau mengalami amputasi) sangat penting untuk mengidentifikasi faktor risiko utama di lingkungan kerja. Dengan memanfaatkan Machine Learning, perusahaan dapat melakukan langkah preventif yang lebih terarah berdasarkan pola historis insiden.

# 1. Businness Understanding
## 1.1 Permasalahan Utama
### Problem Statement:
- Setiap tahun, workplace injuries menyebabkan kerugian finansial signifikan dan kehilangan produktivitas. Perusahaan saat ini bersifat reaktif, mereka menginvestigasi dan merespons SETELAH kecelakaan terjadi, bukan SEBELUM. Bagaimana kita bisa memprediksi risiko severe injury secara proaktif untuk mencegah kecelakaan sebelum terjadi?
- Sejauh mana model Machine Learning dapat memprediksi secara akurat apakah suatu kecelakaan kerja akan berujung pada rawat inap atau tidak?

## 1.2 Kendala yang dihadapi
### Bagi tim HSSE: "Memadamkan Api Tanpa Tahu Sumbernya"
- Seingkali tim HSSE berada di bawah tekanan konstan untuk menurunkan angka insiden, namun seringkali merasa seperti sedang menebak-nebak di dalam gelap.
- Sulit menentukan kapan dan di titik mana risiko tinggi akan muncul sebelum terlambat.
- Sumber daya terbuang karna terkadang alokasi tim dan peralatan seringkali tidak tepat sasaran karena data yang tidak akurat.
- Sulit meyakinkan manajemen untuk berinvestasi pada langkah preventif karena manfaatnya tidak terlihat secara instan di laporan keuangan.

### Bagi Perusahaan: "Lubang Hitam Keuangan & Reputasi"
- Satu kecelakaan kerja bukan hanya angka di laporan bulanan, tapi sebuah hantaman telak bagi bisnis.
- Biaya yang membengkak baik untuk premi asuransi maupun kompensasi pekerja yang terus meroket.
- Setiap insiden berarti mesin berhenti, alur kerja terganggu, dan produktivitas merosot tajam.
- Kepercayaan dan reputasi publik bisa hancur dalam semalam akibat satu kelalaian fatal.

### Bagi Pekerja: "Taruhan Nyawa di Setiap Shift"
- Pekerja adalah mereka yang menanggung dampak paling nyata dan menyakitkan.
- Masa Depan yang terenggut seperti risiko cedera serius hingga cacat permanen yang mengubah hidup selamanya.
- Kehilangan pendapatan berarti ancaman bagi kesejahteraan keluarga.
- Trauma fisik mungkin sembuh, tapi trauma mental dan penurunan kualitas hidup seringkali menetap seumur hidup.

> Safety is not an expensive investment; it’s a life-saving strategy that protects your most valuable assets: your people and your profit.

## 1.3 Tujuan Project
- Dengan memprediksi risiko severe injury menggunakan dataset yang tersedia dan machine learning, perusahaan dapat beralih dari reactive ke proactive safety management, berpotensi mengurangi incidents hingga 67% dan menghemat jutaan dollar.
- Mengolah data insiden kerja untuk mengekstrak fitur-fitur penting yang berpengaruh pada keparahan cedera.
- Membangun model klasifikasi yang mampu memprediksi status Hospitalized dengan akurasi dan nilai ROC-AUC yang tinggi sebagai dasar pengambilan kebijakan keselamatan kerja.

## 1.4 Solution Statements
- Model Baseline & Perbandingan: Menggunakan tiga algoritma berbeda yaitu K-Nearest Neighbors (KNN), Random Forest, dan XGBoost untuk mendapatkan baseline performa terbaik.
- Metrik Evaluasi: Solusi akan diukur menggunakan metrik Akurasi, Precision, Recall, F1-Score, dan khususnya ROC-AUC untuk melihat kemampuan model dalam membedakan kelas target.

# 2. Data Understanding
## 2.1 Data Load
Dataset yang digunakan berisi catatan insiden kecelakaan kerja yang mencakup detail lokasi, waktu, jenis industri, dan deskripsi cedera.
- Sumber Data: [https://www.osha.gov/severeinjury]
- Kondisi Data: Dataset memiliki 101,145 baris dan 28 kolom. Terdapat nilai kosong yang signifikan pada beberapa kolom administratif.

Variabel-variabel pada dataset adalah sebagai berikut:
- Hospitalized: Variabel target (1 jika dirawat inap, 0 jika tidak).
- Amputation: Variabel target (1 jika amputasi, 0 jika tidak).
- EventDate: Tanggal terjadinya insiden kerja.
- State: Lokasi negara bagian tempat kejadian.
- Primary NAICS: Kode klasifikasi industri.
- NatureTitle: Deskripsi jenis cedera (misal: patah tulang, luka bakar).

## 2.2 EDA
Exploratory Data Analysis (EDA): Dilakukan visualisasi missing values untuk menentukan kolom yang layak dipertahankan. Ditemukan bahwa mayoritas insiden (81.6%) melibatkan rawat inap, menunjukkan adanya ketidakseimbangan kelas (imbalanced data) yang perlu diperhatikan.

# 3. Data Preparation
Berikut urutan teknik data preparation yang dilakukan:
1. Data Cleaning: Menghapus kolom dengan missing values > 50% (Address2, Inspection, dll) untuk menghindari bias dan derau (noise).
2. Feature Selection: Menghapus fitur ID dan alamat spesifik yang tidak memiliki nilai prediktif terhadap keparahan medis.
3. Feature Engineering: Mengonversi EventDate menjadi fitur numerik (tahun, bulan, hari) agar pola temporal dapat dipelajari oleh model.
4. Encoding: Mengubah variabel kategorikal seperti State dan NatureTitle menjadi representasi numerik agar dapat diproses algoritma.
5. Data Splitting: Membagi data menjadi 80% training dan 20% testing.
6. Feature Scaling: Menggunakan StandardScaler untuk menyeragamkan rentang nilai fitur, terutama penting bagi algoritma berbasis jarak seperti KNN.

# 4. Modeling
Proyek ini membandingkan tiga algoritma dengan parameter berikut:
- K-Nearest Neighbors (KNN): Menggunakan n_neighbors=5. Kelebihannya sederhana, namun kekurangannya sangat lambat pada dataset besar (100rb+ baris).
- Random Forest: Menggunakan n_estimators=100. Kelebihannya sangat stabil dan tahan terhadap overfitting.
- XGBoost: Algoritma gradient boosting yang dikenal cepat dan akurat, namun membutuhkan tuning parameter yang lebih intensif.

**Pemilihan Model Terbaik** : **Random Forest** dipilih sebagai solusi terbaik karena menghasilkan **ROC-AUC sebesar 0.9124** dan **Akurasi 80.24%**, jauh mengungguli XGBoost pada pengujian ini. Random Forest terbukti lebih handal dalam menangani fitur-fitur kategorikal yang telah di-encode.

# 5. Evaluation
Metrik evaluasi yang digunakan adalah Akurasi, Precision, Recall, dan ROC-AUC.
- **Akurasi** : Mengukur porsi prediksi benar dari keseluruhan data. (Formula: $(TP+TN)/(TP+TN+FP+FN)$).
- **ROC-AUC** : Mengukur luas area di bawah kurva ROC untuk menunjukkan seberapa baik model memisahkan antara kelas positif dan negatif.
- **Precision & Recall** : Penting karena dalam kasus ini, gagal memprediksi orang yang butuh rawat inap (False Negative) memiliki risiko tinggi.

Hasil Proyek (**Model Terbaik: Random Forest**):
- **Akurasi:** 0.8024
- **Precision:** 0.6444
- **Recall:** 0.8024
- **ROC-AUC:** 0.9124.

Model Random Forest mampu mendeteksi 80% kasus kecelakaan kerja dan memiliki kemampuan sangat baik dalam membedakan kondisi berisiko (ROC-AUC 0.91) sehingga baik digunakan sebagai sistem peringatan dini.
