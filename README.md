# Rekomendasi Destinasi Wisata Indonesia

## Latar Belakang
Pariwisata adalah sektor penting dalam perekonomian Indonesia. Dengan keberagaman budaya, alam, dan atraksi, Indonesia menawarkan berbagai destinasi menarik. Namun, banyak wisatawan yang kesulitan menemukan destinasi yang sesuai dengan preferensi mereka. Oleh karena itu, sistem rekomendasi yang personal dapat membantu mempermudah wisatawan dalam memilih destinasi yang sesuai dengan minat mereka.

## Business Understanding
Tujuan utama proyek ini adalah untuk membantu wisatawan dalam memilih destinasi wisata yang sesuai dengan preferensi mereka. Dengan menggunakan sistem rekomendasi berbasis konten (content-based) dan filter kolaboratif (collaborative filtering), pengguna akan mendapatkan rekomendasi destinasi yang dipersonalisasi berdasarkan data perilaku mereka atau kesamaan dengan pengguna lain.

## Problem Statement
- Bagaimana cara membangun dan merekomendasikan destinasi wisata berdasarkan kemiripan destinasi wisata pengguna menggunakan content based filtering?
- Bagaimana cara membangun dan merekomendasikan destinasi wisata berdasarkan kesamaan preferensi dengan pengguna lainnya menggunakan content based filtering?
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
    
Link Dataset : https://www.kaggle.com/datasets/aprabowo/indonesia-tourism-destination?select=tourism_with_id.csv

tourism_rating.csv
1. User_Id : Id user yang me-rating
2. Place_Id : Id destinasi wisata
3. Place_Ratings : Rating yang diberikan user

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

## Exploratory Data Analysis (EDA)
EDA akan dilakukan untuk:
- Memahami distribusi data destinasi, pengguna, dan interaksi.
- Menganalisis pola preferensi pengguna terhadap jenis wisata.
- Mengidentifikasi fitur penting dari destinasi yang mempengaruhi rating.
- Menilai apakah terdapat bias dalam data, misalnya destinasi yang sering dikunjungi versus yang jarang.

Contoh visualisasi dalam EDA:
- Distribusi rating per destinasi.
- Popularitas destinasi berdasarkan jumlah kunjungan.
- Hubungan antara jenis destinasi dan rating.

## Data Preparation
Langkah-langkah persiapan data:
- Membersihkan data yang hilang atau tidak konsisten.
- Menormalisasi data destinasi berdasarkan fitur deskriptif.
- Membagi data menjadi set pelatihan dan pengujian.
- Menyusun matriks pengguna-destinasi untuk collaborative filtering.

## Model Development
1. **Content-Based Filtering**:
   - Menggunakan fitur seperti deskripsi, lokasi, dan jenis wisata.
   - Menggunakan teknik TF-IDF atau word embeddings untuk merepresentasikan teks deskriptif.
   - Menghitung kemiripan antar destinasi menggunakan cosine similarity atau teknik lain.

2. **Collaborative Filtering**:
   - Model berbasis matriks pengguna-destinasi menggunakan pendekatan **matrix factorization** seperti SVD atau ALS.
   - Alternatif: menggunakan **neural collaborative filtering**.

3. **Hybrid Model**:
   - Menggabungkan kedua model di atas dengan bobot tertentu untuk menghasilkan rekomendasi final.

## Evaluasi Model
Evaluasi model dilakukan dengan metrik berikut:
- **Mean Squared Error (MSE)** untuk collaborative filtering.
- **Precision, Recall, dan F1-Score** untuk menilai relevansi rekomendasi.
- **Hit Rate**: seberapa sering destinasi yang direkomendasikan sesuai dengan pilihan pengguna.

Model akan diuji pada data validasi untuk melihat performa dan melakukan tuning pada parameter model untuk hasil terbaik.

## Kesimpulan
Proyek ini bertujuan untuk membangun sistem rekomendasi destinasi wisata Indonesia yang personal, menggabungkan content-based dan collaborative filtering untuk memberikan rekomendasi yang relevan dan akurat. Dengan solusi ini, diharapkan wisatawan dapat lebih mudah menemukan destinasi yang sesuai dengan minat mereka, sehingga meningkatkan kepuasan dan minat berwisata di Indonesia.
