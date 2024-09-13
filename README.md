# Rekomendasi Destinasi Wisata Indonesia

## Latar Belakang
Pariwisata adalah sektor penting dalam perekonomian Indonesia. Dengan keberagaman budaya, alam, dan atraksi, Indonesia menawarkan berbagai destinasi menarik. Namun, banyak wisatawan yang kesulitan menemukan destinasi yang sesuai dengan preferensi mereka. Oleh karena itu, sistem rekomendasi yang personal dapat membantu mempermudah wisatawan dalam memilih destinasi yang sesuai dengan minat mereka.

## Business Understanding
Tujuan utama proyek ini adalah untuk membantu wisatawan dalam memilih destinasi wisata yang sesuai dengan preferensi mereka. Dengan menggunakan sistem rekomendasi berbasis konten (content-based) dan filter kolaboratif (collaborative filtering), pengguna akan mendapatkan rekomendasi destinasi yang dipersonalisasi berdasarkan data perilaku mereka atau kesamaan dengan pengguna lain.

## Problem Statement
- Bagaimana cara membangun dan merekomendasikan destinasi wisata berdasarkan kemiripan destinasi wisata pengguna menggunakan content based filtering?
- Bagaimana cara membangun dan merekomendasikan destinasi wisata berdasarkan kesamaan preferensi dengan pengguna lainnya menggunakan collaborative filtering?
- Apa jenis rekomendasi terbaik untuk merekomendasikan wisata kepada pengguna?

## Goals
- Mengembangkan sistem rekomendasi destinasi wisata berbasis content based filtering dan collaborative filtering.
- Meningkatkan kepuasan pengguna dengan memberikan rekomendasi yang relevan berdasarkan preferensi dan riwayat mereka.
- Menyediakan pengalaman penjelajahan wisata yang lebih personal.

## Solution Statements
Proyek ini akan mengembangkan dua jenis model rekomendasi:
1. **Content-Based Filtering**: Rekomendasi didasarkan pada karakteristik destinasi wisata yang mirip dengan destinasi yang pernah diminati pengguna sebelumnya.
2. **Collaborative Filtering**: Rekomendasi diberikan berdasarkan kesamaan preferensi antar pengguna.

Kedua pendekatan ini akan diintegrasikan untuk memberikan rekomendasi yang lebih akurat dan sesuai.

## Data Understanding
Dataset yang digunakan dalam pengembangan proyek ini adalah tourism_with_id.csv dan tourism_rating.csv

tourism_with_id.csv
1. Place_Id : Id destinasi wisata
2. Place_Name : Nama destinasi wisata
3. Description : Deskripsi destinasi wisata
4. Category : Kategori destinasi wisata
5. City : Kota destinasi wisata 
6. Price : Harga destinasi wisata
7. Rating : Penilaian Secara Umum destinasi wisata
8. Time_Minutes : Waktu dalam menit
9. Coordinate: Koordinat destinasi wisata
10. Lat : Ketinggain destinasi wisata
11. Long : Posisi bujur dalam destinasi wisata
12. Unnamed: 11 : Tidak diketahui
13. Unnamed: 12 : Tidak diketahui

Ukuran : 437 baris dengan 13 kolom
    
Link Dataset : https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination?select=tourism_with_id.csv

tourism_rating.csv
1. User_Id : Id user yang me-rating
2. Place_Id : Id destinasi wisata
3. Place_Ratings : Rating yang diberikan user

Ukuran : 10000 baris dengan 3 kolom atribut
Link Dataset : https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination?select=tourism_rating.csv

Kondisi kedua dataset :
- Belum terintegrasi dan masih terpisah
- Terdapat data yang duplikat

Sehingga dataset akhir yang digunakan menjadi:
1. id : Id destinasi wisata
2. name : Nama destinasi wisata
3. description	: Deskripsi destinasi wisata
4. city : Kota destinasi wisata
5. category: Kategori destinasi wisata
6. city_category : Kombinasi kota dan kategori
7. price : Harga masuk destinasi wisata

dimana atribut city_category adalah atribut yang dilihat kemiripannya untuk contet destinasi wisata.

## Data Preparation
Langkah-langkah persiapan data:
- Merge Dataset : Mengintegrasikan dan menggabungkan kedua dataset sehingga menjadi satu dataset

- Missing Values: Penanganan data yang hilang dengan dengan menghilangkan data.

Berdasarkan data yang diperoleh tidak terdapat missing value sehingga tidak perlu diatasi

- Duplicate Values : Penanganan data yang duplikat dengan menghilangkan data.

Berdasarkan data yang diperoleh terdapat duplicate value sehingga perlu diatasi

- Mengurutkan urutan data dalam dataset berdasarkan id secara  ASCENDING

Pengurutan data dalam dataset ini akan mempermudah dalam membangun model content based filtering dan collaborative filtering

- Melakukan Encoding untuk User_Id dan Place_Id

Proses encoding baik untuk User_Id dan Place_Id dilakukan untuk mengubah data mentah menjadi representasi numerik dan setiap representasinya disimpan dalam suatu dictionary

- Melakukan feature engineering dengan menggabungkan category dan city untuk variabel baru berupa city_category

Proses feature engineering ini dilakukan karena atribut category dan city adalah atribut yang bisa merepresentasikan item destinasi wisata sehingga kita memerlukan atribut tersebut untuk mengukur cosine similiarity-nya.

- Tetapkan jumlah user, jumlah destinasi wisata, minimum rating, dan maksimal rating untuk kebutuhan split training

Penetapan jumlah user, destinasi wisata, minimum rating, dan maksimal rating ini dilakukan untuk kebutuhan class recommenderNet sehingga penulis dapat membangun model recommendation untuk collaborative filtering 

- Melakukan split untuk training dan validation sebesar 80:20

Proses split data training dan validation ini bertujuan untuk membagi data untuk proses training (melatih data yang sudah ada) dan validation model (melatih data baru) sehingga menghasilkan akurasi yang minim bias dan tidak overfitting

## Model Development
1. **Content-Based Filtering**:
   Model menggunakan metode content based filtering dengan TfidfVectorizer untuk menghitung cosine similiarity

### TfidVectorizer

TfidfVectorizer dalam content-based filtering adalah teknik yang digunakan untuk mengubah teks (misalnya deskripsi objek) menjadi representasi numerik berdasarkan frekuensi kata. TF-IDF (Term Frequency-Inverse Document Frequency) menghitung seberapa penting suatu kata dalam sebuah dokumen relatif terhadap semua dokumen.

- Term Frequency (TF): Mengukur seberapa sering kata muncul dalam dokumen.
- Inverse Document Frequency (IDF): Mengurangi bobot kata yang sering muncul di banyak dokumen (misalnya, kata umum seperti "dan", "adalah").
- 
Dalam content-based filtering, TfidfVectorizer digunakan untuk menghitung vektor fitur dari deskripsi item (misalnya, destinasi wisata), sehingga sistem dapat merekomendasikan item dengan deskripsi serupa kepada pengguna berdasarkan preferensi item yang pernah mereka sukai.

### Cosine Similiarity

Cosine similarity dalam content-based filtering adalah metode untuk mengukur kesamaan antara dua vektor (misalnya, item yang direpresentasikan sebagai vektor fitur seperti TF-IDF). Cosine similarity menghitung sudut kosinus antara dua vektor dalam ruang dimensi, di mana nilai 1 berarti sangat mirip (arah vektor sama) dan 0 berarti tidak ada kesamaan (vektor tegak lurus).

Dalam content-based filtering, cosine similarity digunakan untuk membandingkan kesamaan antara item (misalnya, city_category dua destinasi wisata) dan merekomendasikan item yang paling mirip dengan preferensi pengguna.

### Hasil Rekomendasi Content Based Filtering

![Screenshot 2024-09-11 153430](https://github.com/user-attachments/assets/7f17532b-c805-486b-a2ae-1bb31cfcbd8a)

2. **Collaborative Filtering**:
   Model menggunakan metode collaborative filtering dengan deep learning tensorflow dalam bentuk class RecommenderNET

### Class RecommenderNET

Class RecommenderNet adalah model neural network untuk collaborative filtering. Model ini memetakan user dan destinasi wisata ke dalam vektor embedding, yang digunakan untuk menghitung kesamaan (dot product) antara user dan destinasi.

- Layer Embedding: Mengubah user dan destinasi wisata menjadi vektor berukuran lebih kecil.
- Bias: Menambahkan bias spesifik untuk user dan destinasi.
- Dropout: Mengurangi overfitting.
- dot_user_tourism: Hasil dot product ditambah bias, kemudian diaktifkan dengan fungsi sigmoid untuk memberikan skor rekomendasi.

### Hasil Rekomendasi Collaborative Filtering

![Screenshot 2024-09-11 153445](https://github.com/user-attachments/assets/61ffc28c-d83d-4dee-b015-e6c3d81f8963)

## Evaluasi Model
Evaluasi model dilakukan dengan metrik berikut:
- **Root Mean Squared Error (MSE)** Mengukur akar rata-rata dari erorr untuk collaborative filtering.
- **Precision**: Metrik evaluasi yang mengukur seberapa akurat prediksi yang relevan dibandingkan keseluruhan prediksi

Model akan diuji pada data validasi untuk melihat performa dan melakukan tuning pada parameter model untuk hasil terbaik.

### Hasil Evaluasi Content Based Filtering

Precision=i/r

r= total rekomendasi yang relevan
i= jumlah rekomendasi benar yang diberikan

Dari hasil rekomendasi content based filtering diatas dapat dilihat bahwa Taman Legenda Keong Emas masuk dalam city_category berupa Jakarta Taman Hiburan dan hasil rekomendasi menunjuk semua destinasi wisata yang memiliki city_category yang sama. Hal ini menunjukkan bahwa hasil evaluasi berupa precision dari content based filtering adalah 5/5 = 100%

### Hasil Evaluasi Collaborative Filtering

![__results___82_0](https://github.com/user-attachments/assets/54591650-c376-4bc2-9172-08c114149bf6)

Dari hasil evaluasi collaborative filtering diatas dapat dilihat bahwa root mean squared error untuk training setiap epochs-nya selalu turun akan tetapi root mean squared error untuk validation error-nya terjadi peningkatan sedikit akan tetapi tidak signifikan. Hal ini dapat dikatakan bahwa model mengalami sedikit overfitting akan tetapi secara hasil rekomendasi sudah memuaskan.

## Jawaban Dari Problem Statement 
1. Berikut merupakan hasil dari rekomendasi content based filtering :

![Screenshot 2024-09-11 111327](https://github.com/user-attachments/assets/41834c2c-d134-4351-ab36-7897f27e0ab0)

2. Berikut merupakan hasil dari rekomendasi collaborative filtering :

![Screenshot 2024-09-11 111535](https://github.com/user-attachments/assets/871ff99a-0cd6-4489-ace5-3a678696bb4b)

3. Sebenarnya kedua metode rekomendasi saling melengkapi dan memiliki tujuan berbeda dimana content based filtering lebih ke karakteristik komponen item yang telah disukai pengguna sesuai riwayatnya sedangkan collaborative learning lebih ke mencari preferensi pengguna terhadap pengguna lainnya yang serupa sehingga keduanya saling melengkapi.

## Kesimpulan
Proyek ini bertujuan untuk membangun sistem rekomendasi destinasi wisata Indonesia yang personal, menggabungkan content-based dan collaborative filtering untuk memberikan rekomendasi yang relevan dan akurat. Dengan solusi ini, diharapkan wisatawan dapat lebih mudah menemukan destinasi yang sesuai dengan minat mereka, sehingga meningkatkan kepuasan dan minat berwisata di Indonesia.
