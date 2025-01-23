# Materi Kuliah: Normalisasi Basis Data

## Pendahuluan

Normalisasi adalah proses sistematis untuk mengorganisasi data dalam basis data dengan tujuan mengurangi redundansi dan ketergantungan data yang tidak diinginkan. Proses ini memainkan peran krusial dalam perancangan basis data relasional yang efisien dan mudah dipelihara. Dengan normalisasi, kita dapat memastikan integritas data serta meningkatkan kinerja basis data.

**Normal Form (NF)** atau **Bentuk Normal** adalah aturan-aturan yang digunakan dalam proses normalisasi untuk mengklasifikasikan struktur tabel dalam basis data relasional. Bentuk Normal ini dirancang untuk mengatasi berbagai jenis anomali yang dapat terjadi saat memasukkan, mengupdate, atau menghapus data.

## Tujuan Normalisasi

1. **Menghilangkan Redundansi Data**: Mengurangi penyimpanan data yang berulang yang dapat menyebabkan inkonsistensi.
2. **Meningkatkan Integritas Data**: Menjamin bahwa data yang disimpan akurat dan konsisten.
3. **Memudahkan Pemeliharaan Basis Data**: Mempermudah proses pembaruan, penghapusan, dan penyisipan data.
4. **Meningkatkan Kinerja Basis Data**: Mengoptimalkan struktur tabel untuk operasi basis data yang lebih efisien.

## Bentuk Normalisasi

Normalisasi terdiri dari beberapa **Bentuk Normal (Normal Forms)** yang masing-masing memiliki aturan khusus yang harus dipenuhi. Bentuk normal ini dirancang untuk menangani berbagai jenis anomali dalam basis data.

### Bentuk Normal Pertama (1NF)

**Definisi**: Sebuah tabel berada dalam Bentuk Normal Pertama jika memenuhi kriteria berikut:
- Semua atribut dalam tabel harus memiliki nilai atomik (tidak dapat dibagi lagi).
- Tidak ada pengulangan kelompok data atau array dalam satu kolom.
- Setiap kolom harus memiliki nama yang unik.
- Urutan penyimpanan data dalam tabel tidak relevan.

**Contoh**:

*Tabel Tidak Normal*:

| ID_Pelanggan | Nama_Pelanggan | No_Telp                   |
|--------------|-----------------|---------------------------|
| 1            | Budi            | 08123456789, 08129876543  |
| 2            | Siti            | 08134567890                |

*Tabel Setelah 1NF*:

| pelanggan_id | nama_pelanggan | no_telp     |
|--------------|-----------------|-------------|
| 1            | Budi            | 08123456789 |
| 1            | Budi            | 08129876543 |
| 2            | Siti            | 08134567890 |

### Bentuk Normal Kedua (2NF)

**Definisi**: Sebuah tabel berada dalam Bentuk Normal Kedua jika:
- Sudah memenuhi Bentuk Normal Pertama.
- Setiap atribut non-primer sepenuhnya bergantung pada kunci utama.

**Contoh**:

*Tabel Tidak Normal*:

| ID_Pemesanan | pelanggan_id | nama_pelanggan | ID_Produk | nama_produk | jumlah |
|--------------|--------------|-----------------|-----------|-------------|--------|
| 1001         | 1            | Budi            | P01       | Buku        | 2      |
| 1002         | 2            | Siti            | P02       | Pulpen      | 5      |
| 1003         | 1            | Budi            | P03       | Pensil      | 3      |

*Masalah*: `nama_pelanggan` bergantung hanya pada `pelanggan_id`, bukan pada keseluruhan kunci utama (`ID_Pemesanan`, `ID_Produk`).

*Tabel Setelah 2NF*:

**Tabel Pemesanan**:

| ID_Pemesanan | pelanggan_id |
|--------------|--------------|
| 1001         | 1            |
| 1002         | 2            |
| 1003         | 1            |

**Tabel Pelanggan**:

| pelanggan_id | nama_pelanggan |
|--------------|-----------------|
| 1            | Budi            |
| 2            | Siti            |

**Tabel Detail_Pemesanan**:

| ID_Pemesanan | ID_Produk | jumlah |
|--------------|-----------|--------|
| 1001         | P01       | 2      |
| 1002         | P02       | 5      |
| 1003         | P03       | 3      |

### Bentuk Normal Ketiga (3NF)

**Definisi**: Sebuah tabel berada dalam Bentuk Normal Ketiga jika:
- Sudah memenuhi Bentuk Normal Kedua.
- Tidak ada ketergantungan transitif antara atribut non-primer dan kunci utama.

**Contoh**:

*Tabel Tidak Normal*:

| pelanggan_id | nama_pelanggan | kota        | kode_pos |
|--------------|-----------------|-------------|----------|
| 1            | Budi            | Jakarta     | 10001    |
| 2            | Siti            | Bandung     | 20002    |
| 3            | Agus            | Jakarta     | 10001    |

*Masalah*: `kode_pos` bergantung pada `kota`, bukan langsung pada `pelanggan_id`.

*Tabel Setelah 3NF*:

**Tabel Pelanggan**:

| pelanggan_id | nama_pelanggan | kota     |
|--------------|-----------------|----------|
| 1            | Budi            | Jakarta  |
| 2            | Siti            | Bandung  |
| 3            | Agus            | Jakarta  |

**Tabel Kota**:

| kota     | kode_pos |
|----------|----------|
| Jakarta  | 10001    |
| Bandung  | 20002    |

### Bentuk Normal Boyce-Codd (BCNF)

**Definisi**: Sebuah tabel berada dalam Bentuk Normal Boyce-Codd jika:
- Setiap determinan adalah kunci kandidat.
- Menangani anomali yang tidak dapat ditangani oleh 3NF.

**Contoh**:

*Tabel Tidak Normal*:

| mata_kuliah | dosen   | ruangan |
|-------------|---------|---------|
| Basis Data  | Dr. A   | R101    |
| Algoritma   | Dr. B   | R102    |
| Basis Data  | Dr. C   | R101    |

*Masalah*: Ruangan `R101` ditentukan oleh mata_kuliah "Basis Data", tetapi mata_kuliah tidak merupakan kunci.

*Tabel Setelah BCNF*:

**Tabel Mata_Kuliah**:

| mata_kuliah | dosen   |
|-------------|---------|
| Basis Data  | Dr. A   |
| Algoritma   | Dr. B   |

**Tabel Ruangan**:

| mata_kuliah | ruangan |
|-------------|---------|
| Basis Data  | R101    |
| Algoritma   | R102    |

## Proses Normalisasi

1. **Identifikasi Entitas dan Atribut**: Tentukan entitas utama dan atribut yang terkait.
2. **Menentukan Kunci Utama**: Pilih atribut atau kombinasi atribut yang dapat secara unik mengidentifikasi setiap baris dalam tabel.
3. **Menerapkan Bentuk Normal**: Terapkan aturan 1NF, kemudian 2NF, dan seterusnya hingga mencapai bentuk normal yang diinginkan.
4. **Memecah Tabel**: Pisahkan tabel yang melanggar aturan bentuk normal menjadi tabel-tabel yang lebih kecil.
5. **Menentukan Kunci Asing**: Hubungkan tabel-tabel yang telah dinormalisasi menggunakan kunci asing untuk menjaga hubungan antar tabel.

## Keuntungan Normalisasi

- **Reduksi Redundansi**: Mengurangi duplikasi data yang tidak perlu.
- **Integritas Data yang Lebih Baik**: Menjamin konsistensi dan akurasi data.
- **Pemeliharaan yang Lebih Mudah**: Mempermudah proses pembaruan dan pengelolaan basis data.
- **Kinerja yang Dioptimalkan**: Meningkatkan efisiensi query dan operasi basis data lainnya.

## Keterbatasan Normalisasi

- **Kompleksitas Query**: Basis data yang sangat dinormalisasi mungkin memerlukan join yang kompleks, yang dapat mempengaruhi kinerja.
- **Over-Normalisasi**: Bisa menyebabkan terlalu banyak tabel yang dapat menyulitkan pemahaman dan pengelolaan basis data.
- **Pertimbangan Kinerja**: Dalam beberapa kasus, denormalisasi (pengurangan tingkat normalisasi) mungkin diperlukan untuk meningkatkan kinerja aplikasi.

## Kesimpulan

Normalisasi merupakan langkah penting dalam perancangan basis data relasional yang efektif. Dengan mengikuti prinsip-prinsip normalisasi, desainer basis data dapat memastikan bahwa data tersimpan secara efisien, konsisten, dan mudah dikelola. Meskipun demikian, penting untuk menyeimbangkan antara normalisasi dan kebutuhan kinerja aplikasi untuk mencapai solusi basis data yang optimal.

## Referensi

1. Elmasri, R., & Navathe, S. B. (2015). *Fundamentals of Database Systems*. Pearson.
2. Codd, E. F. (1970). *A Relational Model of Data for Large Shared Data Banks*. Communications of the ACM.
3. Date, C. J. (2004). *An Introduction to Database Systems*. Addison-Wesley.