# 📘 Silabus: Performance Monitoring (AE01)

**Judul Pembelajaran: Mata Elang di Produksi: Memantau Performa Web dengan Data Pengguna Nyata**

Optimasi yang dilakukan saat pengembangan tidak ada artinya jika tidak terbukti di dunia nyata. Kursus tingkat ahli ini akan mengajarkan Anda cara memantau dan mengukur performa aplikasi Anda di **produksi**, menggunakan data dari pengguna nyata. Anda akan belajar tentang **Real User Monitoring (RUM)** dan _error tracking_ untuk mendapatkan wawasan yang dapat ditindaklanjuti dan terus meningkatkan pengalaman pengguna.

### 🎯 Tujuan Utama Pembelajaran

Setelah menyelesaikan kursus ini, Anda akan mampu:

1. **Membedakan Data Lab dan Data Lapangan:** Memahami perbedaan krusial antara data sintetis (Lighthouse) dan data dari pengguna nyata (RUM).
2. **Mengimplementasikan _Real User Monitoring_ (RUM):** Mengintegrasikan _provider_ RUM (seperti Vercel Analytics atau pustaka pihak ketiga) untuk mengumpulkan metrik _Core Web Vitals_ dari pengguna.
3. **Menganalisis Data RUM:** Menginterpretasikan dasbor RUM untuk mengidentifikasi masalah performa yang dialami oleh segmen pengguna tertentu (berdasarkan perangkat, negara, dll.).
4. **Mengatur _Error Tracking_:** Mengintegrasikan layanan _error tracking_ (seperti Sentry) untuk menangkap dan menganalisis eror JavaScript yang terjadi di klien.
5. **Menciptakan Siklus Umpan Balik Performa:** Menggunakan data dari RUM dan _error tracking_ untuk memprioritaskan upaya optimasi di siklus pengembangan berikutnya.

### 🗺️ Alur Pembelajaran

Kita akan beralih dari lab ke dunia nyata: mempelajari cara mengumpulkan data dari pengguna, menganalisisnya, menangkap eror, dan terakhir, menggunakan semua wawasan ini untuk perbaikan berkelanjutan.

```mermaid
graph TD
    A[Modul 1: Lab vs. Dunia Nyata (RUM)] --> B[Modul 2: Implementasi Real User Monitoring];
    B --> C[Modul 3: Menganalisis Data dari Lapangan];
    C --> D[Modul 4: Menangkap "Kecelakaan" (Error Tracking)];
    D --> E[Modul 5: Siklus Perbaikan Berbasis Data];
    E --> F[Proyek: Mengintegrasikan Monitoring ke Aplikasi];

    subgraph "Konsep & Implementasi"
        A
        B
    end

    subgraph "Analisis & Aksi"
        C
        D
        E
    end

    subgraph "Aplikasi"
        F
    end
```

### 📚 Modul Pembelajaran

Berikut adalah rincian materi dari setiap modul.

### 🔬 Modul 1: Lab vs. Dunia Nyata (_Real User Monitoring_ - RUM)

**Tujuan Modul:**

- Memahami keterbatasan dari pengujian performa di lingkungan lab.
- Mendefinisikan _Real User Monitoring_ (RUM) sebagai proses pengumpulan data performa dari _browser_ pengguna.
- Memahami bagaimana RUM memberikan gambaran yang lebih akurat tentang pengalaman pengguna.
- Mengenal berbagai penyedia layanan RUM.

**Daftar Lesson:**

- **Lesson 1.1:** Keterbatasan Pengujian Lab.
- **Lesson 1.2:** Pengantar _Real User Monitoring_.
- **Lesson 1.3:** Kenapa Data Lapangan Itu Penting?
- **Lesson 1.4:** Peta Penyedia Layanan RUM.

**Aktivitas Utama Modul:**

- 🗣️ **Diskusi:** "Mengapa sebuah situs bisa mendapatkan skor Lighthouse 100 di laptop developer, tetapi pengguna di negara lain dengan koneksi lambat mengalami pengalaman yang buruk?"

### 📡 Modul 2: Implementasi _Real User Monitoring_

**Tujuan Modul:**

- Mengaktifkan Vercel Analytics untuk proyek yang di-hosting di Vercel.
- Mengintegrasikan pustaka `web-vitals` dari Google untuk pengumpulan manual.
- Mengirim data metrik yang dikumpulkan ke _endpoint_ analitik kustom atau layanan pihak ketiga.
- Memastikan skrip RUM dimuat secara asinkron agar tidak memengaruhi performa.

**Daftar Lesson:**

- **Lesson 2.1:** RUM Mudah dengan Vercel Analytics.
- **Lesson 2.2:** Pengumpulan Manual dengan `web-vitals`.
- **Lesson 2.3:** Mengirim Data Metrik.
- **Lesson 2.4:** Implementasi yang Berperforma.

**Aktivitas Utama Modul:**

- 💻 **Latihan:** Peserta mengaktifkan Vercel Analytics pada salah satu proyek Next.js/React mereka dan mulai melihat data masuk ke dasbor.

### 📊 Modul 3: Menganalisis Data dari Lapangan

**Tujuan Modul:**

- Membaca dasbor RUM.
- Menganalisis distribusi persentil (p75) dari metrik _Core Web Vitals_.
- Memfilter data berdasarkan negara, jenis perangkat, atau browser untuk menemukan pola.
- Mengidentifikasi halaman atau rute mana yang memiliki performa terburuk.

**Daftar Lesson:**

- **Lesson 3.1:** Membaca Dasbor RUM.
- **Lesson 3.2:** Fokus pada Persentil ke-75.
- **Lesson 3.3:** Segmentasi Pengguna untuk Wawasan.
- **Lesson 3.4:** Menemukan Halaman Bermasalah.

**Aktivitas Utama Modul:**

- 📊 **Analisis Data:** Peserta menganalisis dasbor Vercel Analytics mereka dan mencoba menjawab pertanyaan seperti: "Apakah pengguna mobile mengalami CLS yang lebih buruk daripada pengguna desktop?"

### 🐛 Modul 4: Menangkap "Kecelakaan" (_Error Tracking_)

**Tujuan Modul:**

- Mementingkan pentingnya _error tracking_ di frontend.
- Mengintegrasikan layanan seperti Sentry ke dalam aplikasi React.
- Menangkap eror JavaScript yang tidak tertangani secara otomatis.
- Melaporkan eror secara manual dengan konteks tambahan.
- Menganalisis laporan eror untuk melakukan _debugging_.

**Daftar Lesson:**

- **Lesson 4.1:** Kenapa `console.log` Tidak Cukup untuk Eror?
- **Lesson 4.2:** Pengantar Sentry.
- **Lesson 4.3:** Menangkap dan Melaporkan Eror.
- **Lesson 4.4:** Menganalisis Laporan Eror.

**Aktivitas Utama Modul:**

- 🐛 **Latihan:** Peserta mengintegrasikan Sentry ke aplikasi mereka, sengaja membuat sebuah eror, dan memverifikasi bahwa eror tersebut muncul di dasbor Sentry.

### 🔄 Modul 5: Siklus Perbaikan Berbasis Data

**Tujuan Modul:**

- Menggabungkan data dari RUM dan _error tracking_ untuk mendapatkan gambaran lengkap.
- Menggunakan data ini untuk memprioritaskan _backlog_ tugas teknis.
- Menetapkan hipotesis perbaikan dan mengukur dampaknya setelah di-deploy.
- Menciptakan budaya performa yang berbasis data di dalam tim.

**Daftar Lesson:**

- **Lesson 5.1:** Menghubungkan Titik-Titik Data.
- **Lesson 5.2:** Memprioritaskan Perbaikan Berdasarkan Dampak Nyata.
- **Lesson 5.3:** Siklus Hipotesis-Ukur-Iterasi.
- **Lesson 5.4:** Membangun Budaya Performa.

**Aktivitas Utama Modul:**

- 🚀 **Proyek: Mengintegrasikan Monitoring ke Aplikasi:** Peserta mengambil sebuah aplikasi React yang sudah ada. Tugas mereka adalah: (1) Mengintegrasikan layanan RUM (misalnya, dengan `web-vitals`). (2) Mengintegrasikan layanan _error tracking_ Sentry. (3) Menulis sebuah laporan singkat yang menganalisis data performa awal (jika ada) dan menjelaskan bagaimana mereka akan menggunakan kedua _tools_ ini untuk memandu upaya optimasi di masa depan.

### 📖 Sumber Belajar Tambahan

- **Dokumentasi:**
- **Library:**
- **Konsep:**
