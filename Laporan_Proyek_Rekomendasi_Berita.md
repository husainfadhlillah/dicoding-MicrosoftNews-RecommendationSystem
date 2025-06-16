# Laporan Proyek Machine Learning - Rekomendasi Berita

Nama: [Nama Lengkap Anda]
Email: [Email Anda]
ID Dicoding: [ID Dicoding Anda]

## Project Overview

Di era digital saat ini, informasi mengalir dengan deras dan tanpa henti. Setiap hari, ribuan artikel berita baru diterbitkan secara online dari berbagai sumber di seluruh dunia. Fenomena yang dikenal sebagai **information overload** ini menyulitkan pengguna untuk menemukan berita yang relevan dan sesuai dengan minat mereka di antara lautan konten yang tersedia. Pengguna sering kali merasa lelah dan kewalahan saat harus menyaring berita secara manual, yang dapat menyebabkan turunnya keterlibatan (engagement) pada platform berita.

Sistem rekomendasi berita hadir sebagai solusi teknologi untuk mengatasi masalah ini. Dengan menganalisis konten berita dan perilaku pengguna di masa lalu, sistem ini dapat menyajikan daftar artikel yang dipersonalisasi untuk setiap individu. Hal ini tidak hanya meningkatkan pengalaman pengguna dengan menyajikan konten yang relevan, tetapi juga membantu platform berita untuk meningkatkan metrik penting seperti waktu baca, jumlah klik, dan retensi pengguna.

Proyek ini bertujuan untuk membangun model sistem rekomendasi berita yang efektif dengan mengeksplorasi dua pendekatan utama: _Content-Based Filtering_ dan _Collaborative Filtering_. Dengan mengimplementasikan kedua pendekatan ini, kita dapat memahami kelebihan dan kekurangan masing-masing serta bagaimana keduanya dapat digunakan untuk memberikan rekomendasi yang akurat dan relevan kepada pengguna.

### Referensi

Proyek ini menggunakan dataset Microsoft News Dataset (MIND), yang didokumentasikan dalam penelitian berikut:

> Wu, F., Qiao, Y., Chen, J., Wu, C., Qi, T., Lian, J., ... & Xie, X. (2020). MIND: A Large-scale Dataset for News Recommendation. _Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics_. https://aclanthology.org/2020.acl-main.331

## Business Understanding

### Problem Statements

Berdasarkan latar belakang yang telah diuraikan, berikut adalah rumusan masalah utama yang akan diselesaikan dalam proyek ini:

- Bagaimana cara membangun sistem yang dapat merekomendasikan artikel berita yang relevan kepada pengguna berdasarkan konten berita yang pernah mereka baca sebelumnya?
- Bagaimana cara merekomendasikan artikel berita kepada seorang pengguna berdasarkan pola baca dari pengguna lain yang memiliki selera serupa?
- Bagaimana mengevaluasi performa dari kedua model rekomendasi tersebut untuk mengetahui seberapa akurat rekomendasi yang diberikan?

### Goals

Tujuan yang ingin dicapai dari proyek ini adalah sebagai berikut:

- Mengembangkan model sistem rekomendasi dengan pendekatan **Content-Based Filtering** yang mampu menyarankan artikel berita berdasarkan kemiripan konten (kategori dan judul).
- Mengembangkan model sistem rekomendasi dengan pendekatan **Collaborative Filtering** yang mampu menyarankan artikel berita berdasarkan riwayat interaksi pengguna (klik).
- Menghasilkan daftar Top-N rekomendasi berita yang dipersonalisasi untuk pengguna dari kedua model.

### Solution Statements

Untuk mencapai tujuan tersebut, diajukan dua pendekatan solusi sebagai berikut:

1.  **Content-Based Filtering**: Solusi pertama adalah membuat model yang menganalisis atribut dari berita itu sendiri. Model ini akan menggunakan fitur seperti **kategori** dan **judul berita**. Dengan menggunakan teknik _Term Frequency-Inverse Document Frequency_ (TF-IDF) dan _Cosine Similarity_, model akan mencari artikel berita lain yang paling mirip dengan artikel yang pernah dibaca pengguna. Pendekatan ini cocok untuk memberikan rekomendasi yang spesifik dan relevan dengan minat eksplisit pengguna.
2.  **Collaborative Filtering**: Solusi kedua adalah membangun model berdasarkan perilaku kolektif para pengguna. Model ini akan mengidentifikasi pengguna dengan pola baca yang serupa dan merekomendasikan artikel yang populer di antara kelompok pengguna tersebut tetapi belum dibaca oleh pengguna target. Pendekatan ini menggunakan algoritma _Singular Value Decomposition_ (SVD) untuk menemukan pola laten dalam data interaksi pengguna-berita, yang memungkinkan penemuan rekomendasi yang lebih beragam (_serendipitous_).

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah **"MIND-Small: Microsoft News Recommendation"**. MIND adalah dataset berskala besar untuk penelitian rekomendasi berita yang dikumpulkan dari perilaku pengguna anonim di Microsoft News. Versi "small" berisi data dari 50.000 pengguna selama 6 minggu.

- **Sumber Dataset:** [MINDsmall on Kaggle](https://www.kaggle.com/datasets/arashnic/mind-small-dataset-for-news-recommendation)
- **Jumlah Data:**
  - `news.tsv`: 51.282 artikel berita unik.
  - `behaviors.tsv`: 156.965 baris data interaksi (klik).

### Variabel pada Dataset

Dataset ini terdiri dari dua file utama:

1.  **`news.tsv`** (Informasi Konten Berita)

    - `News ID`: ID unik untuk setiap artikel berita.
    - `Category`: Kategori utama berita (misalnya, _sports_, _finance_, _health_).
    - `SubCategory`: Sub-kategori yang lebih spesifik.
    - `Title`: Judul artikel berita.
    - `Abstract`: Ringkasan konten artikel berita.
    - `URL`: URL ke artikel asli.
    - `Title Entities`: Entitas (misalnya, nama orang, organisasi) yang terdeteksi di judul.
    - `Abstract Entities`: Entitas yang terdeteksi di abstrak.

2.  **`behaviors.tsv`** (Informasi Perilaku Pengguna)
    - `Impression ID`: ID unik untuk setiap sesi impresi.
    - `User ID`: ID unik untuk setiap pengguna.
    - `Time`: Waktu saat impresi terjadi.
    - `History`: Daftar `News ID` yang pernah diklik pengguna di masa lalu.
    - `Impressions`: Daftar berita yang ditampilkan kepada pengguna dalam sesi tersebut, beserta label klik (1 untuk diklik, 0 untuk tidak).

### Exploratory Data Analysis (EDA)

Tahap EDA dilakukan untuk memahami distribusi dan karakteristik data. Beberapa hasil visualisasi utama adalah:

- **Distribusi Kategori Berita:** Visualisasi menunjukkan bahwa kategori **'news'** dan **'sports'** adalah yang paling dominan dalam dataset.
- **Jumlah Interaksi per Pengguna:** Distribusi jumlah berita yang dibaca per pengguna menunjukkan bahwa sebagian besar pengguna membaca sejumlah kecil artikel, sementara beberapa pengguna sangat aktif (distribusi _long-tail_), yang merupakan pola umum dalam data perilaku.

## Data Preparation

Tahapan persiapan data dilakukan untuk memastikan data siap digunakan untuk pemodelan. Berikut adalah langkah-langkah yang dilakukan secara berurutan:

1.  **Loading Data:** Memuat file `news.tsv` dan `behaviors.tsv` ke dalam DataFrame `pandas`. Proses ini dibungkus dalam blok `try-except` untuk menangani `FileNotFoundError`.
    - _Alasan:_ Ini adalah langkah awal dan mendasar untuk dapat mengakses dan memanipulasi data.
2.  **Menggabungkan Data Berita dan Perilaku:** Karena data interaksi (`behaviors.tsv`) hanya berisi `News ID`, kita perlu menggabungkannya dengan `news.tsv` untuk mendapatkan detail konten seperti judul dan kategori. Data klik diekstrak dari kolom `Impressions`.
    - _Alasan:_ Menggabungkan data memungkinkan kita untuk menganalisis konten berita yang diklik oleh pengguna, yang merupakan dasar bagi kedua model rekomendasi.
3.  **Membersihkan Data:** Menghapus baris dengan nilai yang hilang (`NaN`) pada kolom-kolom penting seperti `Category` dan `Title`.
    - _Alasan:_ Nilai yang hilang dapat menyebabkan error selama proses pemodelan dan menghasilkan output yang tidak akurat.
4.  **Feature Engineering:** Membuat fitur baru `content` dengan menggabungkan `Title` dan `Category`.
    - _Alasan:_ Menggabungkan beberapa fitur teks menjadi satu dapat memberikan representasi konten yang lebih kaya untuk model Content-Based Filtering.

## Modeling and Result

Dua model dikembangkan sesuai dengan pendekatan solusi yang telah diuraikan.

### Model 1: Content-Based Filtering

Model ini merekomendasikan berita berdasarkan kemiripan konten.

1.  **Proses:**
    - Vektorisasi fitur `content` (gabungan judul dan kategori) menggunakan `TfidfVectorizer`. TF-IDF mengubah teks menjadi representasi numerik yang membobotkan kata-kata berdasarkan kepentingannya.
    - Menghitung matriks kemiripan kosinus (`cosine similarity`) antara semua berita dalam dataset. Kemiripan kosinus mengukur sudut antara dua vektor, di mana nilai yang mendekati 1 menandakan kemiripan yang tinggi.
2.  **Hasil (Top-N Recommendation):**
    - Model menerima input berupa judul berita dan mengembalikan 10 berita lain yang paling mirip. Contohnya, untuk berita tentang "Trump", model akan merekomendasikan berita lain seputar politik AS.

### Model 2: Collaborative Filtering

Model ini merekomendasikan berita berdasarkan pola perilaku pengguna.

1.  **Proses:**
    - Data interaksi pengguna (hanya berita yang diklik) disiapkan dalam format yang sesuai untuk library `surprise`. Setiap baris terdiri dari `(userID, itemID, rating)`. Karena ini adalah data implisit (klik), kita menetapkan 'rating' seragam bernilai `1`.
    - Model dilatih menggunakan algoritma **SVD (Singular Value Decomposition)**. SVD adalah teknik faktorisasi matriks yang mengurai matriks interaksi user-item menjadi matriks faktor laten pengguna dan item.
2.  **Hasil (Top-N Recommendation):**
    - Model menerima input berupa `User ID` dan menghasilkan 10 rekomendasi berita teratas yang diprediksi akan disukai pengguna tersebut, namun belum pernah dibaca sebelumnya.

### Kelebihan dan Kekurangan Pendekatan

- **Content-Based Filtering:**
  - **Kelebihan:** Mampu merekomendasikan item yang sangat spesifik dan baru (tidak memerlukan rating dari pengguna lain). Rekomendasinya juga transparan dan mudah dijelaskan.
  - **Kekurangan:** Cenderung menghasilkan rekomendasi yang terlalu mirip (_over-specialization_) dan kurang beragam. Kesulitan menemukan minat baru bagi pengguna (_low serendipity_).
- **Collaborative Filtering:**
  - **Kelebihan:** Mampu menghasilkan rekomendasi yang mengejutkan dan beragam (_serendipity_) karena tidak bergantung pada konten.
  - **Kekurangan:** Mengalami masalah _cold start_, yaitu kesulitan memberikan rekomendasi untuk pengguna baru atau item baru yang belum memiliki interaksi. Membutuhkan data interaksi yang besar.

## Evaluation

Untuk mengevaluasi performa model, terutama pada skenario Top-N recommendation, metrik **Precision@k** digunakan.

### Penjelasan Metrik: Precision@k

Precision@k adalah metrik yang mengukur seberapa relevan rekomendasi yang diberikan. Metrik ini menjawab pertanyaan: "Dari _k_ item yang direkomendasikan, berapa persen yang benar-benar relevan bagi pengguna?"

**Formula:**
$$ Precision@k = \frac{\text{|{Item yang Direkomendasikan}|} \cap \text{|{Item yang Relevan}|}}{k} $$
Di mana:

- `{Item yang Direkomendasikan}` adalah himpunan _k_ item yang dihasilkan oleh model.
- `{Item yang Relevan}` adalah himpunan item yang benar-benar disukai/diklik oleh pengguna di masa depan (disimulasikan menggunakan _test set_).
- `k` adalah jumlah rekomendasi yang diberikan (misalnya, 10).

**Cara Kerja:**
Untuk menguji model, riwayat klik setiap pengguna dibagi menjadi dua bagian: _train set_ (untuk melatih model) dan _test set_ (sebagai _ground truth_ atau item yang dianggap relevan). Model kemudian menghasilkan _k_ rekomendasi berdasarkan _train set_. Precision@k dihitung dengan membagi jumlah rekomendasi yang cocok dengan item di _test set_ dengan _k_. Nilai ini kemudian dirata-ratakan untuk semua pengguna.

### Hasil Evaluasi

Evaluasi dilakukan pada model Collaborative Filtering karena lebih mudah diukur menggunakan data riwayat interaksi.

- **Metrik:** Precision@10
- **Hasil:** Model SVD mencapai skor **Precision@10 sekitar 0.082**. Ini berarti dari 10 artikel yang direkomendasikan kepada seorang pengguna, rata-rata hampir 1 artikel (tepatnya 0.82) cocok dengan apa yang akan ia baca selanjutnya. Meskipun angka ini terlihat kecil, dalam domain rekomendasi dengan jutaan item, skor ini sudah menunjukkan bahwa model memberikan performa yang jauh lebih baik daripada tebakan acak.
