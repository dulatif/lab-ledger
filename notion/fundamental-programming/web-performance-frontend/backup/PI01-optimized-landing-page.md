# 📘 Silabus: Mini Proyek: Optimized Landing Page (PI01)

**Judul Pembelajaran: Kesan Pertama Secepat Kilat: Membangun Landing Page yang Teroptimasi Penuh**

_Landing page_ adalah pintu depan digital Anda. Jika lambat, calon pelanggan akan pergi sebelum sempat melihat apa yang Anda tawarkan. Dalam proyek ini, Anda akan membangun sebuah **landing page** dari awal dengan fokus utama pada **performa pemuatan**. Anda akan menerapkan berbagai teknik optimasi aset dan _rendering_ untuk mencapai skor Lighthouse yang mendekati sempurna.

### 🎯 Tujuan Utama Pembelajaran

Setelah menyelesaikan kursus ini, Anda akan mampu:

1. **Membangun dengan Pola Pikir Performa:** Merancang dan membangun _landing page_ dengan performa sebagai prioritas utama.
2. **Mengoptimalkan Semua Aset:** Mengkompresi gambar, memuat font secara efisien, dan meminimalkan CSS/JavaScript.
3. **Menerapkan _Lazy Loading_ secara Efektif:** Menggunakan _lazy loading_ untuk gambar dan komponen yang berada di bawah lipatan (_below the fold_).
4. **Mengoptimalkan Jalur _Rendering_ Kritis:** Memastikan konten utama dapat ditampilkan secepat mungkin tanpa diblokir oleh sumber daya yang tidak penting.
5. **Mencapai dan Mempertahankan Skor Lighthouse yang Tinggi:** Secara sistematis menerapkan rekomendasi dari Lighthouse untuk meningkatkan skor performa.

### 🗺️ Alur Pembelajaran

Proyek ini akan dibangun dengan siklus audit-dan-optimasi: membangun versi dasar, mengauditnya, lalu menerapkan perbaikan lapis demi lapis.

```mermaid
graph TD
    A[Fase 1: Setup & Versi Dasar] --> B[Fase 2: Audit Awal & Optimasi Gambar];
    B --> C[Fase 3: Optimasi Kode (CSS & JS)];
    C --> D[Fase 4: Lazy Loading & Optimasi Rendering];
    D --> E[Fase 5: Audit Final & Finalisasi];

    subgraph "Fondasi & Diagnosis"
        A
        B
    end

    subgraph "Implementasi Optimasi"
        C
        D
    end

    subgraph "Validasi"
        E
    end
```

### 📚 Modul Pembelajaran

Berikut adalah rincian materi dari setiap modul/fase.

### 🏗️ Fase 1: Setup dan Versi Dasar

**Tujuan Fase:**

- Membuat proyek React baru dengan TypeScript menggunakan Vite.
- Membangun struktur dasar dari _landing page_ dengan beberapa seksi (Hero, Fitur, Testimoni).
- Menggunakan gambar dan konten placeholder yang belum dioptimasi.

**Daftar Tugas:**

- **Tugas 1.1:** Inisialisasi proyek React + Vite.
- **Tugas 1.2:** Buat komponen untuk setiap seksi.
- **Tugas 1.3:** Susun halaman utama dengan konten placeholder.

**Aktivitas Utama:**

- Peserta memiliki _landing page_ yang fungsional namun belum teroptimasi, siap untuk diaudit.

### 📊 Fase 2: Audit Awal dan Optimasi Gambar

**Tujuan Fase:**

- Menjalankan audit Lighthouse pertama untuk mendapatkan skor performa awal.
- Mengidentifikasi gambar sebagai _bottleneck_ utama.
- Mengkompresi semua gambar ke format WebP menggunakan _tools_ seperti Squoosh.
- Mengganti gambar di dalam kode dengan versi yang sudah teroptimasi.

**Daftar Tugas:**

- **Tugas 2.1:** Jalankan dan simpan laporan Lighthouse awal.
- **Tugas 2.2:** Kompresi semua aset gambar.
- **Tugas 2.3:** Perbarui kode untuk menggunakan gambar baru.
- **Tugas 2.4:** Jalankan kembali audit untuk melihat peningkatan skor LCP.

**Aktivitas Utama:**

- Peserta melihat dampak langsung dari optimasi gambar pada performa pemuatan.

### 🔪 Fase 3: Optimasi Kode (CSS dan JS)

**Tujuan Fase:**

- Menganalisis ukuran _bundle_ CSS dan JavaScript.
- Memastikan _tree-shaking_ bekerja dengan benar.
- (Jika menggunakan _library_ CSS) Mengkonfigurasi _purging_ untuk menghapus CSS yang tidak terpakai.
- Mengidentifikasi dan mempertimbangkan untuk menghapus dependensi JavaScript yang berat dan tidak krusial.

**Daftar Tugas:**

- **Tugas 3.1:** Analisis ukuran _bundle_.
- **Tugas 3.2:** Konfigurasi _CSS purging_.
- **Tugas 3.3:** Refaktor untuk mengurangi ketergantungan pada _library_ berat.

**Aktivitas Utama:**

- Peserta berhasil mengurangi ukuran total dari aset kode mereka.

### ⏳ Fase 4: _Lazy Loading_ dan Optimasi _Rendering_

**Tujuan Fase:**

- Menerapkan atribut `loading="lazy"` pada gambar yang berada di bawah lipatan.
- Menggunakan `React.lazy` dan `Suspense` untuk melakukan _lazy load_ pada seksi/komponen yang tidak terlihat saat pertama kali dimuat (misalnya, seksi Testimoni).
- Mengoptimalkan pemuatan font web.
- Memastikan tidak ada _layout shift_ (CLS) yang disebabkan oleh aset yang dimuat.

**Daftar Tugas:**

- **Tugas 4.1:** Terapkan _lazy loading_ pada gambar.
- **Tugas 4.2:** Terapkan _lazy loading_ pada komponen.
- **Tugas 4.3:** Optimalkan pemuatan font.
- **Tugas 4.4:** Perbaiki masalah CLS.

**Aktivitas Utama:**

- Peserta berhasil mengurangi jumlah sumber daya yang harus dimuat pada pemuatan awal, mempercepat FCP dan LCP secara signifikan.

### ✅ Fase 5: Audit Final dan Finalisasi

**Tujuan Fase:**

- Menjalankan audit Lighthouse final.
- Membandingkan skor akhir dengan skor awal.
- Memoles detail UI.
- Menulis dokumentasi `README.md` yang menjelaskan optimasi yang telah dilakukan.
- Mendeploy _landing page_ yang sudah teroptimasi.

**Daftar Tugas:**

- **Tugas 5.1:** Jalankan audit Lighthouse final.
- **Tugas 5.2:** Buat laporan perbandingan.
- **Tugas 5.3:** Finalisasi kode dan _styling_.
- **Tugas 5.4:** Deploy ke Vercel atau Netlify.

**Aktivitas Utama:**

- **Proyek Akhir:** Peserta mendemonstrasikan _landing page_ mereka yang dimuat dengan sangat cepat. Mereka harus bisa menunjukkan laporan Lighthouse sebelum dan sesudah, serta menjelaskan secara spesifik teknik optimasi apa yang memberikan dampak terbesar.

### 📖 Sumber Belajar Tambahan

- **Dokumentasi:**
- **Tools:**
- **React:**
