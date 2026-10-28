# Analisis Sentimen Kebijakan Pajak Kendaraan Listrik

Proyek skripsi ini membandingkan **Support Vector Machine (SVM)** dan **IndoBERT** untuk mengklasifikasikan sentimen komentar YouTube tentang perubahan kebijakan pajak kendaraan listrik di Indonesia tahun 2026.

Analisis menggunakan tiga kelas sentimen: negatif, netral, dan positif. Dataset berisi 4.645 komentar berlabel. Notebook mencakup pengumpulan dan pembersihan komentar, preprocessing, pelatihan model, serta evaluasi pada beberapa pembagian data.

## Hasil eksperimen

Ringkasan berikut diambil dari keluaran eksperimen yang tersimpan pada notebook. Rekap CSV-nya tersedia di [`outputs/reports/rekap_evaluasi_tercatat.csv`](outputs/reports/rekap_evaluasi_tercatat.csv).

| Model | Split | Akurasi | F1-macro | ROC-AUC |
| --- | --- | ---: | ---: | ---: |
| SVM | 80/20 | 0,7387 | 0,6350 | 0,8513 |
| SVM | 75/25 | 0,7364 | 0,6420 | 0,8528 |
| SVM | 70/30 | 0,7408 | 0,6389 | 0,8522 |
| IndoBERT | 80/20 | 0,7592 | 0,6837 | 0,8185 |
| IndoBERT | 75/25 | 0,7796 | 0,6808 | 0,8295 |
| IndoBERT | 70/30 | 0,7739 | 0,6852 | 0,8186 |

Pada eksperimen yang tercatat, IndoBERT memperoleh F1-macro tertinggi pada split 70/30, sedangkan SVM memperoleh ROC-AUC lebih tinggi. Hasil ini berlaku untuk dataset dan konfigurasi eksperimen tersebut.

## Isi repositori

```text
.
├── data/
│   ├── processed/
│   │   └── hasil_komentar_bersih_berlabel.csv
│   └── raw/                         # Data mentah scraping, tidak disertakan
├── notebooks/
│   ├── Scrapping_Youtube_Listrik_clean.ipynb
│   └── Skripsi_SVM_IndoBERT.ipynb
└── outputs/
    ├── models/                      # Checkpoint dibuat saat notebook dijalankan
    └── reports/
        └── rekap_evaluasi_tercatat.csv
```

## Dataset

Dataset analisis ada di `data/processed/hasil_komentar_bersih_berlabel.csv`. Kolom yang digunakan adalah `comment` dan `label`; nilai labelnya `Negatif`, `Netral`, dan `Positif`. File ini merupakan data komentar yang sudah dibersihkan dan diberi label untuk penelitian.

Notebook scraper tidak melakukan pelabelan sentimen. Jika ingin mengulang pengumpulan data, jalankan scraper, lakukan pelabelan, lalu siapkan CSV dengan kolom dan nilai label yang sama.

## Menjalankan notebook

Gunakan Python 3.10 atau lebih baru dan Jupyter Notebook/Lab. Buka proyek sebagai folder kerja utama, lalu jalankan notebook dari atas ke bawah.

1. `notebooks/Scrapping_Youtube_Listrik_clean.ipynb` mengumpulkan komentar YouTube dan menyimpan data mentah di `data/raw/` serta data bersih di `data/processed/`.
2. Setelah data diberi label, `notebooks/Skripsi_SVM_IndoBERT.ipynb` membaca CSV berlabel dari `data/processed/hasil_komentar_bersih_berlabel.csv`.
3. Laporan eksperimen baru disimpan di `outputs/reports/` dan checkpoint IndoBERT di `outputs/models/`.

Notebook analisis memakai pandas, NumPy, scikit-learn, PyTorch, Transformers, Datasets, Accelerate, Sastrawi, NLTK, emoji, IndoNLP, Matplotlib, Seaborn, dan WordCloud. Notebook scraper memakai pandas, Requests, dan `youtube-comment-downloader`. Beberapa paket dipasang dari dalam notebook.

Pelatihan IndoBERT mengunduh model `indobenchmark/indobert-base-p1` dan membutuhkan ruang penyimpanan serta waktu komputasi yang cukup. Eksperimen yang direkam di notebook berjalan pada GPU Tesla T4; perangkat dan versi pustaka dapat memengaruhi waktu maupun hasil saat dijalankan ulang.

## Struktur analisis

- **SVM:** preprocessing teks bahasa Indonesia, TF-IDF, pencarian parameter `C`, lalu klasifikasi LinearSVC.
- **IndoBERT:** preprocessing ringan, tokenizer IndoBERT, dan fine-tuning untuk klasifikasi tiga kelas.
- **Evaluasi:** akurasi, precision, recall, F1-macro, ROC-AUC, confusion matrix, dan analisis kesalahan.

Oversampling dilakukan pada data latih. Notebook menetapkan seed 42, tetapi hasil tetap dapat berubah karena perbedaan perangkat keras, pustaka, atau data.
