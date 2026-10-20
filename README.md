# UTS Machine Learning – Linear Regression & Logistic Regression

**Nama:** Muhammad Rizky Angkasa Putra Anwar
**NIM:** 241011400213
**Kelas:** Reg B
**random_state:** 13 (2 digit terakhir NIM)

Repositori ini berisi notebook `tugas_UTS.ipynb` (sudah dijalankan lengkap dengan output), dataset `Ads_Data.csv` dan `Email_Data.csv`. Semua model dan metrik diimplementasikan sendiri dengan NumPy, scikit-learn hanya dipakai di soal bonus.

## Bagian A – Linear Regression (Ads_Data.csv, 40 data)

Split 80:20 menghasilkan 32 data training dan 8 data testing. Model normal equation menghasilkan persamaan **ŷ = 27,65 + 9,02X**, artinya setiap kenaikan biaya iklan 1 juta Rp, penjualan naik sekitar 9,02 juta Rp.

| Metrik | Training | Testing |
|---|---|---|
| MAE | 4,47 | 5,60 |
| MSE | 36,32 | 40,70 |
| RMSE | 6,03 | 6,38 |
| R² | 0,942 | 0,866 |

Modelnya cukup baik karena R² training dan testing sama-sama tinggi dan selisihnya kecil, jadi tidak ada tanda overfitting yang parah.

**Gradient descent (5000 iterasi):**

| Learning rate | Intercept | Koefisien | Hasil |
|---|---|---|---|
| 0,001 | 25,35 | 9,39 | Terlalu lambat, belum konvergen |
| 0,02 | 27,65 | 9,02 | Pas, hampir sama dengan normal equation |
| 0,1 | nan | nan | Divergen (muncul RuntimeWarning overflow) |

**Prediksi penjualan:** biaya iklan 4,5 juta menghasilkan 68,22 juta, 7 juta menghasilkan 90,76 juta, dan 12 juta menghasilkan 135,84 juta. Prediksi untuk 12 juta adalah ekstrapolasi (di luar rentang data 0,8 sampai 9,9 juta), jadi kurang bisa dipercaya.

## Bagian B – Logistic Regression (Email_Data.csv, 60 data)

Data terdiri dari 35 email Bukan Spam dan 25 Spam. Split 75:25 menghasilkan 45 data training dan 15 data testing. Model dilatih dengan learning rate 0,1 dan 10000 iterasi, lossnya turun dari 0,693 ke sekitar 0,260 dan sudah stabil.

Persamaan: **P(Spam) = σ(-10,43 + 0,908·JumlahLink + 0,993·KataPromo)**

Decision boundary: **x2 = 10,51 - 0,914·x1**

**Evaluasi pada data testing (threshold 0,5):**

| Metrik | Nilai |
|---|---|
| Accuracy | 0,80 |
| Precision | 0,571 |
| Recall | 1,00 |
| F1-score | 0,727 |
| Confusion matrix | TN = 8, FP = 3, FN = 0, TP = 4 |

Ada 3 email yang salah diklasifikasikan, semuanya false positive (email biasa terdeteksi spam), yaitu (JumlahLink, KataPromo) = (6, 6), (3, 9), dan (3, 10).

**Eksperimen threshold:**

| Threshold | TN | FP | FN | TP | Precision | Recall |
|---|---|---|---|---|---|---|
| 0,3 | 6 | 5 | 0 | 4 | 0,444 | 1,00 |
| 0,5 | 8 | 3 | 0 | 4 | 0,571 | 1,00 |
| 0,7 | 8 | 3 | 0 | 4 | 0,571 | 1,00 |

Threshold yang dipilih adalah **0,7**, karena false positive (email penting masuk spam) dinilai lebih merugikan pengguna daripada false negative.

**Prediksi email baru (threshold 0,7):**

| Email | JumlahLink | KataPromo | P(Spam) | Prediksi |
|---|---|---|---|---|
| E1 | 1 | 2 | 0,0005 | Bukan Spam |
| E2 | 5 | 6 | 0,515 | Bukan Spam |
| E3 | 9 | 10 | 0,9995 | Spam |
| E4 | 2 | 11 | 0,909 | Spam |

## Bagian C – Pemahaman Konsep

Jawaban C1 sampai C5 (sigmoid, normal equation vs gradient descent, overfitting, MSE vs log loss, dan alasan hasil angka berbeda antar mahasiswa) ada di notebook.

## Bonus – Perbandingan dengan scikit-learn

Pada Linear Regression, hasil sklearn sama persis dengan implementasi sendiri (b = 27,65, m = 9,02, R² testing 0,866). Pada Logistic Regression, koefisien berbeda karena sklearn secara default memakai regularisasi L2 dengan C = 1, sehingga koefisiennya lebih kecil. Tanpa penalti, koefisien sklearn (b = -11,27, m1 = 0,984, m2 = 1,068) masih sedikit lebih besar dari implementasi sendiri karena gradient descent berhenti di 10000 iterasi. Akurasi testing ketiganya sama, yaitu 0,80.

## Isi Repositori

- `tugas_UTS.ipynb` – notebook jawaban lengkap dengan output
- `Ads_Data.csv` dan `Email_Data.csv` – dataset
- `README.md` – ringkasan hasil
