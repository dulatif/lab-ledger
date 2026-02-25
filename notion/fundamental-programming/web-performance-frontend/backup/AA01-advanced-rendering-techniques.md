# 📘 Silabus: Advanced Rendering Techniques (AA01)

**Judul Pembelajaran: Melampaui Sisi Klien: Teknik Rendering Lanjutan dengan SSR dan Virtualisasi**

Optimasi di sisi klien memiliki batas. Untuk performa tingkat lanjut, kita perlu mempertimbangkan bagaimana dan di mana konten di-_render_. Kursus ini akan membawa Anda melampaui _Client-Side Rendering_ (CSR) dengan menjelajahi **Server-Side Rendering (SSR)** menggunakan **Next.js**, dan teknik **virtualisasi** untuk menangani data dalam jumlah masif.

### 🎯 Tujuan Utama Pembelajaran

Setelah menyelesaikan kursus ini, Anda akan mampu:

1. **Memahami _Trade-Off_ Rendering:** Menjelaskan perbedaan dan _trade-off_ antara _Client-Side Rendering_ (CSR), _Server-Side Rendering_ (SSR), dan _Static Site Generation_ (SSG).
2. **Mengimplementasikan SSR dengan Next.js:** Menggunakan `getServerSideProps` di Next.js untuk melakukan _data fetching_ dan _rendering_ di sisi server.
3. **Meningkatkan Performa Awal Muat:** Menganalisis bagaimana SSR dapat secara dramatis meningkatkan metrik _First Contentful Paint_ (FCP) dan LCP.
4. **Menguasai Virtualisasi (_Windowing_):** Menggunakan _library_ seperti `react-window` untuk merender daftar dan grid yang sangat besar secara efisien.
5. **Membuat Keputusan Arsitektur Rendering:** Memilih strategi rendering yang tepat (CSR, SSR, SSG, atau hibrida) berdasarkan kebutuhan spesifik sebuah halaman atau aplikasi.

### 🗺️ Alur Pembelajaran

Kita akan mulai dari memahami berbagai strategi rendering, lalu menyelami SSR, kemudian beralih ke teknik untuk data masif (virtualisasi), dan diakhiri dengan cara memilih strategi yang tepat.

```mermaid
graph TD
    A[Modul 1: Spektrum Strategi Rendering] --> B[Modul 2: Server-Side Rendering (SSR) dengan Next.js];
    B --> C[Modul 3: Dampak SSR pada Performa & SEO];
    C --> D[Modul 4: Menangani Data Masif (Virtualisasi)];
    D --> E[Modul 5: Memilih Strategi Rendering yang Tepat];
    E --> F[Proyek: Membangun Halaman Daftar Produk SSR];

    subgraph "Teori & Konsep"
        A
        E
    end

    subgraph "Teknik Implementasi"
        B
        C
        D
    end

    subgraph "Aplikasi"
        F
    end
```

### 📚 Modul Pembelajaran

Berikut adalah rincian materi dari setiap modul.

### 🗺️ Modul 1: Spektrum Strategi Rendering

**Tujuan Modul:**

- Menganalisis cara kerja _Client-Side Rendering_ (CSR) dan *trade-off*nya.
- Memahami _Server-Side Rendering_ (SSR) sebagai solusi untuk FCP cepat dan SEO.
- Memahami _Static Site Generation_ (SSG) sebagai opsi tercepat untuk konten statis.
- Mengenal strategi hibrida seperti _Incremental Static Regeneration_ (ISR).

**Daftar Lesson:**

- **Lesson 1.1:** CSR: Pro dan Kontra.
- **Lesson 1.2:** Pengantar SSR.
- **Lesson 1.3:** Pengantar SSG.
- **Lesson 1.4:** Pendekatan Hibrida.

**Aktivitas Utama Modul:**

- 🗣️ **Diskusi:** Peserta mendiskusikan jenis aplikasi atau situs web apa yang paling cocok untuk setiap strategi rendering (CSR, SSR, SSG).

### ⚙️ Modul 2: Server-Side Rendering (SSR) dengan Next.js

**Tujuan Modul:**

- Menyiapkan proyek Next.js dengan TypeScript.
- Menggunakan fungsi `getServerSideProps` untuk mengambil data di server.
- Melewatkan data sebagai _props_ ke komponen halaman.
- Memahami alur _request/response_ dalam aplikasi SSR.

**Daftar Lesson:**

- **Lesson 2.1:** Pengantar Next.js untuk SSR.
- **Lesson 2.2:** Fungsi `getServerSideProps`.
- **Lesson 2.3:** Mengambil Data di Server.
- **Lesson 2.4:** Alur Hidup Halaman SSR.

**Aktivitas Utama Modul:**

- 💻 **Latihan:** Peserta membuat halaman Next.js sederhana yang mengambil data dari API publik di dalam `getServerSideProps` dan menampilkannya.

### 🚀 Modul 3: Dampak SSR pada Performa dan SEO

**Tujuan Modul:**

- Menganalisis _Network tab_ untuk melihat bagaimana halaman SSR dikirim sebagai HTML penuh.
- Membandingkan skor FCP dan LCP dari halaman CSR vs. SSR.
- Memahami mengapa SSR sangat baik untuk _Search Engine Optimization_ (SEO).
- Membahas _trade-off_ SSR: _Time to First Byte_ (TTFB) yang lebih lama.

**Daftar Lesson:**

- **Lesson 3.1:** Menganalisis Respons SSR.
- **Lesson 3.2:** Peningkatan Drastis pada Metrik _Paint_.
- **Lesson 3.3:** SSR dan SEO.
- **Lesson 3.4:** _Trade-off_ SSR: TTFB.

**Aktivitas Utama Modul:**

- 📊 **Latihan Perbandingan:** Peserta menjalankan audit Lighthouse pada halaman CSR dan halaman SSR yang setara, lalu membandingkan skor performa, terutama FCP dan LCP.

### 📜 Modul 4: Menangani Data Masif (Virtualisasi)

**Tujuan Modul:**

- Memahami masalah performa dari merender ribuan elemen DOM.
- Menggunakan _library_ `react-window` untuk membuat daftar virtual.
- Mengimplementasikan virtualisasi untuk daftar dengan tinggi item yang tetap dan dinamis.
- Mengimplementasikan virtualisasi untuk grid.

**Daftar Lesson:**

- **Lesson 4.1:** Masalah dengan Daftar Panjang.
- **Lesson 4.2:** Pengantar Virtualisasi dengan `react-window`.
- **Lesson 4.3:** Daftar dengan Tinggi Tetap vs. Dinamis.
- **Lesson 4.4:** Grid Virtual.

**Aktivitas Utama Modul:**

- 📜 **Latihan:** Peserta membuat sebuah komponen yang merender daftar 10.000 item menggunakan `react-window` dan mengamati betapa cepatnya proses _render_ awal.

### 🤔 Modul 5: Memilih Strategi Rendering yang Tepat

**Tujuan Modul:**

- Membuat pohon keputusan untuk memilih strategi _rendering_.
- Menganalisis kasus penggunaan: Blog (SSG), Dasbor (CSR/SSR), Halaman Produk E-commerce (SSG/ISR).
- Memahami bagaimana Next.js memungkinkan rendering per halaman.
- Merancang arsitektur hibrida yang optimal.

**Daftar Lesson:**

- **Lesson 5.1:** Pohon Keputusan _Rendering_.
- **Lesson 5.2:** Studi Kasus: Blog vs. Dasbor.
- **Lesson 5.3:** Rendering Per Halaman di Next.js.
- **Lesson 5.4:** Merancang Arsitektur Hibrida.

**Aktivitas Utama Modul:**

- 🚀 **Proyek: Membangun Halaman Daftar Produk SSR:** Peserta membangun sebuah halaman daftar produk untuk situs e-commerce menggunakan Next.js. Halaman ini harus di-_render_ di sisi server (`getServerSideProps`) untuk SEO dan FCP yang cepat. Mereka juga harus mengimplementasikan virtualisasi jika daftar produknya sangat panjang.

### 📖 Sumber Belajar Tambahan

- **Dokumentasi:**
- **Library:**
