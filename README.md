# Laporan Proyek 2 Machine Learning Muhammad Yasir

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

Berdasarkan jurnal penelitian 2024 yang ditulis Nazhif Muafa Roziqiin dan M. Faisal dengan judul [Sistem Rekomendasi Pemilihan Anime Menggunakan User-Based Collaborative Filtering](http://www.jurnal.stkippgritulungagung.ac.id/index.php/jipi/article/view/4222) menjelaskan metode User-Based Collaborative Filtering berdasarkan preferensi pengguna dapat digunakan untuk membangun sistem rekomendasi. Penelitian lain oleh Altolyto Sitanggang dkk dengan judul [Sistem Rekomendasi Anime Menggunakan Metode Singular Value Decomposition (SVD) dan Cosine Similarity](http://jurnal.utu.ac.id/JTI/article/viewFile/7787/4290?form=MG0AV3) menjelaskan cosine similarity dapat digunakan untuk mengukur kemiripan antar anime juga dapat dijadikan dasar sistem rekomendasi.

# Business Understanding

## Proble Statements
1. Bagaimana mengembangkan sistem rekomendasi yang akurat dan mampu membuat sistem rekomendasi yang personal berdasarkan preferensi pengguna serta mampu membuat rekomendasi berdasarkan kemiripan anter anime?
2. Bagaimana memamfaatkan fitur-fitur yang terdapat pada dataset untuk membuat rekomendasi yang relevan bagi pengguna?

## Goals
1. Mengembangkan sistem rekomendasi anime yang akurat dan personal untuk setiap pengguna dan juga mampu memberikan rekomendasi berdasarkan kemiripan antar anime.
2. Memanfaatkan data interaksi pengguna dan fitur-fitur anime untuk memberikan rekomendasi yang relevan.

## Solution Statements
1. Mengembangkan sistem rekomendasi menggunakan metode content based filtering dengan memamfaatkan fitur genre untuk membuat sistem rekomendasi.
2. Mengembangkan sistem rekomendasi menggunakan metode collaborative filtering dengan memamfaatkan korelasi antara anime dengan rating yang diberikan oleh user.

# Data Understanding
Dataset yang digunakan dalam proyek ini adalah "Anime Recommendations Database" yang tersedia di Kaggle. Dataset ini berisi informasi tentang berbagai judul anime serta interaksi pengguna dengan anime tersebut. Dataset ini didapat dari website [myanimelist](https://myanimelist.net/).

Informasi lebih lanjut dapat dilihat di [Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database).

### Struktur Dataset: Dataset ini terdiri dari dua file utama:
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

## Eksplorasi Data Analysis (EDA) pada Data Anime

### Ringkasan Deskripsi Statistik
| Item      | Rating       |
|-----------|--------------|
| count     | 12064.000000 |
| mean      | 6.473902     |
| std       | 1.026746     |
| min       | 1.670000     |
| 25%       | 5.880000     |
| 50%       | 6.570000     |
| 75%       | 7.180000     |
| max       | 10.000000    |

Tabel 1. Deskripsi Statistik Rating pada Anime

Berdasarkan tabel di atas dapat diketahui bahwa rating terendah yang diberi oleh user adalah 1.67 dan rating tertinggi 10. Rata-rata rating adalah 6.47.

### Missing Value
Missing value pada data anime didapat dengan menggunakan fungsi .isnull() dari pandas.
```
anime_df.isnull().sum()
```
Fungsi tersebut menghasilkan output sebagai berikut:
| Fitur     | count |
|-----------|-------|
| anime_id  |   0   |
| name      |   0   |
| genre     |  62   |
| type      |  25   |
| episodes  |   0   |
| rating    | 230   |
| members   |   0   |

Tabel 2 Missing Value pada Data Anime

Jumlah missing value pada data anime sangat sedikit bila dibandingkan dengan jumlah total data yang ada. Metode yang digunakan untuk mengatasi missing value pada tabel anime adalah dengan menghapus baris yang yang memiliki missing value.

###  Plotting Distribusi Jumlah Rating dari Setiap Anime

![jumlah_rating_setiap_anime](https://github.com/user-attachments/assets/408f441a-fa77-4b0e-b29e-aaacba3544dc)

Gambar 1. Distribusi Jumlah Rating pada Anime

Bayangkan sebuah anime baru mendapat rating sebanyak satu, tentu penilaian terhadap anime itu akan sangat dipengaruhi oleh satu rating tersebut. Jika rating yang diberikan tinggi tentu anime tersebut akan memiliki citra yang bagus dan sebaliknya. Anime dengan kondisi seperti ini dapat menjadi bias saat membangun model AI. Oleh karena itu diterapkan nilai ambang yang akan menyaring anime dengan jumlah rating yang rendah. Nilai ambang yang digunakan pada proyek ini adalah 50 yang akan menghapus anime dengan jumlah rating lebih kecil dari 50.

## Eksplorasi Data Analysis (EDA) pada Data Rating

### Ringkasan Deskripsi Statistik
| Item      | Rating        |
|-----------|---------------|
| count     | 7.813737e+06  |
| mean      | 6.144030e+00  |
| std       | 3.727800e+00  |
| min       | -1.000000e+00 |
| 25%       | 6.000000e+00  |
| 50%       | 7.000000e+00  |
| 75%       | 9.000000e+00  |
| max       | 1.000000e+01  |

Tabel 3. Deskripsi Statistik rating pada Rating

Berdasarkan tabel di atas dapat diketahui bahwa rating terendah yang diberi oleh user adalah -1 dimana rating -1 berarti bahwa user tidak memberi rating pada anime dan rating tertinggi 10. Rata-rata rating adalah 6.14.

### Missing Value
Fungsi yang sama untuk melihat missing value pada data anime juga akan diguanakn pada data rating dan didapat bahwa data rating tidak memiliki missing value pada setiap fiturnya. Output dari fungsu isnull() dapat dilihat pada tabel berikut:

| Fitur     | count |
|-----------|-------|
| user_id   | 0     |
| anime_id  | 0     |
| rating    | 0     |

Tabel 4. Missing Value pada Data Rating


### Plotting Distribusi Rating

![distribusi_rating](https://github.com/user-attachments/assets/9571acf8-e205-47f1-9c00-99da8a952091)

Gambar 2. Distribusi Rating

Berdasarkan keterangan dataset, nilai rating -1 menandakan anime tersebut sudah ditonton oleh user namun belum mendapatkan rating. Karena data ini tidak dibutuhkan makan akan dihapus. Nilai rating 0 kosong karena tidak ada opsi untuk memberi rating 0 pada suatu anime. Dan rating tertinggi terpadat pada rating dengan nilai 8 dan nilai terkecil terdapat pada 1.

Berikut adalah kode yang digunakan untuk menghapus rating dengan nilai -1:
```
rating_df = rating_df.loc[rating_df['rating'] != -1]
```

### Plotting Distribusi Jumlah Rating dari User

![distribusi_jumlah_rating](https://github.com/user-attachments/assets/660d3026-bf94-4cbd-9513-d1a24a6dd73b)

Gambar 3. Jumlah Rating yang diberikan User

Pada plot di atas terlihat bahwa setiap user telah memberi rating yang berbeda-beda. Bahkan ada user yang telah memberi 3500 rating. Dan juga dapat dilihat terdapat user yang baru memberi satu rating. User-user yang belum memberi banyak rating menimbulkan bias pada data. Untuk itu diperlukan sebuah nilai batas ambang untuk menyaring user-user dengan jumlah rating yang sedikit. Pada proyek ini nilai ambang batas yang diterapkan adalah 100. Dimana user yang belum memberi rating lebih dari 100 akan dihapus dari data. Berikut kode yang digunakan:

```
user_count_rating = rating_df['user_id'].value_counts()
valid_users = user_count_rating[user_count_rating >= 100]
data_rating = data_rating[data_rating['user_id'].isin(valid_users)]
```

Pertama dihitung berapa banyak rating yang telah diberi oleh setiap user. Kemudian dibuat sebuah variabel yang menampung user yang telah memberi rating lebih dari 100 kemudian menggunakan fungsi isin() menghapus user yang tidak memenuhi ambang batas.

# Data Preparation

## Fitur dan Label Encoding
Fitur encoding adalah proses penting dalam machine learning terutama ketika bekerja dengan data kategorikal. Algoritma machine learning membutuhkan data dalam format numerik agar dapat bekerja dengan baik. 

### Fitur user_id
Setelah melalui berbagai tahap preprocessing ada user_id yang terhapus dalam prosesnya. Sehingga meski user_id sudah disimpan dalam bentuk numerik dibutuhkan encoding agar user_id memilki nilai yang terurut. Kolom user_id akan di-loop untuk kemudian didapat indexnya. Index ini lah yang akan menjadi nilai baru dari user_id.

### Fitur anime_id
Setelah melalui berbagai tahap preprocessing ada anime_id yang terhapus dalam prosesnya. Sehingga meski anime_id sudah disimpan dalam bentuk numerik dibutuhkan encoding agar anime_id memilki nilai yang terurut. Kolom anime_id akan di-loop untuk kemudian didapat indexnya. Index ini lah yang akan menjadi nilai baru dari anime_id.

### Fitur Genre
Nilai pada fitur genre bukan disimpan dalam bentuk numerik sehingga fitur genre harus di-encode agar dapat diolah dengan baik oleh model machine learning. Langkah pertama yang dilakukan adalah mendapat nilai unik dari genre kemudian disimpan dalam sebuah array. Kemudian sama seperti fitur user_id dan anime_id, array akan di-looping untuk mendapatkan indexnya dan index ini lah yang menjadi nilai baru dari genre.

## Seleksi Fitur
Sebelum seleksi fitur dilakukan data akan diacak terlebih dahulu menggunakan syntax berikut:
```
data_rating.sample(frac=1, random_state=22)
```

Fitur user dan anime akan menjadi variabel input pada model collaborative filtering. Dan Rating akan menjadi variabel target.

## Normalisasi
Normalisasi dalam machine learning adalah proses mengubah skala fitur-fitur data sehingga berada dalam rentang tertentu, biasanya antara 0 dan 1, atau mengubah distribusi nilai menjadi memiliki rata-rata 0 dan standar deviasi 1. Tujuan utama normalisasi adalah memastikan setiap fitur berkontribusi setara dalam model pembelajaran mesin, terutama untuk algoritma yang sensitif terhadap skala data.

Metode normalisasi yang digunakan adalah min-max normalisasi dimana metode mengubah nilai asli suatu fitur berdasarkan nilai minimum dan maksimum dari fitur tersebut.

## Splitting Data
Dataset akan dibagi menjadi data latih dan data uji. Data latih digunakan untuk melatih model, sedangkan data uji digunakan untuk menguji kinerja model pada data yang belum pernah dilihat sebelumnya. Dalam proyek ini, data latih akan memiliki ukuran 80% dari dataset dan untuk data latih sebesar 20% dari dataset. Sehingga jumlah data latih adalah 61316 dan jumlah data uji sebanyak 15329.

# Modeling

## 4.1. Content Based Filtering
Metode TF-IDF digunakan untuk menemukan representasi fitur penting dari setiap genre anime. Fungsi tfidfvectorizer() dari scikit-learn akan digunakan untuk menemukan representasi tersebut.

Kemudian metode cosine similarity akan digunakan untuk mengukur kemiripan antar anime berdasarkan representasi genre dari TF-IDF. Cosine similarity adalah metrik yang digunakan untuk mengukur seberapa mirip dua vektor dalam ruang dimensi tinggi, berdasarkan sudut kosinus antara mereka. Cosine similarity tidak memperhitungkan magnitudo (panjang) vektor, tetapi lebih fokus pada arah vektor.

Rumus Cosine Similarity
Rumus untuk menghitung cosine similarity antara dua vektor A dan B adalah:

cosine similarity = 𝐴⋅𝐵 / ∣∣𝐴∣∣ × ∣∣𝐵∣∣

Di mana:
* 𝐴⋅𝐵 adalah dot product dari vektor A dan B.
* ∣∣𝐴∣∣ adalah magnitudo (panjang) dari vektor A.
* ∣∣𝐵∣∣ adalah magnitudo (panjang) dari vektor B.

Berikut rekomendasi top-N dari anime "Death Note" :
| name	                                   | genre                                            |              
|-------------------------------------------|--------------------------------------------------|
|	Death Note Rewrite	                    | Mystery Police Psychological Supernatural Thri...
|	Mousou Dairinin	                       | Drama Mystery Police Psychological Supernatura...
|	Higurashi no Naku Koro ni Kai	           | Mystery Psychological Supernatural Thriller
|	Higurashi no Naku Koro ni Rei	           | Comedy Mystery Psychological Supernatural Thri...
|	Mirai Nikki (TV)                         | Action Mystery Psychological Shounen Supernatu...
|	Mirai Nikki (TV): Ura Mirai Nikki	     | Action Comedy Mystery Psychological Shounen Su...
|	Higurashi no Naku Koro ni	              | Horror Mystery Psychological Supernatural Thri...
|	Monster	                                | Drama Horror Mystery Police Psychological Sein...
|	AD Police	                             | Adventure Dementia Mecha Mystery Police Psycho...
|	Higurashi no Naku Koro ni Kaku: Outbreak | Horror Mystery Psychological Thriller 


## 4.2. Collaborative Filtering
Model menghitung skor kecocokan antara pengguna dan anime dengan teknik embedding. Berikut langkah-langkah yang dilakukan:
1.  dilakukan proses embedding terhadap data user dan resto dengan parameter embeddings_initializer adalah 'he_normal' dan embeddings_regularizer yang digunakan adalah l2 dengan nilai 1e-6 dan ukuran embedding sebesar 50.
2.  lakukan operasi perkalian dot product antara embedding user dan resto.
3.  tambahkan bias untuk setiap user dan resto. 
4.  Skor kecocokan ditetapkan dalam skala [0,1] dengan fungsi aktivasi sigmoid.

Setelah model didefinisikan dengan mengikuti langkat di atas kemudia model di-compile dengan parameter sebagai berikut:
1. Loss function adalah fungsi yang digunakan untuk mengukur seberapa baik model Anda dalam membuat prediksi. Loss function menghitung perbedaan antara prediksi model dan nilai sebenarnya dari data pelatihan. Tujuan pelatihan model adalah untuk meminimalkan nilai loss ini. Fungsi loss yang digunakan pada proyek ini adalah Binary Cross-Entropy.
2. Optimizer adalah algoritma yang digunakan untuk memperbarui bobot model selama proses pelatihan untuk meminimalkan loss. Optimizer menggunakan gradient descent atau variasi lainnya untuk melakukan pembaruan ini. Optimizer yang digunakan adalah Adam.
3. Metrics adalah fungsi yang digunakan untuk mengevaluasi kinerja model. Metrik ini tidak mempengaruhi proses pelatihan tetapi memberikan informasi tentang seberapa baik model Anda berjalan pada data pelatihan dan validasi. Metrics yang digunakan adalah Mean Absolute Error (MAE).

Terakhir proses training dilakukan dengan parameter sebagai berikut:
1. x: Parameter ini menunjukkan data fitur yang digunakan untuk pelatihan model. Ini adalah input yang digunakan oleh model untuk belajar.
2. y: Parameter ini menunjukkan data target yang digunakan untuk pelatihan model. Ini adalah output yang diharapkan dari model, yang akan digunakan untuk membandingkan dan menghitung error selama pelatihan.
3. batch_size: 8 Parameter ini menentukan jumlah sampel yang akan diproses sebelum model diperbarui. Dalam contoh ini, setiap 8 sampel, model akan memperbarui bobotnya berdasarkan perhitungan error.
4. epochs: 100 Parameter ini menunjukkan jumlah iterasi penuh melalui dataset pelatihan. Dalam contoh ini, model akan melewati seluruh dataset pelatihan sebanyak 100 kali.
5. validation_data: (x_valid, y_valid) Parameter ini menunjukkan data validasi yang digunakan untuk mengevaluasi model selama pelatihan. Data ini tidak digunakan untuk pelatihan tetapi untuk memantau performa model dan mendeteksi overfitting.
6. callbacks: EarlyStopping(monitor='val_root_mean_squared_error', mode='min', patience=5, restore_best_weights=True) Parameter ini menggunakan callback EarlyStopping untuk menghentikan pelatihan lebih awal jika metrik yang dipantau (val_root_mean_squared_error) tidak membaik setelah sejumlah epoch tertentu (patience=5). mode='min' menunjukkan bahwa metrik yang dipantau harus diminimalkan. restore_best_weights=True menunjukkan bahwa bobot terbaik akan dipulihkan setelah pelatihan dihentikan.

Berikut anime yang pernah di tonton dan diberi rating oleh user dengan id 666:

| Judul | Genre |
|-------|-------|
| No Game No Life     | Adventure, Comedy, Ecchi, Fantasy, Game, Supernatural |
| High School DxD New | Action, Comedy, Demons, Ecchi, Harem, Romance, School |
| Naruto              | Action, Comedy, Martial Arts, Shounen, Super Power |
| High School DxD     | Comedy, Demons, Ecchi, Harem, Romance, School |
| Black Bullet        | Action, Mystery, Sci-Fi, Seinen |

Berikut rekomendasi top-N dari user dengan id 666 :
| Judul | Genre |
|-------|-------|
| Mushishi | Adventure, Fantasy, Historical, Mystery, Seinen, Slice of Life, Supernatural
| Kuroko no Basket 2nd Season | Comedy, School, Shounen, Sports
| Akatsuki no Yona | Action, Adventure, Comedy, Fantasy, Romance, Shoujo
| Nisekoi | Comedy, Harem, Romance, School, Shounen
| Isshuukan Friends. | Comedy, School, Shounen, Slice of Life
| Amagami SS | Comedy, Romance, School, Slice of Life
| Boku wa Tomodachi ga Sukunai | Comedy, Ecchi, Harem, Romance, School, Seinen, Slice of Life
| Maoyuu Maou Yuusha | Adventure, Demons, Fantasy, Historical, Romance
| Naruto Movie 1: Dai Katsugeki!!| Adventure, Comedy, Drama, Historical, Shounen, Supernatural
| Hidan no Aria | Action, Comedy, Romance, School

# Evaluation
Metrics yang digunakan untuk mengukur performa model collaborative filtering adalah Mean Absolute Error (MAE). Mean Absolute Error (MAE) mengukur rata-rata absolut dari selisih antara nilai yang diprediksi oleh model dan nilai aktual. Ini memberikan gambaran langsung tentang seberapa jauh prediksi model dari nilai yang sebenarnya.

Rumus MAE :

![Screenshot 2024-12-20 234934](https://github.com/user-attachments/assets/1e714702-5c1e-4ef6-805c-f0e06d7ab962)


Di mana:
* 𝑛 adalah jumlah total data.
* 𝑦𝑖 adalah nilai aktual.
* 𝑦^𝑖 adalah nilai yang diprediksi oleh model.
* ∣𝑦𝑖−𝑦^𝑖∣ adalah nilai absolut dari selisih antara nilai aktual dan nilai prediksi.

Kelebihan MAE:
1. Interpretasi yang Sederhana: MAE memberikan nilai rata-rata dari kesalahan prediksi dalam satuan yang sama dengan target.
2. Tidak Sensitif terhadap Outlier: MAE tidak terlalu dipengaruhi oleh nilai ekstrim (outlier) dibandingkan dengan Mean Squared Error (MSE).

Kekurangan MAE:
1. Tidak Menunjukkan Variabilitas: MAE tidak menunjukkan variabilitas dari kesalahan prediksi karena mengabaikan tanda kesalahan.
2. Tidak Berhubungan dengan Variansi: MAE tidak mempertimbangkan kuadrat dari kesalahan, sehingga tidak memberikan gambaran tentang seberapa jauh kesalahan prediksi menyebar.

![MAE](https://github.com/user-attachments/assets/055dfd68-7f91-4c01-95d6-5de48b88c74f)

Gambar 4. Plot MAE

Berdasarkan diagram di atas dapat dilihat kurva biru (train) menunjukkan penurunan RMSE yang tajam pada awal epoch dan kemudian stabil setelah sekitar 10 epoch. Kurva oranye (test) juga menunjukkan penurunan RMSE yang tajam pada awal epoch, tetapi mulai stabil dan sedikit meningkat setelah sekitar 10 epoch.

Grafik ini memberikan gambaran tentang bagaimana performa model meningkat selama pelatihan dan bagaimana model tersebut menggeneralisasi pada data pengujian. Penurunan RMSE pada data pelatihan dan pengujian menunjukkan bahwa model belajar dengan baik
