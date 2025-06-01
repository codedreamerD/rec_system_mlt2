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

![Users Variable - Distribution of Users by Age Group](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/usersvar-distributionofusers.png?raw=true)

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

![booksvar-handleoutliers](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/booksvar-handleoutliers.png?raw=true)

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

![usersvar-handleoutliers](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/usersvar-handleoutliers.png?raw=true)

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

![ratingsvar-distributionofexplicitbooksrating](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/ratingsvar-distributionofexplicitbooksrating.png?raw=true)

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

### Content Based Filtering Data

#### Handling Duplicates Data

```python
cbf_data = all_data[['Book-Title', 'Book-Author', 'Publisher']].drop_duplicates()
cbf_data = cbf_data.drop_duplicates(subset=['Book-Title', 'Book-Author'], keep='first').reset_index(drop=True)
```

Insight:

* Penghapusan duplikasi dilakukan dua tahap: pertama berdasarkan seluruh kolom, kemudian khusus berdasarkan kombinasi judul dan penulis untuk memastikan setiap buku direpresentasikan satu kali dalam sistem rekomendasi.

#### Merging Data

```python
cbf_data['combined_features'] = cbf_data['Book-Title'] + ' ' + cbf_data['Book-Author']
cbf_data['combined_features'] = cbf_data['combined_features'].fillna('')
```

Insight:

* Penggabungan fitur judul dan penulis membentuk satu representasi teks yang menjadi dasar ekstraksi fitur pada model Content-Based Filtering, karena informasi ini menggambarkan karakteristik unik setiap buku.

#### Title Normalization

```python
cbf_data['Book-Title-Lower'] = cbf_data['Book-Title'].str.lower()
indices = pd.Series(cbf_data.index, index=cbf_data['Book-Title-Lower']).drop_duplicates()

def normalize_title(title):
    title = title.lower()
    title = re.sub(r'[^a-z\s]', '', title)
    title = re.sub(r'\s+', ' ', title).strip()
    return title
```

Insight:

* Fungsi normalize_title untuk menghapus variasi tidak relevan dalam dari judul buku, sehingga meminimalkan duplikasi semantik yang dapat mengganggu akurasi sistem pencarian dan rekomendasi.

#### TF-IDF Vectorization

```python
tfidf = TfidfVectorizer()

tfidf_matrix = tfidf.fit_transform(cbf_data['combined_features'])

vocab = tfidf.get_feature_names_out()

print(f"Unique features (token): {len(vocab)}")
print(vocab[:20])

print(f"n\TF-IDF Matrix Dimension: {tfidf_matrix.shape}")
```

Insight:

* Duplikasi dihapus berdasarkan kombinasi judul dan penulis untuk menjaga keunikan setiap buku
* Hasil vektorisasi menghasilkan 71.169 token unik, menunjukkan keragaman kata dalam judul dan nama penulis
* Beberapa token awal menunjukkan adanya karakter angka atau kode, yang dapat berasal dari judul/penulis non-alfabetik atau kode internal dalam data
* Matriks TF-IDF memiliki dimensi (108309, 71169), menunjukkan terdapat 108309 buku unik dan 71169 fitur kata unik

### Collaborative Filtering Data

#### Selecting Relevant Columns

```python
cf_data = all_data[['User-ID', 'ISBN', 'Book-Rating']].copy()

cf_data.info()
```

Insight:

* Dataset yang digunakan terdiri dari 260647 entri, mencakup User-ID, ISBN, dan Book-Rating
* Semua entri memiliki data lengkap tanpa nilai yang hilang
* Tipe data terdiri dari ID pengguna dan rating dalam bentuk numerik (int64), serta ISBN sebagai identifikasi unik buku dalam bentuk object

#### Encoding User and Item Identifiers

```python
user_encoder = LabelEncoder()
item_encoder = LabelEncoder()

cf_data['user'] = user_encoder.fit_transform(cf_data['User-ID'])
cf_data['item'] = item_encoder.fit_transform(cf_data['ISBN'])
cf_data
```

Insight:

* Dataset akhir memiliki lima kolom dan 260647 baris, siap digunakan sebagai input dalam sistem rekomendasi berbasis Collaborative Filtering

#### Converting Type

```python
cf_data['rating'] = cf_data['Book-Rating'].astype(float)
```

Insight:

* Kolom rating telah dikonversi ke dalam tipe data float untuk mendukung komputasi matematis dalam model ML

#### Prepare Final Dataset Structure

```
cf_data = cf_data[['user', 'item', 'rating']]
cf_data
```

Insight:

* Dataset CF telah dibersihkan dan disederhanakan menjadi user, item, dan rating
* Dataset akhir terdiri dari 260647 interaksi pengguna dengan buku, siap digunakan untuk pelatihan dan evaluasi model sistem rekomendasi

#### Exploring Dataset Dimensions and Rating Scale

```
num_users = cf_data['user'].nunique()
num_items = cf_data['item'].nunique()

min_rating = cf_data['rating'].min()
max_rating = cf_data['rating'].max()

print(f"Number of Users       : {num_users}")
print(f"Number of Items       : {num_items}")
print(f"Minimum Rating Value  : {min_rating}")
print(f"Maximum Rating Value  : {max_rating}")
```

Insight:

* Dataset mencakup hampir 40 ribu pengguna dan lebih dari 115 ribu item buku, menunjukkan cakupan interaksi yang luas
* Skala rating bervariasi dari 1 hingga 10

#### Split Dataset into Training and Testing Sets

Data dibagi dengan proporsi 80% untuk pelatihan (train) dan 20% untuk pengujian (test).

```python
X = cf_data[['user', 'item']].values
y = cf_data['rating'].apply(lambda x: (x - min_rating) / (max_rating - min_rating)).values

X_train, X_test, y_train, y_test = train_test_split(
    X,    # input: user-item pairs
    y,    # target: normalized ratings
    test_size=0.2,
    random_state=42
)

print(f"X_train shape : {X_train.shape}")
print(f"X_test shape  : {X_test.shape}")
print(f"y_train shape : {y_train.shape}")
print(f"y_test shape  : {y_test.shape}")
```

Insight:

* Input berupa pasangan user dan item
* Target berupa skor rating yang telah dinormalisasi ke rentang 0 hingga 1

---

## Modelling

### Model Development with Content Based Filtering (CBF)

Sistem rekomendasi ini menggunakan pendekatan Content-Based Filtering (CBF) dengan memanfaatkan kemiripan antar buku berdasarkan sinopsis atau metadata. Proses dimulai dengan mentransformasi teks menjadi representasi numerik menggunakan metode TF-IDF (Term Frequency-Inverse Document Frequency). Kemudian, cosine similarity digunakan untuk mengukur kedekatan antar vektor buku. Buku dengan nilai kemiripan tertinggi dibandingkan buku yang disukai pengguna sebelumnya akan direkomendasikan.

```python
def get_book_recommendations(title, k=5, max_author_ratio=0.3):
    """
    Return top-k diverse books based on TF-IDF cosine similarity.
    Limits max percentage of books from the same author.
    Excludes semantic duplicates based on normalized title + author.
    """
    title = title.lower()

    if title not in indices:
        print(f"Book title '{title}' not found in the dataset.")
        return

    idx = indices[title]
    if isinstance(idx, pd.Series):
        idx = idx.iloc[0]

    if idx >= tfidf_matrix.shape[0]:
        print(f"Index {idx} is out of range for the TF-IDF matrix.")
        return

    input_title_norm = normalize_title(cbf_data.loc[idx, 'Book-Title'])
    input_author = cbf_data.loc[idx, 'Book-Author'].lower()

    sim_scores = cosine_similarity(tfidf_matrix[idx], tfidf_matrix).flatten()
    top_indices = sim_scores.argsort()[::-1]

    max_per_author = max(1, floor(k * max_author_ratio))
    author_counts = defaultdict(int)
    filtered = []

    for i in top_indices:
        if i == idx:
            continue

        candidate = cbf_data.iloc[i]
        candidate_title_norm = normalize_title(candidate['Book-Title'])
        candidate_author = candidate['Book-Author'].lower()

        if candidate_title_norm == input_title_norm and candidate_author == input_author:
            continue

        if author_counts[candidate_author] >= max_per_author:
            continue

        filtered.append(i)
        author_counts[candidate_author] += 1

        if len(filtered) == k:
            break

    if not filtered:
        print("No diverse recommendations found.")
        return

    print("Input Book:")
    input_book = cbf_data.loc[idx]
    print(f"Title     : {input_book['Book-Title']}")
    print(f"Author    : {input_book['Book-Author']}")
    print(f"Publisher : {input_book['Publisher']}")

    print(f"\nTop {k} Recommendations:\n")

    for i, index in enumerate(filtered, start=1):
        rec = cbf_data.iloc[index]
        print(f"#{i}")
        print(f"Title     : {rec['Book-Title']}")
        print(f"Author    : {rec['Book-Author']}")
        print(f"Publisher : {rec['Publisher']}")
        print("-" * 40)

get_book_recommendations("Rich Dad, Poor Dad: What the Rich Teach Their Kids About Money--That the Poor and Middle Class Do Not!")
```

Outputnya sebagai berikut:

**Input Book**

| Field      | Detail                                                                 |
|------------|------------------------------------------------------------------------|
| **Title**     | Rich Dad, Poor Dad: What the Rich Teach Their Kids About Money--That the Poor and Middle Class Do Not! |
| **Author**    | Robert T. Kiyosaki                                                  |
| **Publisher** | Warner Books                                                        |

---

**Top 5 Recommendations**

| Rank | Title                                                                 | Author             | Publisher                |
|------|------------------------------------------------------------------------|---------------------|---------------------------|
| #1   | Rich Dad's Guide to Investing: What the Rich Invest in, That the Poor and the Middle Class Do Not! | Robert T. Kiyosaki | Warner Business Books     |
| #2   | Rich Man, Poor Man                                                    | Irwin Shaw          | Dell Publishing Company   |
| #3   | Dad's Back (Dad and Me)                                               | Jan Ormerod         | Walker Books              |
| #4   | The Politics of Rich and Poor: Wealth and the American Electorate in the Reagan Aftermath | Kevin Phillips      | Harper Audio              |
| #5   | Dad                                                                    | William Wharton     | Avon Books                |

Insight:

* Fungsi get_book_recommendations mengambil masukan berupa judul buku, dan menghitung kemiripan cosine antar buku menggunakan vektor TF-IDF
* Mekanisme filter tambahan diterapkan untuk menghindari dominasi dari satu penulis saja (maksimal 30% dari total rekomendasi) dan menghindari duplikasi semantik
* Hasil rekomendasi menunjukkan kombinasi dari buku dengan kemiripan tema, kata kunci, atau struktur judul, tetapi tetap mempertimbangkan keberagaman penulis
* Visualisasi cover buku ditampilkan jika URL gambar tersedia dan valid, meningkatkan pengalaman pengguna dalam mengeksplorasi rekomendasi
* Contoh hasil: Buku *Rich Dad, Poor Dad* merekomendasikan buku-buku serupa dalam tema keuangan, keluarga, atau dengan struktur judul yang mirip namun dari penulis yang beragam

### Model Development with Collaborative Filtering (CF)

Model Collaborative Filtering dikembangkan menggunakan pendekatan berbasis embedding, dengan arsitektur RecommenderNet. Model ini mempelajari representasi laten pengguna dan item dalam ruang vektor berdimensi rendah, yang kemudian digunakan untuk memprediksi preferensi pengguna terhadap buku yang belum pernah mereka rating.

#### Define RecommenderNet Architecture

```python
class RecommenderNet(keras.Model):
    def __init__(self, num_users, num_items, embedding_size=50, **kwargs):
        super(RecommenderNet, self).__init__(**kwargs)

        self.user_embedding = layers.Embedding(
            input_dim=num_users,
            output_dim=embedding_size,
            embeddings_initializer='he_normal',
            embeddings_regularizer=keras.regularizers.l2(1e-6)
        )
        self.user_bias = layers.Embedding(input_dim=num_users, output_dim=1)

        self.item_embedding = layers.Embedding(
            input_dim=num_items,
            output_dim=embedding_size,
            embeddings_initializer='he_normal',
            embeddings_regularizer=keras.regularizers.l2(1e-6)
        )
        self.item_bias = layers.Embedding(input_dim=num_items, output_dim=1)

    def call(self, inputs):
        user_vector = self.user_embedding(inputs[:, 0])
        user_bias = self.user_bias(inputs[:, 0])
        item_vector = self.item_embedding(inputs[:, 1])
        item_bias = self.item_bias(inputs[:, 1])

        dot_user_item = tf.reduce_sum(user_vector * item_vector, axis=1, keepdims=True)
        result = dot_user_item + user_bias + item_bias

        return tf.nn.sigmoid(result)
```

Insight:

* Model menggunakan pendekatan embedding untuk menangkap representasi pengguna dan item.
* Skor akhir dihitung dari dot product + bias pengguna + bias item.

#### Instantiate & Compile the Model

```python
num_users = cf_data['user'].nunique()
num_items = cf_data['item'].nunique()

model = RecommenderNet(num_users, num_items)
model.compile(
    loss='mse',
    optimizer=tf.keras.optimizers.Adam(learning_rate=0.001),
    metrics=[
        tf.keras.metrics.RootMeanSquaredError(),
        tf.keras.metrics.MeanAbsoluteError()
    ]
)
```

Insight:

* Model di-*compile* dengan loss `MSE` dan metrik evaluasi `RMSE` serta `MAE`.
* Learning rate yang digunakan adalah 0.001.

#### Train the Model

```python
history = model.fit(
    x=X_train,
    y=y_train,
    batch_size=64,
    epochs=10,
    verbose=1,
    validation_data=(X_test, y_test)
)
```

Insight:

* Model converge dengan baik: terlihat dari penurunan loss, MAE, dan RMSE.
* Overfitting belum terjadi hingga epoch ke-10.

#### Generate Personalized Book Recommendations

```python
def show_high_rated_book_and_recommendation(original_user_id, top_k=5, min_rating=8.0, top_k_rating=3):
    try:
        encoded_user_id = user_encoder.transform([original_user_id])[0]
    except ValueError:
        print(f"User-ID {original_user_id} not found in training data.")
        return pd.DataFrame()

    high_rated_df = cf_data[(cf_data['user'] == encoded_user_id) & (cf_data['rating'] >= min_rating)]
    top_rated_df = high_rated_df.sort_values(by='rating', ascending=False).head(top_k_rating)

    high_rated_isbn = item_encoder.inverse_transform(top_rated_df['item'])

    high_rated_books = books[books['ISBN'].isin(high_rated_isbn)][[
        'Book-Title', 'Book-Author', 'Publisher']].drop_duplicates().reset_index(drop=True)

    print(f"Top {top_k_rating} Books rated ≥ {min_rating} by user {original_user_id}:\n")
    if high_rated_books.empty:
        print("No high-rated books found.")
    else:
        for i, row in high_rated_books.iterrows():
            print(f"#{i+1}: {row['Book-Title']} by {row['Book-Author']} ({row['Publisher']})")
            print("-" * 40)

    user_all_rated_df = cf_data[cf_data['user'] == encoded_user_id]
    rated_items = user_all_rated_df['item'].values

    all_items = np.arange(num_items)
    unrated_items = np.setdiff1d(all_items, rated_items)

    user_input = np.array([[encoded_user_id, item] for item in unrated_items])

    predicted_ratings = model.predict(user_input, verbose=0).flatten()

    top_indices = predicted_ratings.argsort()[-top_k:][::-1]

    recommended_item_ids = unrated_items[top_indices]
    recommended_isbn = item_encoder.inverse_transform(recommended_item_ids)
    recommended_books = books[books['ISBN'].isin(recommended_isbn)][[
        'Book-Title', 'Book-Author', 'Publisher']].drop_duplicates().reset_index(drop=True)

    print(f"\nTop {top_k} Book Recommendations for User {original_user_id}:\n")
    if recommended_books.empty:
        print("No recommendations available.")
    else:
        for i, row in recommended_books.iterrows():
            print(f"#{i+1}: {row['Book-Title']} by {row['Book-Author']} ({row['Publisher']})")
            print("-" * 40)
```

Insight:

* Fungsi ini menampilkan kombinasi antara top-rated books oleh user dan hasil rekomendasi model.
* Rekomendasi berdasarkan prediksi rating tertinggi untuk item yang belum dirating user.

#### Test the Function on a Random User

```python
unique_users = np.unique(X_train[:, 0])
original_user_ids = user_encoder.inverse_transform(unique_users)
random_user_id = random.choice(original_user_ids)

print(f"Get high rated books and recommendation for user ID: {random_user_id}")
show_high_rated_book_and_recommendation(random_user_id)
```

Outputnya sebagai berikut:

Get high rated books and recommendation for user ID: 48787
Top 3 Books rated ≥ 8.0 by user 48787:

| Peringkat | Judul Buku                                          | Penulis                  | Penerbit             |
|-----------|------------------------------------------------------|--------------------------|-----------------------|
| #1        | *Come to Grief*                                      | Dick Francis             | Macmillan Pub Ltd     |
| #2        | *Fat Girl Dances With Rocks (Coming of Age)*         | Susan Stinson            | Spinsters Ink Books   |
| #3        | *Rainforest*                                         | Jenny Diski              | Penguin USA           |

Top 5 Book Recommendations for User 48787:

| Peringkat | Judul Buku                                                     | Penulis              | Penerbit            |
|-----------|----------------------------------------------------------------|----------------------|----------------------|
| #1        | *Lonesome Dove*                                                | Larry McMurtry       | Pocket               |
| #2        | *Free*                                                         | Paul Vincent         | Upfront Publishing   |
| #3        | *The Return of the King (The Lord of the Rings, Part 3)*       | J.R.R. Tolkien       | Del Rey              |
| #4        | *Harry Potter and the Sorcerer's Stone (Book 1)*               | J.K. Rowling         | Scholastic           |
| #5        | *My Sister's Keeper : A Novel (Picoult, Jodi)*                 | Jodi Picoult         | Atria                |

Insight untuk User Random (48787):

**Tidak Ada Buku Rating Tinggi**

* User tidak memiliki rating ≥ 8.0 di data pelatihan.
* Kemungkinan user ini masih baru (*cold start*) atau cenderung memberi rating rendah.

**Rekomendasi dari Model**:

1. *Lonesome Dove* — Larry McMurtry (Pocket)
2. *Free* — Paul Vincent (Upfront Publishing)
3. *The Return of the King (The Lord of the Rings, Part 3)* — J.R.R. Tolkien (Del Rey)
4. *Harry Potter and the Sorcerer's Stone (Book 1)* — J.K. Rowling (Scholastic)
5. *My Sister's Keeper* — Jodi Picoult (Atria)

> Rekomendasi ini logis karena model mengandalkan tren umum dari pengguna lain dan genre populer.

#### Test the Function on Specific User ID

```python
show_high_rated_book_and_recommendation(110887)
```

Outputnya sebagai berikut:

Top 3 Books rated ≥ 8.0 by user 110887:

| Peringkat | Judul Buku                                                                 | Penulis         | Penerbit           |
|-----------|------------------------------------------------------------------------------|-----------------|---------------------|
| #1        | *In the Heart of the Sea: The Tragedy of the Whaleship Essex*              | Nat Philbrick   | Penguin Books       |
| #2        | *Driven*                                                                    | W. G. Griffiths | Warner Faith        |
| #3        | *About Three Bricks Shy of a Load*                                          | Mel Blount      | Ballantine Books    |

Top 5 Book Recommendations for User 110887:

| Peringkat | Judul Buku                                                     | Penulis              | Penerbit            |
|-----------|----------------------------------------------------------------|----------------------|----------------------|
| #1        | *Lonesome Dove*                                                | Larry McMurtry       | Pocket               |
| #2        | *Free*                                                         | Paul Vincent         | Upfront Publishing   |
| #3        | *The Return of the King (The Lord of the Rings, Part 3)*       | J.R.R. Tolkien       | Del Rey              |
| #4        | *Harry Potter and the Sorcerer's Stone (Book 1)*               | J.K. Rowling         | Scholastic           |
| #5        | *My Sister's Keeper : A Novel (Picoult, Jodi)*                 | Jodi Picoult         | Atria                |

Insight untuk User 110887:

**3 Buku dengan Rating ≥ 8.0**:

1. *In the Heart of the Sea* — Nat Philbrick (Penguin Books)
2. *Driven* — W. G. Griffiths (Warner Faith)
3. *About Three Bricks Shy of a Load* — Mel Blount (Ballantine Books)

> User ini tampaknya menyukai buku bertema sejarah nyata, petualangan, dan olahraga — cenderung ke non-fiksi.

**Rekomendasi Model**:

1. *Lonesome Dove* — Larry McMurtry (Pocket)
2. *Free* — Paul Vincent (Upfront Publishing)
3. *The Return of the King* — J.R.R. Tolkien (Del Rey)
4. *Harry Potter and the Sorcerer's Stone* — J.K. Rowling (Scholastic)
5. *My Sister's Keeper* — Jodi Picoult (Atria)

> Ada gap antara preferensi aktual (non-fiksi) dan rekomendasi model (fiksi populer), menunjukkan bahwa model kolaboratif sangat dipengaruhi oleh pola umum pengguna lain.

## Evaluation

Model RecommenderNet merupakan model neural network berbasis embedding yang dirancang untuk mempelajari representasi pengguna dan item dalam ruang vektor. Evaluasi model dilakukan setelah proses pelatihan dengan langkah-langkah berikut:

1. Data dibagi menjadi data pelatihan dan data validasi.
2. Model dilatih menggunakan fungsi loss Mean Squared Error (MSE) dan optimizer Adam.
3. Setelah pelatihan, prediksi rating dilakukan pada data validasi.
4. Hasil prediksi dibandingkan dengan rating aktual untuk menghitung RMSE (Root Mean Square Error) sebagai metrik evaluasi akurasi.

Evaluasi ini membantu mengukur keberhasilan model dalam menjawab problem statement kedua, yaitu memberikan rekomendasi yang dipersonalisasi meskipun interaksi pengguna masih terbatas.

Output training selama 10 epoch:

| Epoch | Loss   | MAE    | RMSE   | Val Loss | Val MAE | Val RMSE |
|-------|--------|--------|--------|----------|---------|----------|
| 1     | 0.0902 | 0.2608 | 0.2998 | 0.0659   | 0.2144  | 0.2538   |
| 2     | 0.0578 | 0.1960 | 0.2358 | 0.0534   | 0.1849  | 0.2250   |
| 3     | 0.0448 | 0.1652 | 0.2042 | 0.0472   | 0.1717  | 0.2118   |
| 4     | 0.0376 | 0.1494 | 0.1878 | 0.0431   | 0.1637  | 0.2037   |
| 5     | 0.0327 | 0.1381 | 0.1762 | 0.0406   | 0.1582  | 0.1984   |
| 6     | 0.0293 | 0.1296 | 0.1676 | 0.0391   | 0.1545  | 0.1950   |
| 7     | 0.0269 | 0.1226 | 0.1605 | 0.0382   | 0.1520  | 0.1927   |
| 8     | 0.0248 | 0.1165 | 0.1540 | 0.0376   | 0.1503  | 0.1913   |
| 9     | 0.0234 | 0.1119 | 0.1495 | 0.0372   | 0.1492  | 0.1903   |
| 10    | 0.0222 | 0.1079 | 0.1456 | 0.0370   | 0.1484  | 0.1897   |

### Visualize Metrics

![mse-rmse-mae](https://github.com/codedreamerD/rec_system_mlt2/blob/main/repo-dir/mse-rmse-mae.png?raw=true)

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

\[1] M. B. Kim, H. Lee, dan Y. J. Hwang, “When E-Commerce Personalization Systems Show and Tell: Investigating the Relative Persuasive Appeal of Content-Based versus Collaborative Filtering,” *Journal of Advertising*, vol. 50, no. 2, pp. 123–135, 2021. \[Online]. Tersedia: [https://doi.org/10.1080/00913367.2021.1887013](https://doi.org/10.1080/00913367.2021.1887013)

\[2] I. Novitasari, D. W. Sari, dan A. Setiawan, “Improving Recommender Systems using Hybrid Techniques of Collaborative Filtering and Content-Based Filtering,” *Journal of Artificial Intelligence and Data Science*, vol. 4, no. 3, pp. 175–182, 2022. \[Online]. Tersedia: [https://doi.org/10.47738/jads.v4i3.115](https://doi.org/10.47738/jads.v4i3.115)

\[3] A. K. Mishra dan P. Kumar, “A Brief Analysis of Collaborative and Content-Based Filtering Algorithms,” dalam *IOP Conference Series: Materials Science and Engineering*, vol. 981, no. 2, 2020. \[Online]. Tersedia: [https://doi.org/10.1088/1757-899X/981/2/022008](https://doi.org/10.1088/1757-899X/981/2/022008)
