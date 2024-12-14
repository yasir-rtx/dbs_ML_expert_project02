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

#### Dataset Anime

#### Dataset Rating

# Data Preparation

## Eksplorasi Data Analysis (EDA)

### Ringkasan Deskripsi Statistik

### Missing Value

### Unrated Anime


# Modeling

# Evaluation
