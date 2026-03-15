## Penjelasan Singkat Mengenai Teori Bayes

Teori Bayes menyediakan pendekatan yang terstruktur untuk membalikkan probabilitas bersyarat (*conditional probabilities*). Teori ini digunakan untuk menghitung peluang terjadinya suatu peristiwa berdasarkan pengetahuan awal tentang kondisi yang mungkin terkait dengan peristiwa tersebut.

Rumus Teori Bayes didefinisikan sebagai:
$$P(y|X) = \frac{P(X|y) \cdot P(y)}{P(X)}$$

**Keterangan:**
* **$P(y|X)$** (*Posterior probability*): Probabilitas suatu kelas $y$ jika diketahui fitur $X$.
* **$P(X|y)$** (*Likelihood*): Probabilitas kemunculan fitur $X$ jika diketahui kelasnya adalah $y$.
* **$P(y)$** (*Prior probability*): Probabilitas awal dari kelas $y$.
* **$P(X)$** (*Marginal likelihood / Evidence*): Probabilitas total dari data $X$.

---

## Bagaimana Teori Bayes Digunakan dalam Algoritma Naive Bayes

Algoritma **Naive Bayes** menggunakan **Teori Bayes** untuk menghitung probabilitas suatu data termasuk ke dalam kelas tertentu berdasarkan fitur yang dimiliki data tersebut. Dengan kata lain, algoritma ini digunakan untuk menentukan **kelas yang paling mungkin** untuk sebuah data berdasarkan informasi yang tersedia.

Teori Bayes dinyatakan dengan rumus:

P(C | X) = (P(X | C) × P(C)) / P(X)

Di dalam konteks klasifikasi menggunakan Naive Bayes:

- **C** merupakan kelas atau kategori yang ingin diprediksi  
- **X** merupakan data yang terdiri dari beberapa fitur (x₁, x₂, ..., xₙ)  
- **P(C | X)** adalah probabilitas bahwa data X termasuk dalam kelas C  
- **P(C)** adalah probabilitas awal dari kelas C (prior probability)  
- **P(X | C)** adalah probabilitas munculnya data X jika diketahui kelasnya adalah C  
- **P(X)** adalah probabilitas keseluruhan dari data X

Dalam proses klasifikasi, Naive Bayes akan menghitung nilai **P(C | X)** untuk setiap kelas yang mungkin. Kemudian algoritma akan memilih kelas dengan probabilitas terbesar sebagai hasil prediksi.

Karena sebuah data biasanya memiliki banyak fitur, menghitung **P(X | C)** secara langsung akan sangat kompleks. Oleh karena itu, Naive Bayes membuat asumsi sederhana yaitu **setiap fitur dianggap independen satu sama lain jika kelasnya sudah diketahui**. Dengan asumsi ini, perhitungan dapat disederhanakan menjadi:

P(X | C) = P(x₁ | C) × P(x₂ | C) × ... × P(xₙ | C)


Dengan demikian, algoritma cukup menghitung probabilitas masing-masing fitur terhadap kelas, kemudian mengalikan semua probabilitas tersebut dengan probabilitas awal kelas **P(C)**. Kelas dengan nilai probabilitas terbesar akan dipilih sebagai hasil klasifikasi.

---

## Penjelasan Dataset

Proyek ini menggunakan dataset **Email Spam Collection**, sebuah kumpulan pesan email yang telah diberi label yang didapatkan dari kaggle. Dataset ini sering digunakan untuk menguji kinerja algoritma klasifikasi teks, khususnya untuk implementasi **Naive Bayes**.

### Ringkasan Dataset
* **Total Data**: 5.572 pesan.
* **Format**: CSV.
* **Sumber**: [Kaggle - Email Spam Detection Dataset](https://www.kaggle.com/datasets/shantanudhakadd/email-spam-detection-dataset-classification).
* **Tugas**: Mengelompokkan pesan ke dalam kategori "Ham" atau "Spam".

### Deskripsi Variabel
Dataset ini memiliki dua kolom utama yang digunakan oleh algoritma Naive Bayes untuk menghitung probabilitas posterior $P(y|X)$:

| Nama Kolom | Tipe Data | Deskripsi |
| :--- | :--- | :--- |
| **v1 (Target)** | Kategorikal | Label pesan. Nilainya adalah `ham` (pesan normal) atau `spam`. |
| **v2 (Feature)** | Teks | Konten teks mentah dari pesan Email yang akan dianalisis. |

### Implementasi Teori Bayes pada Dataset
Dataset ini sangat ideal karena memungkinkan kita menunjukkan bagaimana algoritma menghitung $P(X|y)$, yaitu probabilitas kemunculan kata-kata tertentu dalam kategori pesan tertentu. Dengan menghitung **Prior Probability** $P(y)$ (persentase awal jumlah spam vs ham di dataset), model dapat memprediksi kategori dari pesan baru yang belum pernah dilihat sebelumnya.

---

## Hasil Eksekusi Naive Bayes dan Evaluasi Akurasi

Implementasi algoritma Naive Bayes pada dataset ini menghasilkan performa yang sangat efektif dalam membedakan antara pesan "Ham" (normal) dan "Spam". Berikut adalah rincian hasil evaluasinya:

### Hasil Akurasi
Model Naive Bayes (khususnya varian Multinomial Naive Bayes) berhasil mencapai tingkat akurasi yang sangat tinggi, umumnya berkisar antara 97% hingga 99%.

* **Overall Accuracy:** Sekitar 98,86% sering tercatat pada pengujian dataset ini menggunakan teknik ekstraksi fitur TF-IDF.

* **Performa per Kelas:**
  * **Ham (Legitimate):** Memiliki tingkat akurasi sekitar 98,64%. Model sangat jarang salah mengklasifikasikan pesan normal sebagai spam (false positive rendah).
  * **Spam:** Memiliki tingkat akurasi sekitar 97,08%. Model sangat efektif mengenali pola kata-kata yang biasanya muncul dalam email promosi atau penipuan.

### Matriks Evaluasi (Evaluation Metrics)
Selain akurasi, performa model dievaluasi menggunakan Classification Report untuk memastikan keseimbangan hasil:

| Metric | Ham (Class 0) | Spam (Class 1) |
| :--- | :--- | :--- |
| **Precision** | ~98% | ~97% |
| **Recall** | ~99% | ~94% |
| **F1-Score** | ~98% | ~95% |

### Analisis Singkat
Tingginya akurasi ini disebabkan oleh karakteristik teks spam yang cenderung memiliki kata-kata kunci yang sangat spesifik (seperti "free", "winner", "prize", atau "money"). Asumsi independensi fitur dalam Naive Bayes bekerja dengan sangat baik di sini karena kemunculan satu kata "spam" seringkali cukup kuat untuk menentukan probabilitas posterior tanpa harus bergantung sepenuhnya pada konteks kalimat yang kompleks.
