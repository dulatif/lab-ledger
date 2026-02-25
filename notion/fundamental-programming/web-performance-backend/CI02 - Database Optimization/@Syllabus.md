# 📘 Silabus: Database Optimization (CI02)

**Judul Pembelajaran: Mempercepat Jantung Aplikasi: Optimasi Query dan Pengindeksan Database**

Database adalah jantung dari sebagian besar aplikasi _backend_. Jika jantungnya lambat, seluruh tubuh aplikasi akan ikut melambat. Kursus ini akan mengajarkan Anda teknik-teknik paling berdampak untuk mengoptimalkan performa database relasional (dengan fokus pada **PostgreSQL**), terutama melalui **optimasi _query_** dan **strategi pengindeksan** yang cerdas.

### 🎯 **Tujuan Utama Pembelajaran**

Setelah menyelesaikan kursus ini, Anda akan mampu:

1. **Menganalisis Rencana Eksekusi _Query_:** Menggunakan `EXPLAIN ANALYZE` untuk memahami bagaimana database mengeksekusi _query_ Anda.
2. **Mengidentifikasi _Query_ yang Tidak Efisien:** Membedakan antara _Sequential Scan_ yang lambat dan _Index Scan_ yang cepat.
3. **Menerapkan Strategi Pengindeksan:** Membuat indeks B-Tree pada kolom yang tepat untuk mempercepat operasi `WHERE`, `JOIN`, dan `ORDER BY`.
4. **Menulis _Query_ yang Lebih Efisien:** Merefaktor _query_ untuk memanfaatkan indeks dan menghindari _anti-patterns_ umum.
5. **Mengintegrasikan dengan ORM:** Memahami bagaimana menambahkan indeks melalui migrasi ORM dan bagaimana metode ORM diterjemahkan menjadi SQL.

### 🗺️ **Alur Pembelajaran**

Kita akan mulai dari diagnosis (EXPLAIN), lalu mempelajari solusi utamanya (indeks), bagaimana merancang indeks yang baik, dan terakhir, bagaimana menulis _query_ yang "ramah-indeks".

```
graph TD
    A[Modul 1: Menjadi Detektif Query (EXPLAIN ANALYZE)] --> B[Modul 2: Jalan Tol Database (Indeks)];
    B --> C[Modul 3: Merancang Indeks yang Tepat];
    C --> D[Modul 4: Menulis Query yang Ramah-Indeks];
    D --> E[Proyek: Mengoptimalkan Query API];

    subgraph "Diagnosis & Analisis"
        A
    end

    subgraph "Implementasi Optimasi"
        B
        C
        D
    end

    subgraph "Aplikasi"
        E
    end

```

### 📚 **Modul Pembelajaran**

Berikut adalah rincian materi dari setiap modul.

### **🕵️ Modul 1: Menjadi Detektif _Query_ (`EXPLAIN ANALYZE`)**

**Tujuan Modul:**

- Menggunakan `EXPLAIN` untuk melihat rencana eksekusi _query_.
- Menggunakan `EXPLAIN ANALYZE` untuk melihat rencana dan waktu eksekusi aktual.
- Menginterpretasikan output dasar: _cost, rows, node type_.
- Mengidentifikasi _Sequential Scan_ sebagai tanda pertama adanya masalah.

**Daftar Lesson:**

- **Lesson 2.1:** Pengantar _Query Planner_.
- **Lesson 2.2:** Membaca Rencana Eksekusi dengan `EXPLAIN`.
- **Lesson 2.3:** Melihat Kinerja Aktual dengan `EXPLAIN ANALYZE`.
- **Lesson 2.4:** Berburu _Sequential Scan_.

**Aktivitas Utama Modul:**

- 💻 **Latihan:** Peserta menjalankan `EXPLAIN ANALYZE` pada sebuah _query_ `SELECT ... WHERE` pada tabel besar tanpa indeks dan mengamati biaya serta waktu eksekusinya yang tinggi.

### **⚡ Modul 2: Jalan Tol Database (Indeks B-Tree)**

**Tujuan Modul:**

- Memahami indeks B-Tree sebagai struktur data utama untuk pengindeksan.
- Membuat indeks pada satu kolom (`CREATE INDEX`).
- Melihat dampak dramatis dari indeks pada performa _query_ `WHERE`.
- Memahami _trade-off_ indeks: mempercepat baca, memperlambat tulis.

**Daftar Lesson:**

- **Lesson 2.1:** Apa Itu Indeks?
- **Lesson 2.2:** Indeks B-Tree: Struktur Data di Balik Kecepatan.
- **Lesson 2.3:** Membuat Indeks Pertama Anda.
- **Lesson 2.4:** _Trade-off_ Pengindeksan.

**Aktivitas Utama Modul:**

- ⚡ **Latihan:** Peserta membuat indeks pada kolom yang di-_query_ di modul sebelumnya, lalu menjalankan `EXPLAIN ANALYZE` lagi untuk melihat perubahan dari _Sequential Scan_ menjadi _Index Scan_ dan penurunan waktu eksekusi yang drastis.

### **📐 Modul 3: Merancang Indeks yang Tepat**

**Tujuan Modul:**

- Membuat indeks komposit (_multi-column_) untuk _query_ yang memfilter beberapa kolom.
- Memahami pentingnya urutan kolom dalam indeks komposit.
- Membuat indeks untuk mempercepat operasi `ORDER BY`.
- Menggunakan ORM (misalnya, TypeORM) untuk menambahkan indeks melalui dekorator atau migrasi.

**Daftar Lesson:**

- **Lesson 3.1:** Indeks untuk _Query_ Multi-Kolom.
- **Lesson 3.2:** Pentingnya Urutan Kolom.
- **Lesson 3.3:** Mempercepat Pengurutan dengan Indeks.
- **Lesson 3.4:** Mengelola Indeks di Aplikasi (ORM).

**Aktivitas Utama Modul:**

- ✍️ **Latihan:** Peserta merancang dan membuat sebuah indeks komposit untuk mendukung _query_ yang mencari pengguna berdasarkan `status` dan mengurutkannya berdasarkan `created_at`.

### **✍️ Modul 4: Menulis _Query_ yang Ramah-Indeks**

**Tujuan Modul:**

- Memahami mengapa penggunaan fungsi pada kolom di `WHERE` dapat mencegah penggunaan indeks.
- Menghindari penggunaan `LIKE '%...'` di awal string.
- Memastikan tipe data dalam _query_ cocok dengan tipe data kolom.
- Memahami bagaimana `JOIN` dapat menggunakan indeks pada _foreign key_.

**Daftar Lesson:**

- **Lesson 4.1:** Menulis _Query_ yang Bisa "Dilihat" oleh Indeks.
- **Lesson 4.2:** Jebakan Fungsi di Klausa `WHERE`.
- **Lesson 4.3:** Jebakan `LIKE`.
- **Lesson 4.4:** Optimasi `JOIN`.

**Aktivitas Utama Modul:**

- 🚀 **Proyek: Mengoptimalkan _Query_ API:** Peserta diberi sebuah _endpoint_ API yang lambat karena _query_ N+1 dan _query_ tanpa indeks. Tugas mereka adalah: (1) Menggunakan `EXPLAIN ANALYZE` untuk mendiagnosis masalah. (2) Merefaktor kode untuk menyelesaikan masalah N+1 (misalnya, dengan satu `JOIN`). (3) Membuat indeks yang sesuai untuk mendukung _query_ yang telah dioptimalkan. (4) Mendokumentasikan peningkatan performa.

### 📖 **Sumber Belajar Tambahan**

- **Dokumentasi:**
    - [PostgreSQL Docs - EXPLAIN](https://www.postgresql.org/docs/current/sql-explain.html)
    - [PostgreSQL Docs - Indexes](https://www.postgresql.org/docs/current/indexes.html)
- **Situs Web:**
    - Use The Index, Luke! (situs yang didedikasikan untuk pengindeksan database).
- **Tools:**
    - pgMustard, `explain.depesz.com` (untuk visualisasi `EXPLAIN`).