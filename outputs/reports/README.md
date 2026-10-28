# Catatan hasil eksperimen

`rekap_evaluasi_tercatat.csv` disusun dari tabel keluaran yang sudah tersimpan di notebook `notebooks/Skripsi_SVM_IndoBERT.ipynb`. Nilainya adalah rekaman eksperimen sebelumnya, bukan hasil eksekusi pada komputer ini.

| Model | Split terbaik berdasarkan F1 | F1-macro | Akurasi |
|---|---:|---:|---:|
| SVM | 75/25 | 0.6420 | 0.7364 |
| IndoBERT | 70/30 | 0.6852 | 0.7739 |

Waktu pelatihan IndoBERT yang tercatat (sekitar 707–919 detik per split) berasal dari lingkungan dengan GPU Tesla T4. Hasil dapat berubah jika eksperimen dijalankan ulang.
