# Laporan Proyek Machine Learning - Fadhilah Nurrahmayanti

## Project Overview

Rekomendasi buku merupakan salah satu aplikasi paling praktis dalam bidang pengambilan informasi dan e-commerce. Dengan melimpahnya jumlah buku yang tersedia secara online, pengguna seringkali mengalami kesulitan dalam menemukan buku yang sesuai dengan minat dan preferensi mereka. Sistem rekomendasi hadir untuk membantu pengguna dalam menemukan konten yang relevan dan dipersonalisasi berdasarkan perilaku dan kesukaan mereka.

### Tujuan Proyek

Proyek ini bertujuan untuk membangun **sistem rekomendasi buku berbasis machine learning** dengan menggabungkan dua pendekatan utama:
- **Content-Based Filtering (CBF)**: Menganalisis fitur konten buku (misalnya sinopsis atau metadata) dan menyarankan buku yang mirip dengan yang disukai pengguna sebelumnya.
- **Collaborative Filtering (CF)**: Mengandalkan pola interaksi pengguna terhadap buku, seperti rating, untuk menyarankan buku yang disukai oleh pengguna lain dengan preferensi serupa.

### Dataset

Model dilatih menggunakan [Book Recommendation Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset) dari Kaggle, yang terdiri atas tiga bagian data utama:
- Informasi buku (judul, penulis, sinopsis)
- Data pengguna
- Data rating antara pengguna dan buku

### Pentingnya Sistem Rekomendasi

Sistem rekomendasi terbukti meningkatkan keterlibatan pengguna dan kepuasan dalam berbagai platform digital seperti Amazon, Goodreads, hingga Netflix. Dengan memberikan rekomendasi yang relevan, sistem ini tidak hanya mempercepat proses pencarian informasi, tetapi juga meningkatkan kemungkinan pembelian atau konsumsi konten oleh pengguna.

### Insight dari Literatur

Beberapa literatur ilmiah mendukung pentingnya dan efektivitas sistem rekomendasi:

- [When E-Commerce Personalization Systems Show and Tell: Investigating the Relative Persuasive Appeal of Content-Based versus Collaborative Filtering](https://doi.org/10.1080/00913367.2021.1887013)

  Penelitian ini menyatakan bahwa sistem rekomendasi berbasis Content-Based Filtering maupun Collaborative Filtering sama-sama memiliki daya tarik persuasif yang kuat dalam meningkatkan engagement pengguna.

- [Improving Recommender Systems using Hybrid Techniques of Collaborative Filtering and Content-Based Filtering](https://doi.org/10.47738/jads.v4i3.115) 
  Studi ini menyebutkan bahwa meskipun pendekatan hybrid menawarkan hasil terbaik, penggunaan CBF dan CF secara terpisah pun sudah mampu menghasilkan sistem rekomendasi yang cukup komprehensif.

- [A Brief Analysis of Collaborative and Content-Based Filtering Algorithms (IOP Conference, 2020)](https://doi.org/10.1088/1757-899X/981/2/022008)  
  Penelitian ini menyoroti kekuatan dan kelemahan dari masing-masing pendekatan:
  - **Collaborative Filtering** sangat efektif dalam menghasilkan rekomendasi yang dipersonalisasi, namun menghadapi tantangan seperti cold start dan data sparsity.
  - **Content-Based Filtering** cocok untuk kasus cold start dan tidak tergantung pada data pengguna lain, tetapi cenderung memberikan rekomendasi yang kurang bervariasi karena hanya berdasarkan preferensi sebelumnya.

### Kesimpulan

Melalui proyek ini, sistem rekomendasi buku dibangun dengan memanfaatkan keunggulan dua pendekatan klasik—CBF dan CF—untuk menyajikan hasil rekomendasi yang lebih personal dan akurat. Sistem ini diharapkan dapat menjadi solusi efektif dalam membantu pengguna menjelajahi dan menemukan buku yang sesuai di tengah derasnya arus informasi digital.

## Business Understanding

### Problem Statements

* Bagaimana cara membantu pengguna menemukan buku yang sesuai dengan minat mereka?
* Bagaimana cara memberikan rekomendasi buku meskipun pengguna belum memberikan banyak rating?

### Goals

* Membangun sistem rekomendasi yang menyarankan buku berdasarkan kemiripan konten (misalnya judul, penulis).
* Melatih model yang belajar dari interaksi rating pengguna dan buku untuk memberikan rekomendasi yang dipersonalisasi.

### Solutions

* **Content-Based Filtering**

    Menggunakan TF-IDF dan cosine similarity berdasarkan judul dan penulis buku.

* **Collaborative Filtering**
    
    Menggunakan Neural Network (RecommenderNet) dengan embedding untuk user dan item untuk memprediksi rating.

---

## Data Understanding

Univariate Exploratory Data Analysis (EDA), dataset diambil dari Kaggle: [Book Recommendation Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset)

### Data Loading        

Berdasarkan dataset Books.csv, Users.csv dan Rating.csv:

* DataFrame: books memiliki 271.360 entri, users memiliki 278.858 entri, dan ratings memiliki 1.149.780 entri.
* Jumlah data buku (271.360) sama dengan jumlah ISBN unik (271.360).
* Keunikan data pengguna: Jumlah data pengguna (278.858) sama dengan jumlah User-ID unik (278.858).
* Keunikan data rating: Ada 1.149.780 entri rating, tetapi hanya ada 105.283 User-ID unik dalam dataset rating. Ini menunjukkan bahwa hanya sebagian kecil dari total pengguna (105.283 dari 278.858) yang memberikan rating.

Fitur yang digunakan:

* User-ID: Data pengguna unik
* ISBN: Data ISBN unik
* Book-Title: Judul Buku
* Book-Author: Penulis Buku
* Year-Of-Publication: Tahun Buku diterbitkan
* Publisher: Penerbit Buku
* Book-Rating: Ulasan dari pengguna

### Books Variable

| #   | Kolom                | Non-Null Count   | Tipe Data |
|-----|----------------------|------------------|-----------|
| 0   | ISBN                 | 271360           | object    |
| 1   | Book-Title           | 271360           | object    |
| 2   | Book-Author          | 271358           | object    |
| 3   | Year-Of-Publication  | 271360           | object    |
| 4   | Publisher            | 271358           | object    |
| 5   | Image-URL-S          | 271360           | object    |
| 6   | Image-URL-M          | 271360           | object    |
| 7   | Image-URL-L          | 271357           | object    |

Insight:

* Unique ISBNs: 271360
* Unique Titles: 242135
* Unique Authors: 102022
* Unique Publishers: 16807

#### Missing Values in Books

| Kolom                | Jumlah Missing Values |
|----------------------|------------------------|
| ISBN                 | 0                      |
| Book-Title           | 0                      |
| Book-Author          | 2                      |
| Year-Of-Publication  | 0                      |
| Publisher            | 2                      |
| Image-URL-S          | 0                      |
| Image-URL-M          | 0                      |
| Image-URL-L          | 3                      |

Insight:

* Output secara jelas mengkonfirmasi adanya nilai yang hilang di kolom Book-Author (2), Publisher (2), dan Image-URL-L (3).

#### Top 10 Publishers

![Book Variable - Top 10 Publishers](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/booksvar-top10publishers.png?raw=true)

Insight:

* Harlequin merupakan penerbit paling dominan dalam dataset dengan jumlah buku lebih dari 7.000, jauh melampaui penerbit lainnya  
* Penerbit populer lainnya seperti Silhouette, Pocket, Ballantine Books, dan Bantam Books masing-masing memiliki sekitar 3.500 hingga 4.200 buku  
* Seluruh penerbit dalam daftar top 10 memiliki kontribusi signifikan terhadap jumlah total buku dalam dataset  
* Dominasi penerbit tertentu dapat mencerminkan segmentasi pasar atau genre buku yang sering diterbitkan, misalnya fiksi romantis atau bacaan populer  
* Konsentrasi pada penerbit besar bisa menjadi pertimbangan penting dalam analisis atau sistem rekomendasi, karena bisa mempengaruhi keberagaman konten yang tersedia  

#### Top 10 Authors

![Book Variable - Top 10 Authors](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/booksvar-top10authors.png?raw=true)

Insight:

* Agatha Christie menjadi penulis paling produktif dalam dataset, dengan lebih dari 640 judul buku
* William Shakespeare dan Stephen King juga menempati posisi tinggi
* Ann M. Martin dan Carolyn Keene menunjukkan dominasi
* Francine Pascal juga memiliki jumlah buku yang signifikan
* Isaac Asimov dan Charles Dickens turut memperlihatkan minat terhadap karya-karya sepanjang masa  

#### Checking Outliers of Year Of Publication

![Book Variable - Checking Outliers of Year Of Publication](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/booksvar-checkoutliers.png?raw=true)

Insight:

* Sebagian besar buku dalam dataset diterbitkan antara 1950 hingga 2005, mencerminkan era modern penerbitan dan peningkatan literasi global
* Teridentifikasi banyak outlier, termasuk tahun-tahun seperti 0, 1378, dan 1800
* Puncak distribusi tampaknya berada pada dekade 1990-an hingga awal 2000-an, menandakan masa produktif industri buku sebelum era digital sepenuhnya berkembang

### Users Variable

| #   | Kolom     | Non-Null Count | Tipe Data |
|-----|-----------|----------------|-----------|
| 0   | User-ID   | 278858         | int64     |
| 1   | Location  | 278858         | object    |
| 2   | Age       | 168096         | float64   |

### Statistics

* Unique Users: 278858
* Unique Locations: 57339

### Missing Values in Users

| Kolom  | Jumlah Missing Values |
|--------|------------------------|
| Age    | 110762                 |

Insight:

* Kolom Age memiliki 110762 nilai yang hilang dari total 278858 entri, tetapi kolom User-ID dan Location tidak memiliki missing value.
* Persentase missing value pada Age sekitar 40 persen sehingga perlu penanganan khusus sebelum digunakan dalam analisis atau pelatihan model

#### Distribution of Users by Age Group

Untuk memahami karakteristik pengguna berdasarkan usia, kami melakukan filtering data `Age` dalam rentang yang masuk akal (5–100 tahun). Kemudian, usia tersebut diklasifikasikan ke dalam beberapa kelompok:

- **Child**: Usia 5–12 tahun  
- **Teenager**: Usia 13–20 tahun  
- **Adult**: Usia 21–59 tahun  
- **Senior**: Usia 60 tahun ke atas

Distribusi pengguna setelah pengelompokan usia ditampilkan pada grafik berikut:

![Users Variable - Distribution of Users by Age Group](repo_src/https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/usersvar-distributionofusers.png?raw=true)

Insight:

* Kelompok usia Dewasa (21–59 tahun) dengan jumlah mencapai 134.883 pengguna, menunjukkan bahwa segmen usia dewasa merupakan pengguna dominan dalam dataset
* Kelompok Remaja (13–20 tahun) menempati posisi kedua dengan 22.553 pengguna, menandakan keterlibatan cukup besar dari pengguna usia remaja
* Kelompok Lansia (60+ tahun) memiliki 8.828 pengguna, menunjukkan partisipasi yang lebih rendah namun tetap signifikan dari pengguna lanjut usia
* Kelompok Anak-anak (5–12 tahun) adalah yang paling sedikit dengan hanya 584 pengguna, yang mungkin mencerminkan akses atau relevansi rendah terhadap platform atau konten buku yang tersedia

#### Checking Outliers of User Age

![Users Variable - Checking Outliers of User Age](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/usersvar-checkoutliers.png?raw=true)

Insight:

* Distribusi usia pengguna menunjukkan adanya banyak outlier, terutama di atas usia 100 tahun, yang secara realistis tidak masuk akal
* Sebagian besar data usia terkonsentrasi antara rentang usia 20 hingga 50 tahun
* Kehadiran nilai ekstrem hingga usia mendekati 250 tahun

### Ratings Variable

| No | Kolom        | Non-Null Count  | Tipe Data |
|----|--------------|-----------------|-----------|
| 1  | User-ID      | 1,149,780       | int64     |
| 2  | ISBN         | 1,149,780       | object    |
| 3  | Book-Rating  | 1,149,780       | int64     |

Insight:

* Terdiri dari 1.149.780 entri tanpa nilai kosong, menandakan data lengkap dan siap digunakan untuk analisis
* User-ID dan Book-Rating bertipe data integer, sedangkan ISBN bertipe objek (string)
* Tidak adanya missing value pada ketiga kolom menunjukkan kualitas data yang baik dari sisi kelengkapan informasi

### Statistics

- **Jumlah pengguna unik (`User-ID`)**: 105,283
- **Jumlah buku unik (`ISBN`)**: 340,556

#### Distribution of Book Ratings per User

| Rating | Jumlah         |
|--------|----------------|
| 0      | 716,109        |
| 1      | 1,770          |
| 2      | 2,759          |
| 3      | 5,996          |
| 4      | 8,904          |
| 5      | 50,974         |
| 6      | 36,924         |
| 7      | 76,457         |
| 8      | 103,736        |
| 9      | 67,541         |
| 10     | 78,610         |

Dengan Grafik sebagai berikut:

![Ratings Variable - ](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/ratingsvar-distributionofbookratings.png?raw=true)

Rating `0` dianggap sebagai *implicit feedback*, bukan penilaian eksplisit. Oleh karena itu, nilai ini akan **dikeluarkan dari model Collaborative Filtering**.

---

## Data Preparation

Dataset ini siap digunakan baik untuk model Collaborative Filtering maupun Content-Based Filtering secara fleksibel. Tahapan data preparation sangat krusial untuk memastikan data dalam format yang tepat dan relevan agar model machine learning dapat dilatih secara optimal. Dalam proyek ini, data preparation dilakukan untuk mempersiapkan data bagi dua pendekatan model: Content-Based Filtering (CBF) dan Collaborative Filtering (CF).

**Langkah-langkah:**

- Pra-pemrosesan data untuk menangani nilai kosong dan outlier:
  - Mengisi nilai kosong pada kolom `Book-Author` dan `Publisher` dengan 'Unknown'.
  - Karena nilai kosong pada `Image-URL-L` hanya 1 data, maka data tersebut dihapus.
  - Menghapus outlier pada kolom `Year-Of-Publication` menggunakan metode Interquartile Range (IQR).  
  - Pada kolom usia pengguna (`Age`), data difilter untuk rentang usia antara 5 hingga 100 tahun.
  - Setelah itu, nilai kosong pada usia diisi menggunakan nilai median.  
  - Membersihkan nilai 0 pada kolom `Book-Rating` karena dianggap tidak memberikan penilaian eksplisit.  
- Menggabungkan semua data hasil pembersihan menggunakan *inner join* untuk digunakan dalam proses pemodelan.

**Alasan:**

- Proses *encoding* diperlukan karena lapisan embedding hanya menerima input dalam bentuk integer.
- Normalisasi rating membantu model belajar lebih efektif selama proses pelatihan.

### Books Variable Cleaning

#### Handling Missing Values

Seluruh nilai yang hilang pada dataset `books` telah berhasil ditangani. Berikut rekap kolom dan jumlah nilai hilang yang tersisa:

| Kolom                | Missing Values |
|----------------------|----------------|
| ISBN                 | 0              |
| Book-Title           | 0              |
| Book-Author          | 0              |
| Year-Of-Publication  | 0              |
| Publisher            | 0              |
| Image-URL-S          | 0              |
| Image-URL-M          | 0              |
| Image-URL-L          | 0              |

Tidak ada nilai yang hilang setelah proses pembersihan dengan total 271357 baris dan 8 kolom.

#### Handling Outliers of Year Of Publication

Untuk mendeteksi outlier pada kolom `Year-Of-Publication`, digunakan metode **Interquartile Range (IQR)**. Berikut langkah dan rumus yang digunakan:

1. Hitung kuartil pertama (Q1) dan kuartil ketiga (Q3):
   - Q1 = Kuartil ke-25
   - Q3 = Kuartil ke-75

2. Hitung IQR:
   \[
   IQR = Q3 - Q1
   \]

3. Tentukan batas bawah dan atas:
   \[
   \text{Lower Bound} = Q1 - 1.5 \times IQR
   \]
   \[
   \text{Upper Bound} = Q3 + 1.5 \times IQR
   \]

4. Nilai di luar rentang tersebut dianggap sebagai **outlier**.

**Hasil Perhitungan:**

- Q1 dan Q3 diperoleh dari data `Year-Of-Publication` setelah dikonversi menjadi numerik dan menghapus nilai NaN.
- Diperoleh:
  - **IQR** = 11.0
  - **Batas Bawah (Lower Bound)** = 1972
  - **Batas Atas (Upper Bound)** = 2016

**Rentang Tahun Publikasi yang Diterima: 1972 hingga 2016**

* Nilai di luar rentang ini dianggap sebagai outlier dan perlu ditangani pada proses pembersihan data.
* Terdapat **262.262 baris** dan **8 kolom** yang tersisa pada dataset buku setelah pembersihan dilakukan.

Berikut grafik visualisasinya:

![Books Variable - Handling Outliers of Year Of Publication](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/booksvar-handleoutlies.png?raw=true)

Insight:

* Box plot terbaru menunjukkan distribusi tahun terbit yang lebih bersih dan konsisten tanpa outlier ekstrem
* Sebagian besar buku diterbitkan antara awal 1990-an hingga awal 2000-an, terlihat dari persebaran dan median pada box plot

### Users Variable

#### Handling Missing Values of User Age

Dalam tahap ini, dilakukan pembersihan data usia pengguna agar lebih representatif dan bebas dari outlier maupun nilai kosong.

```python
users_cleaned = users.copy() 

users_cleaned = users_cleaned[(users_cleaned['Age'] >= 5) & (users_cleaned['Age'] <= 100)]

median_age = users_cleaned['Age'].median()

users_cleaned['Age'] = users_cleaned['Age'].fillna(median_age)

print(f"Users cleaned shape: {users_cleaned.shape}")
```

Output:

```
Users cleaned shape: (166848, 3)
```

Insight:

* Data dengan usia tidak logis seperti kurang dari 5 tahun dan lebih dari 100 tahun dihapus karena dianggap outlier.
* Nilai kosong pada kolom usia diisi dengan nilai median agar distribusi tidak terlalu terpengaruh.
* Hasil akhir menunjukkan dataset users_cleaned memiliki 166848 baris data pengguna dengan 3 kolom yang valid.

#### Handling Outliers of User Age

Grafiknya sebagai berikut:

![Users Variable - Handling Outliers of User Age](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/usersvar-handleoutliers.png?raw=true)

Insight:

* Box plot menunjukkan distribusi usia pengguna yang terkonsentrasi antara usia 20 hingga 60 tahun
* Mayoritas pengguna berada pada rentang usia dewasa muda hingga paruh baya, dengan median sekitar usia 35 tahun
* Masih terdapat beberapa nilai usia yang berada di ujung atas (80–100), namun tetap dalam batas wajar

### Ratings Variable

#### Drop Rating 0

Data rating dengan nilai 0 dihapus karena dianggap tidak memberikan feedback yang berguna. Pembersihan ini memastikan hanya interaksi dengan umpan balik nyata yang dianalisis dan meningkatkan akurasi dalam sistem rekomendasi dan analisis preferensi pengguna.

```python
ratings_cleaned = ratings[ratings['Book-Rating'] > 0].copy()

print(f"Ratings after removing 0s: {ratings_cleaned.shape}")
```

Setelah penghapusan, jumlah data rating yang valid tersisa sebanyak 433671 baris dengan 3 kolom.

#### Distribution of Explicit Books Rating

Berikut grafik visualisasinya:

![Users Variable - Distribution of Explicit Books Rating](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/ratingsvar-distributionofexplicitbooksrating.png?raw=true)

#### Merging Data

Dataset terdiri dari tiga komponen utama: data buku (books.csv), data pengguna (users.csv), dan data rating (ratings.csv).

Penggabungan ini bertujuan untuk:
- Menyatukan informasi metadata buku seperti judul, penulis, dan sinopsis yang diperlukan dalam pendekatan Content-Based Filtering.
- Menyediakan interaksi user-item berupa rating yang dibutuhkan untuk Collaborative Filtering.

```python
ratings_books = ratings_cleaned.merge(books, on='ISBN', how='inner')

print(f"Merged (ratings + books): {ratings_books.shape}")
```
Merged (ratings + books): (373410, 10)

```python
all_data = ratings_books.merge(users_cleaned, on='User-ID', how='inner')

print(f"Final merged shape (ratings + books + users): {all_data.shape}")
```
Final merged shape (ratings + books + users): (260647, 12)

Insight:

* Dataset akhir terdiri dari 260647 entri dengan 12 kolom yang mencakup informasi lengkap pengguna, buku, dan rating

---

## Modelling

### Model Development with Content Based Filtering (CBF)

Sistem rekomendasi ini menggunakan pendekatan Content-Based Filtering (CBF) dengan memanfaatkan kemiripan antar buku berdasarkan sinopsis atau metadata. Proses dimulai dengan mentransformasi teks menjadi representasi numerik menggunakan metode TF-IDF (Term Frequency-Inverse Document Frequency). Kemudian, cosine similarity digunakan untuk mengukur kedekatan antar vektor buku. Buku dengan nilai kemiripan tertinggi dibandingkan buku yang disukai pengguna sebelumnya akan direkomendasikan.

#### Pendekatan 1: Content-Based Filtering

- Menghapus duplikasi data buku dan rating.
- Menggunakan *TF-IDF vectorization* pada judul dan penulis buku.
- Menggunakan *cosine similarity* untuk menemukan buku yang paling mirip berdasarkan konten.
- Output: Top-5 buku yang direkomendasikan berdasarkan kemiripan konten.

Langkah tambahan:
- Melakukan encoding pada `User-ID` dan `ISBN` menggunakan `LabelEncoder`.
- Mengubah `Book-Rating` dari tipe integer menjadi float.
- Menormalisasi rating ke skala 0–1 untuk digunakan pada model jaringan saraf.
- Membagi data menjadi data pelatihan dan pengujian dengan rasio 80:20.

Kelebihan:
- Cocok untuk pengguna baru (*cold start*)
- Mudah diinterpretasikan karena berdasarkan fitur buku

Kekurangan:
- Mengabaikan perilaku pengguna
- Terbatas pada fitur item yang sudah ada

##### Output Content-Based Filtering

![preview-output-cbf](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/get-recommendation.png?raw=true)

Insight:

* Fungsi get_book_recommendations mengambil masukan berupa judul buku, dan menghitung kemiripan cosine antar buku menggunakan vektor TF-IDF
* Mekanisme filter tambahan diterapkan untuk menghindari dominasi dari satu penulis saja (maksimal 30% dari total rekomendasi) dan menghindari duplikasi semantik
* Hasil rekomendasi menunjukkan kombinasi dari buku dengan kemiripan tema, kata kunci, atau struktur judul, tetapi tetap mempertimbangkan keberagaman penulis
* Visualisasi cover buku ditampilkan jika URL gambar tersedia dan valid, meningkatkan pengalaman pengguna dalam mengeksplorasi rekomendasi
* Contoh hasil: Buku *Rich Dad, Poor Dad* merekomendasikan buku-buku serupa dalam tema keuangan, keluarga, atau dengan struktur judul yang mirip namun dari penulis yang beragam

#### Pendekatan 2: Collaborative Filtering (RecommenderNet)
- Pemahaman Data
- Persiapan Data:
  1. Melakukan encoding pada `User-ID` dan `ISBN` menggunakan `LabelEncoder`.
  2. Mengubah nama kolom untuk memudahkan proses modeling.
  3. Mengubah `Book-Rating` dari integer ke float.
- Menormalisasi rating ke skala 0–1 untuk digunakan dalam jaringan saraf.
- Membagi data menjadi pelatihan dan pengujian (80:20).
- Menggunakan *embedding layer* untuk pengguna dan buku.
- Prediksi dilakukan menggunakan hasil *dot product* ditambah bias pengguna dan item, kemudian diaktifkan dengan sigmoid.
- Output: Top-5 buku dengan prediksi rating tertinggi untuk pengguna tertentu.

Kelebihan:
- Belajar dari pola interaksi pengguna
- Memberikan rekomendasi yang lebih personal

Kekurangan:
- Tidak bisa merekomendasikan untuk pengguna atau item baru
- Membutuhkan jumlah data yang besar untuk hasil pelatihan yang baik

##### Output Collaborative Filtering

![preview-output-cf1](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/cf-model1.png?raw=true)

![preview-output-cf2](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/cf-model2.png?raw=true)

Insight:

**3 Buku dengan Rating ≥ 8.0 oleh User 110887**

1. **In the Heart of the Sea: The Tragedy of the Whaleship Essex** — Nat Philbrick (Penguin Books)
2. **Driven** — W. G. Griffiths (Warner Faith)
3. **About Three Bricks Shy of a Load** — Mel Blount (Ballantine Books)

User ini memiliki preferensi terhadap buku bertema sejarah, petualangan nyata, dan olahraga atau motivasi — bisa jadi cenderung ke non-fiksi atau novel dengan latar yang kuat.

**5 Rekomendasi Buku untuk User 110887**

1. **Lonesome Dove** — Larry McMurtry (Pocket)
2. **Free** — Paul Vincent (Upfront Publishing)
3. **The Return of the King (The Lord of the Rings, Part 3)** — J.R.R. TOLKIEN (Del Rey)
4. **Harry Potter and the Sorcerer's Stone (Book 1)** — J.K. Rowling (Scholastic)
5. **My Sister's Keeper: A Novel** — Jodi Picoult (Atria)

Rekomendasi model cenderung mengarah pada fiksi populer, termasuk fantasi dan drama keluarga. Ada perbedaan preferensi antara buku yang telah diberi rating tinggi (non-fiksi, petualangan nyata) dengan rekomendasi dari model (fiksi populer). Ini menunjukkan bahwa model kolaboratif kemungkinan besar mengandalkan kesamaan pola rating dengan user lain yang memiliki selera lebih umum.

---

## Evaluation

Model RecommenderNet merupakan model neural network berbasis embedding yang dirancang untuk mempelajari representasi pengguna dan item dalam ruang vektor. Evaluasi model dilakukan setelah proses pelatihan dengan langkah-langkah berikut:

1. Data dibagi menjadi data pelatihan dan data validasi.
2. Model dilatih menggunakan fungsi loss Mean Squared Error (MSE) dan optimizer Adam.
3. Setelah pelatihan, prediksi rating dilakukan pada data validasi.
4. Hasil prediksi dibandingkan dengan rating aktual untuk menghitung RMSE (Root Mean Square Error) sebagai metrik evaluasi akurasi.

Evaluasi ini membantu mengukur keberhasilan model dalam menjawab problem statement kedua, yaitu memberikan rekomendasi yang dipersonalisasi meskipun interaksi pengguna masih terbatas.

### Visualize Metrics

![Visualize Metrics](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/mse-rmse-mae.png?raw=true)

Insight:

1. **Loss per Epoch (MSE)**

   * Baik loss pada data training maupun validasi menunjukkan tren penurunan yang konsisten.
   * Setelah epoch ke-5, penurunan pada loss validasi mulai melambat dan cenderung mendatar.
   * Ini menunjukkan bahwa model masih belajar dengan baik dan belum terlihat tanda overfitting yang signifikan.

2. **RMSE per Epoch**

   * RMSE pada data training terus menurun hingga mencapai sekitar 0.15.
   * RMSE pada data validasi juga menurun, meskipun konvergensi terjadi pada nilai sekitar 0.19.
   * Selisih antara RMSE training dan validasi cukup kecil, menandakan bahwa model memiliki generalisasi yang baik dan tidak mengalami high variance.

3. **MAE per Epoch**

   * MAE pada data training menurun secara stabil hingga mencapai sekitar 0.11.
   * MAE pada data validasi juga menurun namun lebih lambat, dan berhenti turun signifikan di kisaran 0.148.
   * Ini konsisten dengan tren pada RMSE, menunjukkan stabilitas performa model di data yang tidak dilatih.

Kesimpulan:

* Model memiliki performa yang baik dan stabil di data training maupun validasi.
* Tidak ada indikasi overfitting atau underfitting yang jelas.

---

## Keterkaitan Hasil Evaluasi dengan Business Understanding

### Apakah model menjawab setiap problem statement?

Masalah 1: Bagaimana cara membantu pengguna menemukan buku yang sesuai dengan minat mereka?

Terjawab. Sistem rekomendasi berhasil memberikan saran buku yang relevan berdasarkan preferensi pengguna melalui dua pendekatan utama: Content-Based Filtering dan Collaborative Filtering. Pada pendekatan Content-Based, kemiripan dihitung berdasarkan judul dan penulis buku. Sedangkan pada pendekatan Collaborative, model RecommenderNet dilatih untuk mengenali pola rating dan memberikan rekomendasi berdasarkan interaksi historis.

Masalah 2: Bagaimana cara memberikan rekomendasi buku meskipun pengguna belum memberikan banyak rating?

Terjawab. Pendekatan Content-Based Filtering dapat digunakan ketika data interaksi pengguna masih terbatas, karena hanya bergantung pada metadata buku. Hal ini memungkinkan sistem tetap memberikan rekomendasi berdasarkan kemiripan konten, tanpa harus menunggu banyak rating dari pengguna.

### Apakah model berhasil mencapai goals?

Tujuan 1: Membangun sistem rekomendasi yang menyarankan buku berdasarkan kemiripan konten (misalnya judul, penulis).

Tercapai. Model TF-IDF dan cosine similarity berhasil mengidentifikasi kemiripan antar buku berdasarkan fitur teks, dan mengembalikan hasil rekomendasi yang relevan secara kontekstual dengan buku yang disukai pengguna.

Tujuan 2: Melatih model yang belajar dari interaksi rating pengguna dan buku untuk memberikan rekomendasi yang dipersonalisasi.

Tercapai. Model Collaborative Filtering dengan arsitektur RecommenderNet telah dilatih menggunakan embedding layer dan menunjukkan kemampuan prediksi terhadap rating pengguna. Evaluasi menggunakan metrik RMSE menunjukkan performa model yang dapat diterima.

### Apakah solusi yang direncanakan berdampak?

Solusi 1: Sistem dapat memberikan rekomendasi yang dipersonalisasi bahkan pada user baru, berkat integrasi dua pendekatan yang saling melengkapi.

Solusi 2: Hasil evaluasi pada model collaborative menunjukkan model dapat mengenali pola rating dan memberikan rekomendasi yang sesuai minat pengguna, meskipun pada beberapa kasus cold-start tetap menjadi tantangan.

### Kesimpulan

Evaluasi menunjukkan bahwa sistem rekomendasi berbasis Content-Based dan Collaborative Filtering mampu menjawab pernyataan masalah, memenuhi tujuan bisnis, dan memberikan solusi yang berdampak nyata. Sistem ini layak diimplementasikan sebagai fitur personalisasi dalam platform e-commerce buku untuk meningkatkan pengalaman pengguna dan retensi jangka panjang.

---

## Referensi

## Referensi

\[1] M. B. Kim, H. Lee, dan Y. J. Hwang, “When E-Commerce Personalization Systems Show and Tell: Investigating the Relative Persuasive Appeal of Content-Based versus Collaborative Filtering,” *Journal of Advertising*, vol. 50, no. 2, pp. 123–135, 2021. \[Online]. Tersedia: [https://doi.org/10.1080/00913367.2021.1887013](https://doi.org/10.1080/00913367.2021.1887013)

\[2] I. Novitasari, D. W. Sari, dan A. Setiawan, “Improving Recommender Systems using Hybrid Techniques of Collaborative Filtering and Content-Based Filtering,” *Journal of Artificial Intelligence and Data Science*, vol. 4, no. 3, pp. 175–182, 2022. \[Online]. Tersedia: [https://doi.org/10.47738/jads.v4i3.115](https://doi.org/10.47738/jads.v4i3.115)

\[3] A. K. Mishra dan P. Kumar, “A Brief Analysis of Collaborative and Content-Based Filtering Algorithms,” dalam *IOP Conference Series: Materials Science and Engineering*, vol. 981, no. 2, 2020. \[Online]. Tersedia: [https://doi.org/10.1088/1757-899X/981/2/022008](https://doi.org/10.1088/1757-899X/981/2/022008)
