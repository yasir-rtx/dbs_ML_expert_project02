# Laporan Proyek 02 - Muhammad Yasir

# Domain Proyek
Dalam industri hiburan anime, penonton sering kali mengalami kesulitan dalam menemukan judul anime yang sesuai dengan preferensi mereka di tengah banyaknya pilihan yang tersedia. Hal ini diperparah dengan pertumbuhan eksponensial konten anime setiap tahunnya, yang membuat proses pencarian anime yang diinginkan menjadi lebih kompleks dan memakan waktu. Tanpa panduan atau rekomendasi yang tepat, penonton mungkin melewatkan anime yang sesuai dengan selera mereka dan berpotensi menikmati pengalaman menonton yang kurang memuaskan.

Oleh karena itu, diperlukan sebuah sistem rekomendasi anime yang mampu memberikan saran judul-judul anime yang relevan dan personal kepada pengguna berdasarkan preferensi dan perilaku mereka sebelumnya. Sistem ini tidak hanya membantu pengguna menemukan anime baru dengan lebih efisien, tetapi juga meningkatkan keterlibatan dan kepuasan mereka terhadap platform penyedia layanan streaming anime.

Berikut beberapa alasan mengapa proyek ini untuk diselesaikan:
1. Meningkatkan Pengalaman Pengguna
2. Mengatasi Masalah Overload Informasi
3. Peningkatan Keterlibatan Pengguna
4. Pemanfaatan Data untuk Keputusan Bisnis
5. Mendorong inovasi dalam bidang teknologi informasi
6. Mendorong eksposur dan apresiasi terhadap berbagai jenis anime

Berdasarkan jurnal penelitian 2024 yang ditulis Nazhif Muafa Roziqiin dan M. Faisal dengan judul Sistem Rekomendasi Pemilihan Anime Menggunakan User-Based Collaborative Filtering[1] menjelaskan metode User-Based Collaborative Filtering berdasarkan preferensi pengguna dapat digunakan untuk membangun sistem rekomendasi. Penelitian lain oleh Altolyto Sitanggang dkk dengan judul Sistem Rekomendasi Anime Menggunakan Metode Singular Value Decomposition (SVD) dan Cosine Similarity[2] menjelaskan cosine similarity dapat digunakan untuk mengukur kemiripan antar anime juga dapat dijadikan dasar sistem rekomendasi.

# Business Understanding

### Problem Statements
1. Bagaimana mengembangkan sistem rekomendasi yang akurat dan mampu membuat sistem rekomendasi yang personal berdasarkan preferensi pengguna serta mampu membuat rekomendasi berdasarkan kemiripan antara anime?
2. Bagaimana memanfaatkan fitur-fitur yang terdapat pada data set untuk membuat rekomendasi yang relevan bagi pengguna?

### Goals
1. Mengembangkan sistem rekomendasi anime yang akurat dan personal untuk setiap pengguna dan juga mampu memberikan rekomendasi berdasarkan kemiripan antar anime.
2. Memanfaatkan data interaksi pengguna dan fitur-fitur anime untuk memberikan rekomendasi yang relevan.

### Solution Statements
1. Mengembangkan sistem rekomendasi menggunakan metode content based filtering dengan memanfaatkan fitur genre untuk membuat sistem rekomendasi.
2. Mengembangkan sistem rekomendasi menggunakan metode collaborative filtering dengan memanfaatkan korelasi antara anime dengan rating yang diberikan oleh user.
# Data Understanding
Data set yang digunakan dalam proyek ini adalah "Anime Recommendations Database" yang tersedia di Kaggle. Data set ini berisi informasi tentang berbagai judul anime serta interaksi pengguna dengan anime tersebut. Data set ini didapat dari website [myanimelist](https://myanimelist.net/).

Informasi lebih lanjut dapat dilihat di [Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database).

### Struktur Data set: Data set ini terdiri dari dua file utama:
1. anime.csv (dimensi data 12294 baris x 7 kolom): file ini berisi informasi mengenai judul-judul anime. Beberapa kolom penting dalam file ini meliputi:
   * anime_id: Identifikasi unik untuk setiap anime.
   * name: Nama atau judul anime.
   * genre: Genre dari anime, seperti aksi, komedi, drama, dll.
   * type: Tipe anime, seperti TV, Movie, OVA, dll.
   * episodes: Jumlah episode dari anime.
   * rating: Rata-rata rating yang diberikan pengguna untuk anime tersebut.
   * members: Jumlah anggota komunitas yang menyukai atau mengikuti anime ini.

2. rating.csv (dimensi data 7813737  baris x 3 kolom): file ini berisi data interaksi pengguna dengan anime, berupa penilaian yang diberikan oleh pengguna. Beberapa kolom penting dalam file ini meliputi:
   * user_id: Identifikasi unik untuk setiap pengguna.
   * anime_id: Identifikasi unik untuk setiap anime (berhubungan dengan anime_id di file anime.csv).
   * rating: Rating yang diberikan oleh pengguna untuk anime tertentu (bernilai dari -1 hingga 10 dimana -1 berarti pengguna telah menonton anime tetapi tidak memberikan rating).

## Eksplorasi Data Analysis (EDA)

### Anime

#### Ringkasan Deskripsi Rating
Pada data set anime didapat informasi statistik rating terendah adalah 1.67 dan rating tertinggi adalah 10. Dan rata-rata rating yang diberikan pada anime tersebut adalah 6.47.
| item  |    Rating    |
|-------|--------------|
| count | 12064.000000 |
| mean  |     6.473902 |
| std   |     1.026746 |
| min   |     1.670000 |
| 25%   |     5.880000 |
| 50%   |     6.570000 |
| 75%   |     7.180000 |
| max   |    10.000000 |

Tabel 1. Deskripsi Statistik Rating pada Anime

### Rating

#### Ringkasan Deskripsi Rating
Pada data set rating didapat informasi statistik rating terendah adalah -1 dan rating tertinggi adalah 10. Dan rata-rata rating yang diberikan pada anime tersebut adalah 6.14.
| item  |     Rating    |
|-------|---------------|
| count |  7.813737e+06 |
| mean  |  6.144030e+00 |
| std   |  3.727800e+00 |
| min   | -1.000000e+00 |
| 25%   |  6.000000e+00 |
| 50%   |  7.000000e+00 |
| 75%   |  9.000000e+00 |
| max   |  1.000000e+01 |

Tabel 2. Deskripsi Statistik Rating pada Rating

### Plotting Distribusi Rating
![Distribusi_Rating](https://github.com/user-attachments/assets/2e3943c8-ab07-4861-b722-a6a4f8644f21)

Gambar 1. Distribusi Rating

Berdasarkan diagram bar di atas didapat informasi sebagai berikut :
   * Rating -1 memiliki jumlah tertinggi, sekitar 1.400.000.
   * Rating 1 hingga 5 memiliki jumlah yang relatif rendah, masing-masing kurang dari 200.000.
   * Rating 6 hingga 10 menunjukkan peningkatan jumlah, dengan puncaknya pada rating 8 yang mencapai sekitar 1.600.000.
   * Rating 7 dan 9 juga memiliki jumlah yang signifikan, masing-masing sekitar 1.200.000 dan 1.000.000.
   * Rating 10 memiliki jumlah sekitar 800.000.

Dari plot ini, terlihat bahwa sebagian besar rating terkonsentrasi pada nilai 1 dan 7 hingga 10, dengan puncak pada rating 8. Hal ini bisa menunjukkan kecenderungan pengguna untuk memberikan rating yang sangat rendah atau sangat tinggi. Dan dikarenakan rating -1 berarti anime tersebut belum diberi rating dan jumlahnya sangat banyak, maka rating -1 dianggap bias dan akan di hapus di tahap Data Preparation.

### Plotting Distribusi Jumlah Rating dari User
![Rating_Count_per_User](https://github.com/user-attachments/assets/42912632-10b1-476b-ba79-665f2b60f8fb)

Gambar 2. Distribusi Jumlah Rating dari User

Berikut penjelasan terkait Scatter Plot di atas:
   * Titik-titik biru menunjukkan pengguna yang memberikan 100 atau lebih rating.
   * Titik-titik merah menunjukkan pengguna yang memberikan kurang dari 100 rating.

Dari plot ini, terlihat bahwa sebagian besar pengguna memberikan kurang dari 100 rating, yang ditunjukkan oleh konsentrasi titik merah di bagian bawah plot. Namun, terdapat beberapa pengguna yang memberikan lebih dari 100 rating, yang ditunjukkan oleh titik-titik biru yang tersebar di seluruh plot.

Dengan kata lain, sebagian besar pengguna aktif memberikan rating dalam jumlah kecil, sementara ada sebagian kecil pengguna yang sangat aktif dengan memberikan rating dalam jumlah besar. User yang memberi rating di bawah 100 akan menimbulkan bias terhadap model jaringan saraf yang akan dibangun. Untuk itu data ini juga akan di hapus pada tahap Data Preparation.

### Plotting Distribusi Jumlah Rating tiap Anime
![Rating_Count_per_Anime](https://github.com/user-attachments/assets/d5152bd3-a44d-48ba-8a6c-7543223a2004)

Gambar 3. Distribusi Jumlah Rating tiap Anime

Berikut penjelasan terkait Scatter Plot di atas:
   * Sumbu x mewakili ID anime,
   * Sumbu y mewakili jumlah rating yang diterima anime tersebut.
   * Titik-titik biru menandakan anime yang menerima 50 atau lebih rating, sedangkan titik-titik merah menandakan anime yang menerima kurang dari 50 rating.

Dari plot ini, terlihat bahwa sebagian besar anime memiliki jumlah rating yang rendah (kurang dari 50), yang ditunjukkan oleh banyaknya titik merah yang tersebar di sepanjang sumbu x dekat sumbu y. Sebaliknya, anime dengan jumlah rating yang lebih tinggi (50 atau lebih) tersebar lebih merata di sepanjang sumbu x, dengan beberapa anime memiliki jumlah rating yang sangat tinggi hingga mendekati 40.000.

Plot ini menarik karena menunjukkan distribusi jumlah rating di antara berbagai anime, yang dapat memberikan wawasan tentang popularitas dan tingkat partisipasi pengguna dalam memberikan rating pada anime tertentu. Anime dengan jumlah rating kurang 50 akan menjadi bias pada data dan akan dihapus pada tahap Data Preparation.

# Data Preparation

## Content Based Filtering
Pada model sistem rekomendasi menggunakan Content Based Filtering akan menggunakan data set Anime. Dimana akan membuat kluster user yang memiliki preferensi serupa. Sebelum membuat model data perlu dipersiapkan. 

### Missing Value
Untuk melihat missing value pada data set anime, dapat menggunakan syntax berikut :

``` anime_df.isnull().sum() ```

Dengan output sebagai berikut :
|   Fitur  | Missing Value |
|----------|---------------|
| anime_id |             0 |
| name     |             0 |
| genre    |            62 |
| type     |            25 |
| episodes |             0 |
| rating   |           230 |
| members  |             0 |

Tabel 3. Missing Value pada Data set Anime

Dari output di atas dapat diketahui fitur genre memiliki 62 missing value, fitur type memiliki 25 value, dan fitur rating memiliki 230 missing value yang merupakan fitur dengan missing value terbanyak. Jika dibandingkan dengan jumlah data secara keseluruhan dapat dikatakan jumlah missing value sengat kecil sehingga solusinya, data dengan missing value akan dihapus. Berikut syntax yang digunakan :

``` anime_df.dropna() ```

### Feature Encoding
Fitur encoding adalah tahap penting dimana mengubah nilai genre yang categorical menjadi numerical. Langkah pertama yang dilakukan adalah mengambil nilai unik dari genre dengan syntax berikut :

``` list_genres = sort(anime_df['genre'].str.split(', ').explode('genre').unique()) ```

Selanjutnya adalah membuat kamus yang menyimpan nilai asli dan nilai yang telah di encode.

```
genre_encode_dict = {genre : i for i, genre in enumerate(list_genres)}
genre_decode_dict = {i : genre for i, genre in enumerate(list_genres)}
```

Selanjutnya membuat fungsi encoding dan decoding genre. Fungsi decoding diperlukan pada tahap simulasi model nantinya.
Fungsi Encoding Genre :
```
def genre_encoding(data):
    return ' '.join([str(genre_encode_dict[genre]) for genre in data.split(', ')])
```

Fungsi Decoding Genre:
```
def genre_decoding(data):
    return ', '.join(genre_decode_dict[int(genre)] for genre in data.split())
```

Terakhir menerapkan fungsi encoding dengan syntax berikut :
``` anime_df['genre'] = anime_df['genre'].apply(genre_encoding) ```

### TF-IDF
Dalam konteks sistem rekomendasi berbasis pengguna (user-based) dan item (item-based), TF-IDF tetap memainkan peran penting untuk mengekstrak fitur penting dari teks yang digunakan dalam rekomendasi. Matrix inilah yang akan digunakan untuk mendapat nilai kesamaannya menggunakan fungsi cosine similarity.

Rumus TFIDF:

$$
TF\text{-}IDF(t,d,D) = TF(t,d) \times IDF(t,D)
$$

Berikut adalah syntax yang digunakan untuk mendapatkan matrix relasi TFIDf:
```
tfidf = TfidfVectorizer(token_pattern=r'\b\w+\b')
tfidf.fit(anime_df['genre'])
tfidf.get_feature_names_out()
```

Berikut 5 sample dari matrix TFIDF:
|                                    | Super Power | Game |  Horror  |  Seinen   |  Shoujo  |   Adventure |   Space  | Josei | Shounen Ai | Historical |
|------------------------------------|-------------|------|----------|-----------|----------|-------------|----------|-------|------------|------------|
| Ninja-tai Gatchaman ZIP!           |    0.0      | 0.0  | 0.000000 |  0.000000 |    0.0   |      0.0    | 0.000000 |  0.0  |     0.0    |    0.0     |
| Penguin&#039;s Memory: Shiawase    |    0.0      | 0.0  | 0.000000 | 0.621717  |    0.0   |      0.0    | 0.000000 |  0.0  |     0.0    |    0.0     |
| Mobile Suit Gundam Unicorn RE:0096 |    0.0      | 0.0  | 0.000000 | 0.000000  |    0.0   |      0.0    | 0.521114 |  0.0  |     0.0    |    0.0     |
| Kyoto Animation: Ajisai-hen        |    0.0      | 0.0  | 0.000000 | 0.000000  |    0.0   |      0.0    | 0.000000 |  0.0  |     0.0    |    0.0     |
| Tokyo Ghoul: &quot;Pinto&quot;     |    0.0      | 0.0  | 0.477662 | 0.000000  |    0.0   |      0.0    | 0.000000 |  0.0  |     0.0    |    0.0     |

Tabel 4. Sample dari Matrix TFIDF

## Collaborative Filtering
Pada model sistem rekomendasi menggunakan Collaborative Filtering akan menggunakan data set Rating. Dimana akan dilatih sebuah model jaringan syaraf tiruan untuk memprediksi rating yang akan diberikan oleh user berdasarkan anime yang telah ditonton dan diberikan rating oleh user tersebut. Sebelum membuat model data perlu dipersiapkan. 

### Missing Value
Dengan menggunakan syntax yang sama pada content based filtering untuk melihat missing value didapat output sebagai berikut :

| Item | Missing Value |
|----------|-----------|
| user_id  |   0 |
| anime_id |   0 |
| rating   |   0 |

Tabel 5. Missing Value pada Data set Rating

Berdasarkan tabel di atas dapat diketahui data set rating tidak menyimpan nilai missing value sama sekali.

### Unrated Anime
Unrated anime di sini adalah anime dengan rating -1 yang berarti anime tersebut telah ditonton oleh user namun belum diberi rating. Berdasarkan plotting distribusi rating dapat dilihat bahwa jumlah anime dengan rating -1 adalah sekitar 1.400.000 data. Tentu ini akan menjadi bias yang sangat besar terhadap model yang akan dibangun. Untuk itu data dengan rating -1 akan dihapus menggunakan syntax berikut :

``` rating_df = rating_df.loc[rating_df['rating'] != -1] ```

Sehingga data rating yang awalnya berjumlah *7.813.737* berkurang menjadi *6.337.241* data.

### Bias Data
Bias data selanjutnya adalah user yang memberi rating kurang dari 100 dan anime dengan jumlah rating kurang 50. Berdasarkan plotting distribusi jumlah rating di dapat user bias berjumlah 19949 dan anime bias berjumlah 3981. Kedua bias ini akan dihapus sehingga jumlah data menjadi sebagai berikut :

| Bias  | Sebelum | Sesudah |
|-------|---------|---------|
| user  |   69600 |   19949 |
| anime |    9890 |    3981 |

### Feature Encoding
Fitur yang akan di-encoding pada tahap ini adalah user_id dan anime_id. Langkah-langkah yang dilakukan adalah sebagai berikut :
1. Membuat kamus yang menyimpan user_id dan anime_id serta nilai encoded
2. Melakukan mapping kepada data set rating dengan membuat dua kolom baru yaitu user_id_encoded dan anime_id_encoded.

### Convert Rating Values to Float
Karena model jaringan saraf tiruan bekerja dengan nilai float nilai rating perlu diubah ke tipe data float untuk memastikan model dapat membuat prediksi dengan baik. Berikut syntax yang digunakan:

``` rating_df.loc[:, 'rating'].values.astype(float32) ```

### Seleksi Fitur
Fitur yang akan digunakan sebagai input pada model jaringan saraf tiruan yang dibangun adalah user_id_encoded dan anime_id_encoded. Tapi sebelumnya data perlu diacak untuk memastikan model tidak membuat prediksi yang bias. Berikut syntax yang digunakan untuk mengacak data : 

``` rating_df.sample(frac=1, random_state=22) ```

Selanjutnya menyimpan fitur ke dalam variabel x dan y.

```
x = rating_df[['user_id_encoded', 'anime_id_encoded']].values
y = rating_df['rating']
```

### Normalisasi
Normalisasi adalah teknik dalam pemrosesan data yang bertujuan untuk mengubah nilai-nilai fitur menjadi skala yang umum atau standar. Normalisasi sering kali dilakukan dengan mengubah nilai fitur sehingga mereka berada dalam rentang tertentu, seperti 0 hingga 1.
Metode normalisasi yang akan digunakan pada proyek ini adalah MinMaxScaler. Berikut syntax yang digunakan:

```
scaler = MinMaxScaler()
scaler.fit(reshape(y, (-1, 1)))
y = scaler.fit_transform(reshape(y, (-1, 1)))
```

### Data Splitting
Data akan dibagi menjadi data latih dan data uji. Perbandingan yang digunakan adalah 80% data sebagai data latih dan sisa 20% sebagai data uji. Berikut shape data latih dan uji:
|  data   |  count  |
|---------|---------|
| x_train | 3698383 |
| y_train | 3698383 |
| x_test  |  924596 |
| y_test  |  924596 |

# Modeling

### Content Based Filtering
Cosine Similarity adalah ukuran kesamaan antara dua vektor dalam ruang vektor, yang sering digunakan dalam content-based filtering. Content-based filtering adalah pendekatan dalam sistem rekomendasi yang memberikan rekomendasi berdasarkan atribut atau fitur dari item itu sendiri, seperti kata kunci atau deskripsi. Keuntungan Cosine Similarity dalam Content-Based Filtering adalah tidak terpengaruh oleh perbedaan ukuran vektor (misalnya, panjang deskripsi yang berbeda). Dengan menggunakan cosine similarity, content-based filtering dapat memberikan rekomendasi yang relevan dan disesuaikan dengan preferensi unik setiap pengguna berdasarkan atribut item yang sudah mereka sukai.

Cosine Similarity antara vektor A dan B dihitung dengan rumus:

**_cosine_similarity_(A, B) = (A • B) / (||A|| * ||B||)**

Keterangan:
   * A • B: Hasil perkalian dot (dot product) antara vektor A dan B.
   * ||A|| dan ||B||: Magnitudo (norm) dari vektor A dan B, secara berturut-turut.
  
Berikut top 10 rekomendasi anime yang serupa dari anime "UFO Princess Valkyrie: Recap" dengan genre Comedy, Romance, Sci-Fi :

|   | name                                               | genre            | similarity_score |
|---|----------------------------------------------------|------------------|------------------|
| 0 | Hoshi Neko Fullhouse | Comedy, Romance, Sci-Fi  | 1.0 |
| 1 | Narue no Sekai | Comedy, Romance, Sci-Fi  | 1.0 |
| 2 | Misute♡naide Daisy   | Comedy, Romance, Sci-Fi  | 1.0 |
| 3 | UFO Princess Valkyrie: Special   | Comedy, Romance, Sci-Fi  | 1.0 |
| 4 | UFO Princess Valkyrie 2: Juunigatsu no Yasoukyoku  | Comedy, Romance, Sci-Fi   | 1.0 |
| 5 | Blue Seed 1.5  | Comedy, Romance, Sci-Fi  | 1.0 |
| 6 | Onegai☆Teacher: Reminiscence Disc   | Comedy, Romance, Sci-Fi  | 1.0 |
| 7 | UFO Princess Valkyrie 4: Toki to Yume to Ginga...  | Comedy, Romance, Sci-Fi   | 1.0 |
| 8 | DNA²  | Comedy, Romance, Sci-Fi  | 1.0 |
| 9 | Soul Link Picture | Drama  Romance, Sci-Fi   | 0.904857 |

### Collaborative Filtering
Model yang digunakan adalah jaringan saraf tiruan sederhana. Model ini sangat cocok untuk memodelkan hubungan antara fitur user dan anime sebagai input dengan rating sebagai output. Model jaringan saraf tiruan akan mempelajari hubungan antara user dengan anime yang telah ditonton dan diberi rating untuk membuat prediksi rating yang akan diberikan kepada anime baru. Kemudian anime-anime baru ini akan disimpan dan diurutkan berdasarkan rating prediksinya untuk dijadikan top-n rekomendasi kepada user.
Berikut adalah struktur model yang digunakan :
1. Embedding Layer: melakukan embedding terhadap nilai fitur user dengan parameter embeddings_initializer = 'he_normal' dan embeddings_regularizer = 1e-6
2. Bias Layer: men-generate bias untuk user yang kemudian ditambahkan setelah operasi dot matrix
3. Embedding Layer: melakukan embedding terhadap nilai fitur user dengan parameter embeddings_initializer = 'he_normal' dan embeddings_regularizer = 1e-6
4. Bias layer: men-generate bias untuk user yang kemudian ditambahkan setelah operasi dot matrix
5. dot matrix: operasi dot matrix terhadap anime dan user yang telah di-embedding)
6. Menjumlahkan hasil operasi dot matrix dengan bias user dan bias anime dan menyimpannya dalam variabel x
7. activation layer: fungsi aktivasi sigmoid diterapkan kepada x untuk mengubah rentang nilai menjadi 0 hingga 1

Callback juga akan diimplementasikan pada proses pelatihan dimana metode callback yang digunakan adalah EarlyStopping dengan parameter sebagai berikut :
1. monitor = 'val_root_mean_squared_error'
2. mode = 'min'
3. patience = 5
4. restore_best_weights = True

Selanjutnya model akan di-compile dengan parameter sebagai berikut:
1. loss = losses.MeanSquaredError(),
2. optimizer = optimizers.Adam(learning_rate=0.0001),
3. metrics = [metrics.RootMeanSquaredError]

Terakhir model akan dilatih sebanyak 100 kali dengan batch_size=128 untuk mengurangi beban komputasi.

Berikut top 10 rekomendasi anime dengan anime yang telah ditonton sebagai berikut :
| | Judul | Genre |
|-|-------|-------|
| 0 | Haikyuu!!   | Comedy, Drama, School, Shounen, Sports |
| 1 | No Game No Life   | Adventure, Comedy, Ecchi, Fantasy, Game, Super... |
| 2 | Digimon Adventure | Action, Adventure, Comedy, Fantasy, Kids |
| 3 | Naruto   Action, | Comedy, Martial Arts, Shounen, Super P... |
| 4 | High School DxD   |Comedy, Demons, Ecchi, Harem, Romance, School |

Top-10 rekomendasi anime :
| | Judul   | Genre |
|-|-------|-------|
| 0 | Noblesse: Awakening  | Action, School, Supernatural, Vampire |
| 1 | Nodame Cantabile OVA 2  | Comedy, Josei, Romance |
| 2 | Acchi Kocchi (TV): Place=Princess   | Comedy, Romance, School, Seinen, Slice of Life |
| 3 | One Piece: Romance Dawn | Action, Comedy, Fantasy, Shounen, Super Power |
| 4 | Uchuu Show e Youkoso | Adventure, Fantasy, Space |
| 5 | Soukyuu no Fafner: Dead Aggressor - Heaven and...  | Action, Drama, Mecha, Military, Sci-Fi |
| 6 | Mangaka-san to Assistant-san to The Animation ...  | Comedy, Ecchi, Harem, Seinen, Slice of Life |
| 7 | Buddy Complex: Kanketsu-hen - Ano Sora ni Kaer...  | Action, Mecha, Sci-Fi
| 8 | Bananya  | Comedy, Kids, Slice of Life |
| 9 | Fumiko no Kokuhaku   |Comedy |

# Evaluation

### Metrik Evaluasi
* Precision
  
  Precision adalah metrik evaluasi yang mengukur seberapa baik model membuat prediksi yang benar untuk kelas positif dari total prediksi positif yang dilakukan. Metrik ini digunakan pada model berbasis konten saja. 
  
  Precision pada proyek ini dihitung dengan rumus berikut:

  **precision = similarity_score / jumlah data rekomendasi**

* MSE
  MSE mengukur rata-rata kuadrat dari kesalahan atau perbedaan antara nilai yang diprediksi oleh model dan nilai aktual. MSE memberikan bobot lebih tinggi pada kesalahan yang lebih besar karena kesalahan dikuadratkan.
  
  Rumus MSE adalah:

  **_MSE_ = $\frac{1}{n} \Sigma_{i=1}^n({y}-\hat{y})^2$**
  Di mana:
  * 𝑛 adalah jumlah pengamatan.
  * 𝑦𝑖 adalah nilai aktual.
  * 𝑦^𝑖 adalah nilai yang diprediksi oleh model.

* RMSE
  RMSE adalah akar kuadrat dari MSE. RMSE memberikan interpretasi yang lebih langsung tentang kesalahan karena berada dalam skala yang sama dengan nilai asli. RMSE sering kali lebih mudah dipahami dan digunakan untuk membandingkan kinerja model.
  Rumus RMSE adalah:
  
  **_RMSE_ = $\sqrt{MSE}$**

### Content Based Filtering
Pada model dengan menggunakan Cosine Similarity menghasilkan nilai presisi 99.05% dari top 10 rekomendasi film.

### Collaborative Filtering
Pada model dengan pendekatan Jaringan Syarat Tiruan menghasilkan nilai kesalahan sebagai berikut :
* MSE : 0.0222 
* Validation MSE : 0.0225
* RMSE : 0.1491
* Validation RMSE : 0.1499

![Model_Evaluasi](https://github.com/user-attachments/assets/853fd41f-8dea-4de6-8888-ade954c0a542)

Gambar 4. MSE dan RMSE Model Collaborative Filtering

### Kesimpulan
#### Problem Statement Answers
1. Mengembangkan sistem rekomendasi menggunakan model content based filtering dan collaborative filtering dengan melakukan tahapan EDA kepada data set untuk menemukan fitur yang cocok sebagai dasar dalam membuat sistem rekomendasi.
2. Berdasarkan EDA yang dilakukan, untuk sistem dengan content based filtering dapat menggunakan TFIDF terhadap fitur genre untuk kemudian dihitung nilai kesamaannya dengan cosine similairty. Sedangkan untuk collaborative filtering menggunakan model JST dengan fitur user dan anime sebagai input kemudian model akan memprediksi rating sebagai output.

#### Goal Achievement
1. Proyek ini menghasilkan rekomendasi yang relevan dengan user berdasarkan kemiripan antara genre dan berdasarkan anime yang telah ditonton.
2. Berhasil menemukan korelasi antara user dan anime yang telah ditonton dengan rating untuk membuat rekomendasi baru untuk user tersebut.

#### Solution 
1. Content-Based Filtering dengan menghitung nilai kemiripan antara genre menggunakan Cosine Similarity dimana pendekatan ini terbukti efektif dalam merekomendasikan film berdasarkan kesamaan genre.
2. Collaborative Filtering dengan model JST (Jaringan Syaraf Tiruan) dengan memanfaatkan jaringan saraf tiruan untuk memprediksi rating film yang belum ditonton.

# References
[1]   A. Sitanggang et al., “Sistem Rekomendasi Anime Menggunakan Metode Singular Value Decomposition (SVD) dan Cosine Similarity,” 2023, [Online]. Available: http://jurnal.utu.ac.id/JTI

[2]   N. M. Roziqiin and M. Faisal, “SISTEM REKOMENDASI PEMILIHAN ANIME MENGGUNAKAN USER-BASED COLLABORATIVE FILTERING,” JIPI (Jurnal Ilmiah Penelitian dan Pembelajaran Informatika), vol. 9, no. 1, pp. 299–306, Feb. 2024, doi: 10.29100/jipi.v9i1.4222.
