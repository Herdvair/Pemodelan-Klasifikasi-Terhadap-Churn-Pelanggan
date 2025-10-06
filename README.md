# Customer Churn Prediction: Menganalisis dan Memprediksi Pelanggan Berpotensi Berhenti Berlangganan

# 💡Latar Belakang Proyek 
Proyek ini berfokus pada analisis dan pengembangan model Machine Learning untuk memprediksi Customer Churn (pelanggan yang berhenti berlangganan) pada sebuah perusahaan layanan telekomunikasi. Mengidentifikasi dan memprediksi churn adalah suatu strategi pada retensi yang efektif, sehingga memungkinkan perusahaan mengambil tindakan secara proaktif untuk mempertahankan pelanggan. Saya menggunakan dataset historis yang mencakup berbagai fitur, mulai dari informasi pelanggan, layanan yang digunakan, hingga detail tagihan. Proses dimulai dari pembersihan data, eksplorasi data, penanganan imbalance data menggunakan SMOTE, hingga perbandingan performa beberapa model klasifikasi.

# 🛠️ Tahapan Proyek
1. Data Cleaning & Preprocessing
  - Pengecekan Missing Value: Ditemukan 11 missing value pada kolom TotalCharges.
  - Penanganan Missing Value: Karena distribusi TotalCharges cenderung right-skewed, missing value diimputasi menggunakan median untuk menjaga kualitas distribusi data.
  - Encoding Data Tidak Konsisten: Nilai-nilai yang menunjukkan tidak adanya layanan (No phone service, No internet service) di kolom layanan tertentu diubah menjadi 'No' untuk konsistensi data.

2. Exploratory Data Analysis (EDA)
  - Distribusi Target (Churn): Data terdeteksi imbalance, dengan proporsi pelanggan Non-Churn (~73%) jauh lebih besar dibanding Churn (~26%).
  - Analisis Korelasi Numerik (Heatmap):
      - Terdapat korelasi kuat antara tenure dan MonthlyCharges terhadap TotalCharges.
      - Status SeniorCitizen menunjukkan korelasi yang sangat lemah dengan variabel biaya/lamanya berlangganan, mengindikasikan fitur ini mungkin kurang relevan dalam konteks prediksi churn.
  - Analisis Korelasi Kategorikal (Chi-Square & Cramer's V):
      - Variabel Contract (Cramer's V: 0.4065) dan InternetService (Cramer's V: 0.3160) terbukti memiliki hubungan medium yang signifikan dengan churn.
      - Variabel lain seperti PaymentMethod, PaperlessBilling, TechSupport, dan OnlineSecurity memiliki hubungan signifikan namun dengan effect yang lemah.
      - gender dan PhoneService tidak menunjukkan hubungan yang signifikan.

3. Feature Engineering & Handling Imbalance
  - Encoding:
      - Kolom (InternetService, Contract, PaymentMethod) di-encode menggunakan One Hot Encoding (OHE).
      - Kolom biner lainnya (gender, Partner, Dependents, dll.) di-encode menggunakan Label Encoder.
    - Oversampling: Karena masalah imbalance data (perbandingan 73:27), dilakukan oversampling pada data training menggunakan teknik SMOTE (Synthetic Minority Oversampling Technique) untuk menyeimbangkan jumlah kelas (menjadi 50:50).

# 🤖 Pemodelan & Hasil

Saya menggunaka dua model klasifikasi, Decision Tree (DT) dan Random Forest (RF), pemodelan ini menggunakan Hyperparameter Tuning (GridSearchCV/RandomizedSearchCV) untuk mendapatkan performa terbaik.

| Metrik Evaluasi | Decision Tree (Train) | Random Forest (Train) | Decision Tree (Test) | Random Forest (Test) |
| :--- | :---: | :---: | :---: | :---: |
| Accuracy | 0.83 | 0.89 | 0.78 | 0.80 |
| F1-Score | 0.83 | 0.90 | 0.63 | 0.64 |
| AUC-ROC | 0.91 | 0.96 | 0.82 | 0.85 |
| Recall (Churn/Class 1) | 0.84 | 0.90 | 0.70 | 0.67 |
| Precision (Churn/Class 1) | 0.83 | 0.89 | 0.57 | 0.61 |

**Model Pilihan: Random Forest**
-> Random Forest terpilih sebagai model terbaik dengan performa yang lebih stabil dan seimbang pada data test:
  - Akurasi Keseluruhan (80%) lebih tinggi.
  - AUC-ROC (0.86) lebih tinggi, menunjukkan kemampuan diskriminasi antar kelas yang lebih baik.
    Precision (61%) lebih tinggi pada kelas Churn, yang berarti ketika model memprediksi pelanggan akan churn, kemungkinannya 61% benar. Ini krusial untuk meminimalkan False Positives (pelanggan sehat yang salah diprediksi churn), sehingga menghemat biaya kampanye retensi yang tidak perlu.

# 🎯 Potensi Dampak Bisnis
Penerapan model Random Forest memungkinkan perusahaan:
1. Identifikasi Dini Pelanggan Berisiko Tinggi: Perusahaan dapat secara proaktif mengidentifikasi pelanggan yang kemungkinan besar akan churn dengan tingkat kepastian yang seimbang antara precision dan recall.
2. Efisiensi Biaya Retensi: Dengan precision 61% pada kelas churn, perusahaan dapat memfokuskan sumber daya dan anggaran kampanye retensi (seperti penawaran khusus atau layanan pelanggan personal) hanya pada pelanggan dengan risiko churn yang benar-benar nyata, dibandingkan dengan model yang menghasilkan banyak false alarm.
3. Optimalisasi Strategi: Hasil analisis EDA menguatkan bahwa jenis kontrak dan layanan internet adalah faktor pendorong churn yang signifikan, memberikan wawasan langsung bagi tim marketing dan produk untuk perbaikan layanan.
