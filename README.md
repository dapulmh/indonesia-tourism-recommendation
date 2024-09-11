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

## Data Preparation
Langkah-langkah persiapan data:
- Merge Dataset : Mengintegrasikan dan menggabungkan kedua dataset sehingga menjadi satu dataset

- Missing Values: Penanganan data yang hilang dengan dengan menghilangkan data.

Berdasarkan data yang diperoleh tidak terdapat missing value sehingga tidak perlu diatasi

- Duplicate Values : Penanganan data yang duplikat dengan menghilangkan data.

Berdasarkan data yang diperoleh terdapat duplicate value sehingga perlu diatasi

- Mengurutkan urutan data dalam dataset berdasarkan id secara  ASCENDING

## Model Development
1. **Content-Based Filtering**:
   Model menggunakan metode content based filtering dengan TfidfVectorizer untuk menghitung cosine similiarity
   
### Cara Kerja 
1. Ekstraksi Fitur dari Item

Dalam tahap ini penulis menggunakan TfidVectorizer untuk mengekstrak fitur yang ada dalam konteks ini fitur yang diekstrak berupa atribut city_category 

2. Pembuatan Profil Pengguna

Dalam tahap ini penulis membentuk profil pengguna berdasarkan fitur yang diekstrak tadi dalam bentuk matrix yang disebut tfidf_matrix

3. Pencarian Item Serupa

Setelah profil pengguna dibuat, sistem akan membandingkan fitur-fitur dari item baru dengan profil pengguna. Sistem menghitung kesamaan antara item baru dan item yang disukai sebelumnya dengan metode cosine similiarity

4. Perekomendasian Item
Berdasarkan cosine similiarity tersebutlah, pengguna mendapatkan rekomendasi item berdasarkan kemiripan item yang telah dikonsumsi.

2. **Collaborative Filtering**:
   Model menggunakan metode collaborative filtering dengan deep learning tensorflow
   
### Cara Kerja 
1. Encoding fitur yang non numerik

Dalam tahap ini penulis men-encode fitur yang non numerik menjadi numerik.

2. Split training dan validation untuk data 

Dalam tahap ini penulis melakukan penyebaran untuk data training dan validation berupa 80:20.

3. Pembuatan model deep learning

Dalam tahap ini penulis membuat class model rekomendasi dengan tensorflow.

4. Inisiasi model tersebut
   
Dalam tahap ini penulis menginisiasikan model untuk menghitung binarycrossentropy dengan adam optimizer dan metrik rootmeansquared untuk errornya.

5. Melakukan Training Model
   
Dalam tahap ini penulis melakukan training untuk model

## Evaluasi Model
Evaluasi model dilakukan dengan metrik berikut:
- **Root Mean Squared Error (MSE)** Mengukur akar rata-rata dari erorr untuk collaborative filtering.
- **Accuracy**: Untuk melihat akurasi content based filtering

Model akan diuji pada data validasi untuk melihat performa dan melakukan tuning pada parameter model untuk hasil terbaik.

## Jawaban Dari Problem Statement 
1. Berikut merupakan hasil dari rekomendasi content based filtering :
2. Berikut merupakan hasil dari rekomendasi collaborative filtering :
3. Sebenarnya kedua metode rekomendasi saling melengkapi dan memiliki tujuan berbeda dimana content based filtering lebih ke karakteristik komponen item yang telah disukai pengguna sesuai riwayatnya sedangkan collaborative learning lebih ke mencari preferensi pengguna terhadap pengguna lainnya yang serupa sehingga keduanya saling melengkapi.
4.  
## Kesimpulan
Proyek ini bertujuan untuk membangun sistem rekomendasi destinasi wisata Indonesia yang personal, menggabungkan content-based dan collaborative filtering untuk memberikan rekomendasi yang relevan dan akurat. Dengan solusi ini, diharapkan wisatawan dapat lebih mudah menemukan destinasi yang sesuai dengan minat mereka, sehingga meningkatkan kepuasan dan minat berwisata di Indonesia.
