# 🤖 Sistem Rekomendasi Berita Cerdas 📰

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ed/Pandas_logo.svg/2560px-Pandas_logo.svg.png" width="90" alt="Pandas Logo">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/NumPy_logo_2020.svg/2560px-NumPy_logo_2020.svg.png" width="90" alt="NumPy Logo">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Scikit_learn_logo_small.svg/1200px-Scikit_learn_logo_small.svg.png" width="110" alt="Scikit-learn Logo">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2d/Tensorflow_logo.svg/1200px-Tensorflow_logo.svg.png" width="50" alt="TensorFlow Logo">
  <img src="https://seaborn.pydata.org/_images/logo-wide-lightbg.svg" width="130" alt="Seaborn Logo">
</p>

Selamat datang di proyek Sistem Rekomendasi Berita! Proyek ini dirancang untuk mengatasi masalah **_information overload_** di dunia digital dengan menyajikan berita yang paling relevan dan personal bagi setiap pengguna. Dengan menganalisis data interaksi dan konten, kami membangun dan membandingkan dua pendekatan _machine learning_ populer untuk menemukan solusi terbaik.

---

## 🎯 Tujuan Proyek

Tujuan utama dari proyek ini adalah:

1.  **Mengembangkan model _Content-Based Filtering_** yang merekomendasikan berita berdasarkan kemiripan konten (judul dan kategori).
2.  **Mengembangkan model _Collaborative Filtering_** yang merekomendasikan berita berdasarkan pola perilaku kolektif pengguna (menggunakan algoritma SVD).
3.  **Mengevaluasi dan membandingkan** performa kedua model secara kuantitatif menggunakan metrik presisi.
4.  **Menentukan pendekatan yang paling efektif** untuk memberikan rekomendasi berita yang dipersonalisasi dan akurat.

---

## 📊 Dataset

Proyek ini menggunakan **Microsoft News Dataset (MIND)**, sebuah dataset skala besar untuk riset rekomendasi berita. Dataset ini berisi data klik anonim dari jutaan pengguna, serta informasi detail mengenai artikel berita yang dibaca.

---

## ⚙️ Metodologi & Pendekatan

Proyek ini mengeksplorasi dua arsitektur model rekomendasi yang fundamental berbeda.

### 1. Model A: Content-Based Filtering

Pendekatan ini bekerja seperti pustakawan pribadi yang merekomendasikan buku berdasarkan buku yang pernah Anda sukai.

- **Cara Kerja**: Model ini menganalisis teks dari **judul** dan **kategori** berita. Menggunakan teknik **TF-IDF Vectorizer**, setiap berita diubah menjadi representasi vektor numerik. Rekomendasi diberikan dengan mencari berita lain yang memiliki vektor paling mirip (menggunakan **Cosine Similarity**).
- **Karakteristik**: Cenderung merekomendasikan item yang sangat mirip dengan apa yang sudah disukai pengguna, aman namun kurang mengejutkan.

### 2. Model B: Collaborative Filtering

Pendekatan ini bekerja seperti teman yang merekomendasikan film dengan berkata, "Orang-orang yang seleranya mirip denganmu juga suka film ini!".

- **Cara Kerja**: Model ini sama sekali tidak melihat konten berita. Sebaliknya, ia belajar dari **matriks interaksi pengguna-berita**. Dengan menggunakan algoritma **Singular Value Decomposition (SVD)**, model ini menemukan pola atau "selera" laten dari perilaku kolektif semua pengguna.
- **Karakteristik**: Mampu menghasilkan rekomendasi yang beragam dan tak terduga (_serendipity_) karena menangkap selera implisit pengguna.

---

## 🚀 Hasil & Evaluasi

Kedua model dievaluasi menggunakan metrik **Precision@10**, yang mengukur seberapa banyak item yang relevan dari 10 rekomendasi teratas yang diberikan. Metrik ini sangat cocok untuk sistem rekomendasi karena mencerminkan pengalaman pengguna di "halaman pertama".

Hasilnya menunjukkan perbedaan performa yang sangat signifikan:

| Model                             | Precision@10  | Keterangan                                          |
| :-------------------------------- | :-----------: | :-------------------------------------------------- |
| Content-Based Filtering           |    0.0032     | Performa sangat rendah, rekomendasi kurang relevan. |
| **Collaborative Filtering (SVD)** | **0.5312** 🏆 | **Performa JAUH LEBIH UNGGUL** dan akurat.          |

<br>

> **Insight Kunci**: Hasil evaluasi membuktikan bahwa memahami **pola perilaku kolektif pengguna (Collaborative Filtering)** jauh lebih efektif daripada sekadar mencocokkan **konten (Content-Based Filtering)**. Model SVD mampu menangkap preferensi pengguna yang kompleks dan implisit, menghasilkan rekomendasi yang jauh lebih relevan.

---

## 💡 Kesimpulan

Berdasarkan analisis dan evaluasi yang mendalam, **Collaborative Filtering menggunakan algoritma SVD** adalah pendekatan yang jelas **lebih unggul** untuk membangun sistem rekomendasi berita pada dataset ini. Model ini tidak hanya memberikan akurasi yang lebih tinggi secara dramatis, tetapi juga memiliki potensi untuk memberikan rekomendasi yang lebih beragam dan personal.

---

## 📁 Struktur Repositori

```
├── assets
├── dataset
├── laporan_proyek_rekomendasi_berita.md
├── proyek_rekomendasi_berita.ipynb
└── README.md
```

- **`laporan_proyek_rekomendasi_berita.md`**: Laporan lengkap proyek dalam format markdown.
- **`proyek_rekomendasi_berita.ipynb`**: Kode Jupyter Notebook yang berisi semua tahapan proyek, mulai dari pra-pemrosesan data, EDA, pemodelan, hingga evaluasi.
- **`README.md`**: Dokumentasi yang sedang Anda baca ini.

---

## 💻 Cara Menjalankan Proyek

Untuk menjalankan notebook dan mereplikasi hasil proyek ini, ikuti langkah-langkah berikut:

1.  **Clone Repository**

    ```bash
    git clone [https://github.com/NAMA_USER/NAMA_REPO.git](https://github.com/NAMA_USER/NAMA_REPO.git)
    cd NAMA_REPO
    ```

2.  **Buat Lingkungan Virtual (Opsional tapi Direkomendasikan)**

    ```bash
    python -m venv venv
    source venv/bin/activate  # Untuk Windows: venv\Scripts\activate
    ```

3.  **Instal Dependensi**
    Instal semua _library_ yang dibutuhkan menggunakan pip.

    ```bash
    pip install pandas numpy matplotlib seaborn scikit-learn tensorflow wordcloud
    ```

4.  **Jalankan Jupyter Notebook**
    Buka dan jalankan file `proyek_rekomendasi_berita.ipynb` untuk melihat seluruh proses analisis dan pemodelan.
    ```bash
    jupyter notebook proyek_rekomendasi_berita.ipynb
    ```
