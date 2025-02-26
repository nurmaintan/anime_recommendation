# Laporan Proyek Machine Learning - Nurma Intan Harianja

## Project Overview
Dalam era digital saat ini, popularitas anime telah meningkat secara signifikan, dengan banyaknya judul yang tersedia di berbagai platform streaming. Fenomena ini sering kali membuat penggemar kesulitan menemukan anime yang sesuai dengan preferensi mereka di antara ribuan pilihan yang ada.

Sistem rekomendasi berbasis konten hadir sebagai solusi untuk masalah ini. Pendekatan ini menganalisis karakteristik konten, seperti genre, untuk merekomendasikan anime yang serupa dengan yang telah disukai atau ditonton sebelumnya oleh pengguna. Dengan demikian, sistem dapat memberikan rekomendasi yang lebih personal dan relevan tanpa bergantung pada data pengguna lain.

Beberapa studi sebelumnya telah mengimplementasikan sistem rekomendasi berbasis konten untuk anime. Misalnya, penelitian yang dilakukan oleh Ikhsan Rifky Baihaqi menerapkan content-based filtering pada sistem rekomendasi anime berbasis semantik, yang membantu pengguna memilih anime berdasarkan karakteristik konten 
[1].

Dalam proyek ini, sistem rekomendasi dikembangkan dengan menganalisis genre anime menggunakan teknik seperti TF-IDF (Term Frequency-Inverse Document Frequency) dan Cosine Similarity untuk mengukur kesamaan antar anime. Pendekatan ini memungkinkan sistem untuk merekomendasikan anime yang sesuai dengan preferensi pengguna secara efisien dan efektif.

Dengan demikian, sistem rekomendasi berbasis konten ini diharapkan dapat meningkatkan pengalaman pengguna dalam menemukan anime yang sesuai dengan minat mereka, serta mendukung pertumbuhan industri anime dengan menghubungkan penggemar dengan konten yang relevan.

Referensi:

[1] [Baihaqi, I. R. (2021). Penerapan Content-Based Filtering Pada Sistem Rekomendasi Anime Berbasis Semantik. Skripsi Program Studi Teknik Informatika, STMIK Widya Cipta Dharma Samarinda.](https://repository.wicida.ac.id/3899/2/1743011-S1-Jurnal.pdf)

## Business Understanding

### Problem Statements
- Bagaimana merancang sistem rekomendasi anime yang dapat membantu pengguna menemukan anime yang sesuai dengan preferensi mereka?
- Metode apa yang paling efektif untuk mengukur kesamaan (similarity) antar anime sehingga rekomendasi yang diberikan dapat memenuhi keinginan pengguna?

### Goals
- Mengembangkan sistem rekomendasi anime berbasis konten yang dapat membantu pengguna menemukan anime baru yang sesuai dengan minat mereka.
- Mengevaluasi dan menentukan metode pengukuran kesamaan antar anime yang paling efektif untuk sistem rekomendasi berbasis konten.

### Solution Statements
- Implementasi algoritma content-based filtering untuk memberikan rekomendasi berdasarkan kesamaan genre dan fitur konten lainnya.
- Pengujian berbagai metode pengukuran similarity, seperti Cosine Similarity dan Euclidean Distance, untuk menentukan mana yang paling efektif.
- Mengatur parameter dalam algoritma rekomendasi untuk meningkatkan akurasi dan personalisasi rekomendasi.
- Analisis hasil evaluasi untuk memastikan sistem rekomendasi memberikan hasil yang akurat dan sesuai dengan harapan pengguna.

## Data Understanding
Dataset ini mencakup berbagai anime yang terdaftar di platform penyedia konten seperti MyAnimeList, dengan fitur yang mencakup nama anime, genre, rating, dan lainnya. Setiap pengguna dapat menambahkan anime ke daftar tontontan mereka dan memberikan rating, dataset ini merupakan kompilasi dari rating tersebut. Kumpulan data ini berisi informasi tentang data preferensi pengguna dari 73.516 pengguna di 12.294 anime. Dataset ini menyediakan informasi yang relevan untuk analisis dan pembuatan rekomendasi. 

Sumber: [Anime Recommendations Database](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database)

Deskripsi Variabel :
- anime_id: ID unik untuk setiap anime.
- name: Judul anime.
- genre: Genre atau jenis anime.
- type: movie, TV, OVA, dll.
- episodes: jumlah episode.
- rating: Rating yang diberikan oleh pengguna.
- members: jumlah anggota/pengguna yang ada di grup anime tersebut.

Jumlah Baris (Entries): 12294 baris.
Jumlah Kolom (Columns): 7 kolom data.

### Kesalahan Tipe Data

![image](https://github.com/user-attachments/assets/9df16cca-04d4-4fd9-b60d-044ab3a69dbf)

Kolom episodes memiliki tipe data object yang perlu dikonversi menjadi int64.

### Missing Value

![image](https://github.com/user-attachments/assets/e323acc0-541a-49a6-9f55-f7d1006113e3)

Variabel genre, type, dan rating memiliki beberapa missing value.

### Duplikasi Data

![image](https://github.com/user-attachments/assets/553e977e-fe22-4f45-aa8c-73ef009a72e6)

Beberapa anime tercatat lebih dari sekali dalam dataset.

## Exploratory Data Analysis

### Univariate Analysis

**Kolom numerikal**

Statistik Deskriptif:

![image](https://github.com/user-attachments/assets/7562b73b-0c3f-46bc-93a0-86f431d0c961)

- Rata-rata rating anime adalah sekitar 6.48
- Minimum rating adalah 1.67, dan maksimum adalah 10.
- Sebagian besar anime memiliki jumlah episode yang rendah, dengan rata-rata 12.43 episode. Hal ini menunjukkan bahwa banyak anime yang relatif singkat (misalnya, episode 12 adalah angka umum untuk season).
- Rata-rata jumlah anggota (members) anime adalah sekitar 18.159, dengan jumlah anggota yang sangat tinggi untuk anime populer (mencapai lebih dari 1 juta anggota).

Rating:

![image](https://github.com/user-attachments/assets/bc54dee1-6abc-4f59-980e-74088bbab028)

- Distribusi rating terlihat normal, dengan puncak di sekitar nilai 6-7, menunjukkan bahwa sebagian besar anime mendapatkan penilaian yang cukup baik, tetapi tidak luar biasa tinggi.
- Ada outliers pada rating dengan beberapa anime mendapatkan rating 10 (terbaik), meskipun jumlahnya sangat sedikit.

Episodes:

![image](https://github.com/user-attachments/assets/b71859e8-9d98-46ad-91e4-217d426ae3d2)

- Sebagian besar anime memiliki jumlah episode yang sangat sedikit, dengan banyak anime memiliki hanya satu episode (film) atau sedikit episode.
- Distribusi dengan log scale menunjukkan bahwa ada beberapa anime dengan jumlah episode yang sangat tinggi (misalnya, lebih dari 100 episode), tetapi ini sangat jarang terjadi.

Members:

![image](https://github.com/user-attachments/assets/7b03680b-1552-457e-9f75-d47f31e9995f)

- Ada banyak anime dengan jumlah anggota yang sangat rendah, tetapi anime yang lebih populer mendapatkan jumlah anggota yang jauh lebih tinggi.
- Terdapat skewness yang kuat, dengan beberapa anime yang sangat populer menambah jumlah anggota secara signifikan.

**Kolom kategorikal**

Genre:

![image](https://github.com/user-attachments/assets/b7f5d33a-b27a-4250-9549-943c5e433d69)

![image](https://github.com/user-attachments/assets/b2a8990e-499f-43ce-ada9-1bed5306b778)

- Genre anime sering kali memiliki beberapa kombinasi genre, seperti "Action, Comedy, Drama", yang menunjukkan bahwa banyak anime dapat digolongkan dalam lebih dari satu kategori.
- Genre yang paling sering muncul adalah Hentai, Comedy, dan Music, yang menunjukkan bahwa genre-genre ini paling umum dalam dataset.
- Heterogenitas genre (beragamnya kombinasi genre) menunjukkan bahwa penonton anime mungkin memiliki selera yang sangat beragam, dan anime sering kali menarik dari berbagai jenis cerita atau tema.

Type:

![image](https://github.com/user-attachments/assets/8c54903f-643c-4f6c-a846-ed36d693b3da)

![image](https://github.com/user-attachments/assets/87c76965-6784-4833-acf3-1e93ae8fb74b)

- Jenis anime yang paling banyak ditemukan adalah TV dan OVA, yang menunjukkan bahwa sebagian besar anime dalam dataset ini adalah serial anime dengan durasi lebih panjang atau produksi khusus.
- Movie, Special, dan ONA hadir dalam jumlah yang lebih sedikit, yang menunjukkan bahwa anime dalam format-film atau spesial lebih jarang dibandingkan dengan serial TV.

## Data Preparation

### Menangani Kesalahan Tipe Data

Kolom episodes harus diubah dari object menjadi int64 karena merupakan variabel numerik. Namun, kolom episodes mengandung nilai yang tidak dapat dikonversi menjadi integer, seperti Unknown. Maka, perlu menangani nilai yang hilang terlebih dahulu sebelum mengonversinya ke tipe int64.

```python
# Imputasi nilai Unknown pada 'episodes' dengan modus (nilai yang paling sering muncul)
anime['episodes'] = anime['episodes'].replace('Unknown', pd.NA).fillna(anime['episodes'].mode()[0])
```

```python
# Mengonversi kolom 'episodes' ke tipe integer
anime['episodes'] = anime['episodes'].astype('int64')
```

### Menangani Missing Value

Karena jumlah missing values tidak terlalu banyak dapat dilakukan penghapusan pada missing values.

```python
anime.dropna(subset=['genre', 'type', 'rating'], inplace=True)
```

### Menangani duplikasi

Ada beberapa anime yang tercatat lebih dari satu, maka dari itu kita perlu melakukan drop agar hasil rekomendasi lebih akurat.

```python
anime = anime[~anime['name'].duplicated(keep='first')]
```

## Feature Engineering

Memilih kolom yang relevan terlebih dahulu, yaitu 'anime_id', 'name', 'genre'.

```python
rec_anime = anime[['anime_id', 'name', 'genre']]
rec_anime.head()
```

Lalu, dilakukan pembersihan dan standarisasi kolom genre untuk memastikan konsistensi format. Proses ini penting untuk TF-IDF Vectorization karena tanda hubung dan spasi berpengaruh.

![image](https://github.com/user-attachments/assets/e71f62a2-59c9-4de1-a428-03220bd7abbc)

Setelah melakukan standarisasi pada genre, kolom diubah menjadi list untuk memudahkan pemrosesan data menggunakan TfidfVectorizer dari scikit-learn.

```python
anime_id = rec_anime['anime_id'].tolist()
anime_name = rec_anime['name'].tolist()
anime_genre = rec_anime['genre'].tolist()
```

## One Hot Encoding

Tahap ini bertujuan untuk mengubah genre menjadi format numerik yang bisa diproses oleh model machine learning.

```python
genre_list = []

# Membuat daftar genre unik
for index in anime_new.index:
    temp = anime_new['genre'][index].split(',')
    for i in temp:
        if i not in genre_list:
            genre_list.append(i)

onehot_df = pd.DataFrame(0, index=anime_new.index, columns=genre_list)

# Mengisi nilai 1 untuk genre yang sesuai
for index in anime_new.index:
    temp = anime_new['genre'][index].split(',')
    for i in temp:
        onehot_df.loc[index, i] = 1

anime_new = pd.concat([anime_new, onehot_df], axis=1).fillna(0)
anime_new.head()
```

![image](https://github.com/user-attachments/assets/6e21ff1d-3609-4b32-be98-0cdc82def7fc)

0 berarti genre tidak ada pada anime, 1 berarti ada.

## TF-IDF Vectorizer

Tahap untuk menghitung bobot untuk setiap genre berdasarkan frekuensi kemunculan di seluruh dataset.

```python
from sklearn.feature_extraction.text import TfidfVectorizer

# Inisialisasi TfidfVectorizer
tf = TfidfVectorizer()

# Melakukan perhitungan idf pada data genre
tf.fit(anime_new['genre'])

# Mapping array dari fitur index integer ke fitur nama
tf.get_feature_names_out()
```

![image](https://github.com/user-attachments/assets/3189d7a0-07f5-45b7-bd66-1f7c7b178ac7)

## Modelling

### Cosine Similarity

Cosine Similarity adalah sebuah metrik yang digunakan untuk mengukur sejauh mana dua vektor (atau representasi numerik) memiliki arah yang sama, terlepas dari panjang vektor tersebut. Metode ini banyak digunakan dalam sistem rekomendasi, terutama dalam analisis teks atau data yang dapat direpresentasikan dalam bentuk vektor, seperti dalam kasus sistem rekomendasi berbasis konten untuk anime atau film.
Cosine Similarity digunakan untuk mengukur seberapa mirip genre antar anime. Semakin tinggi nilai similarity, semakin relevan anime tersebut untuk direkomendasikan kepada pengguna. 

```python
from sklearn.metrics.pairwise import cosine_similarity

# Menghitung cosine similarity pada matrix tf-idf
cosine_sim = cosine_similarity(tfidf_matrix)
cosine_sim
```

**Rumus Cosine Similarity**
![image](https://github.com/user-attachments/assets/75fe0696-d4e0-45a0-a29a-837a37c181a2)

Di mana:
- A⋅B adalah hasil perkalian dot (produk titik) antara vektor 𝐴 dan 𝐵.
- ∥𝐴∥ adalah panjang (norma) vektor 𝐴.
- ∥𝐵∥ adalah panjang (norma) vektor 𝐵.

**Kelebihan:**
- Tidak Terpengaruh oleh Panjang Vektor: Cosine Similarity mengukur arah, bukan panjang, sehingga cocok untuk data yang memiliki dimensi yang sangat besar atau sparsity (kejarangan).
- Mudah Dipahami dan Implementasi: Konsepnya yang sederhana membuatnya mudah dipahami dan diimplementasikan dalam sistem rekomendasi berbasis konten.
- Efektif untuk Data Teks: Cocok untuk teks atau data kategori lainnya, karena dapat menghitung kesamaan antar dokumen atau entitas berdasarkan fitur atau kategori mereka.

**Kekurangan:**
- Tidak Mempertimbangkan Urutan: Cosine Similarity hanya mengukur kesamaan arah, tidak mempertimbangkan urutan kata dalam teks atau elemen dalam vektor.
- Tidak Optimal untuk Perbedaan Kecil: Jika dua vektor sangat mirip tetapi memiliki sedikit perbedaan, cosine similarity mungkin tidak memberikan informasi yang cukup akurat tentang seberapa mirip mereka sebenarnya.

**Hasil Rekomendasi Cosine Similarity untuk anime 'Shingeki no Kyojin':**

![image](https://github.com/user-attachments/assets/b64d7c29-31d5-4f23-905c-46deb41e6513)

Hasil rekomendasinya dapat dilihat pada gambar di bawah ini.
![image](https://github.com/user-attachments/assets/27dec55f-8eaa-4ebf-83d4-e5949e4a19e9)

### Euclidean Distance
Euclidean Distance adalah salah satu metode yang paling umum digunakan untuk mengukur jarak antara dua titik dalam ruang vektor, baik itu dalam konteks ruang dua dimensi, tiga dimensi, atau lebih tinggi. Metode ini banyak digunakan dalam berbagai aplikasi, termasuk dalam sistem rekomendasi berbasis konten untuk mengukur seberapa mirip dua objek berdasarkan atribut mereka. 
Euclidean Distance mengukur jarak antar anime berdasarkan genre yang direpresentasikan dalam matriks TF-IDF. Semakin kecil jarak, semakin mirip anime tersebut.

```python
def anime_euclidean(anime_name, similarity_data=euclidean_sim_df, items=anime_new[['name', 'genre']], k=10):
    similarity_scores = similarity_data[anime_name].to_numpy()

    closest_indices = similarity_scores.argsort()[:k]

    closest_animes = similarity_data.columns[closest_indices]

    closest_animes = closest_animes.drop(anime_name, errors='ignore')

    if len(closest_animes) < k:
        additional_animes = similarity_data.columns[similarity_scores.argsort()[k:2*k]]
        closest_animes = closest_animes.append(additional_animes).drop_duplicates()

    result_euclidean = pd.DataFrame(closest_animes[:k], columns=['name'])

    result_euclidean = result_euclidean.merge(items, on='name', how='left')

    return result_euclidean.head(k)
```
**Rumus Euclidean Distance**
![image](https://github.com/user-attachments/assets/e8aab047-c110-4e02-974f-9c55659243c2)

Di mana:
- A = (a1,a2,...an) dan B=(b1,b2,...,bn) adalah dua vektor yang dibandingkan.
- ai dan 𝑏𝑖 adalah elemen-elemen dari vektor 𝐴 dan 𝐵, masing-masing.​

**Kelebihan:**
- Mudah Dipahami dan Intuitif: Euclidean Distance mudah dipahami dan cukup intuitif karena mengukur jarak dalam ruang geometris.
- Sederhana dalam Implementasi: Algoritma untuk menghitung Euclidean Distance sangat mudah diterapkan, yang membuatnya cocok untuk banyak aplikasi.
- Efektif pada Data dengan Skala yang Sama: Metode ini efektif ketika data yang digunakan memiliki skala yang sama atau hampir sama.

**Kekurangan:**
- Sensitif terhadap Skala Data: Jika fitur-fitur dalam data memiliki skala yang berbeda, fitur dengan skala lebih besar dapat mendominasi perhitungan Euclidean Distance, sehingga distorsi dapat terjadi. Misalnya, jika satu fitur memiliki nilai yang jauh lebih besar daripada fitur lain, maka perbedaan pada fitur kecil tidak akan berpengaruh besar pada jarak.
Solusi: Normalisasi atau standarisasi data sebelum menghitung Euclidean Distance untuk mengatasi masalah ini.
- Tidak Mempertimbangkan Korelasi Antar Fitur: Euclidean Distance tidak mempertimbangkan hubungan atau korelasi antar fitur. Jadi, meskipun dua vektor memiliki nilai yang sangat berbeda pada beberapa elemen, mereka mungkin tetap dianggap mirip jika jarak antar elemen-elemen lainnya kecil.
- Kesulitan pada Data Sparse: Jika data sangat jarang atau sparse (seperti banyak nol pada vektor), maka Euclidean Distance mungkin tidak dapat memberikan hasil yang efektif karena perhitungan jarak berdasarkan elemen-elemen yang tidak ada (nol) bisa menjadi kurang berarti.

**Hasil Rekomendasi Cosine Similarity untuk anime 'Death Note':**

![image](https://github.com/user-attachments/assets/40c937b0-7718-4895-bc60-7b699e5a1001)

Hasil rekomendasinya dapat dilihat pada gambar di bawah ini.

![image](https://github.com/user-attachments/assets/7332763d-5e22-4946-bafc-72a5dc35bce2)

## Evaluation

Untuk mengevaluasi kinerja sistem rekomendasi, metrik Precision at k (P@k) digunakan untuk mengukur seberapa banyak rekomendasi yang relevan ada di antara 10 rekomendasi teratas. 

![image](https://github.com/user-attachments/assets/7937c0fa-3d8a-41e8-b017-aacaec743b23)

Dimana:

- Jumlah rekomendasi relevan dalam k teratas: Ini adalah jumlah item yang relevan yang berhasil ditemukan dalam 10 rekomendasi teratas yang diberikan oleh sistem.
- k adalah jumlah rekomendasi teratas yang ingin dievaluasi. Biasanya, nilai k diatur menjadi 10, yaitu sistem akan mengevaluasi 10 rekomendasi teratas.

Dalam proyek ini, dua pendekatan, yaitu Cosine Similarity dan Euclidean Distance, diterapkan untuk menghasilkan rekomendasi. Kedua metode tersebut menghasilkan daftar game yang dianggap sesuai dengan preferensi pengguna, dan metrik Precision digunakan untuk mengevaluasi seberapa banyak game yang relevan terdapat dalam 10 rekomendasi teratas.

![image](https://github.com/user-attachments/assets/6e30c0d4-45dc-4d6a-99b4-d1bfcbad9801)

Hasil perbandingan precision ini menunjukkan bahwa semua rekomendasi yang diberikan oleh kedua metode adalah relevan dan termasuk dalam 10 rekomendasi teratas. Dengan kata lain, kedua model berhasil memberikan hanya rekomendasi yang relevan dalam daftar rekomendasi teratas mereka.

## Kesimpulan

Sistem rekomendasi ini berhasil menunjukkan kinerja yang baik dengan menerapkan pendekatan Content-Based Filtering untuk mengukur kesamaan antar anime. Berdasarkan evaluasi, kedua metode, yaitu Cosine Similarity dan Euclidean Distance, memberikan hasil yang memuaskan dalam hal akurasi rekomendasi, dengan Precision yang tinggi pada 10 rekomendasi teratas. Sistem ini efektif membantu pengguna dalam menemukan anime yang sesuai dengan preferensi mereka secara lebih efisien, tanpa tergantung pada data pengguna lain.





