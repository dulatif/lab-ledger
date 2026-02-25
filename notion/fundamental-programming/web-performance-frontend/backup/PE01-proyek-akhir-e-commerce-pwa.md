# 📘 Silabus: Proyek Akhir: E-Commerce PWA (PE01)

**Judul Pembelajaran: Capstone: Membangun E-Commerce PWA Berperforma Tinggi**

Ini adalah puncak dari perjalanan performa frontend Anda. Anda akan menyatukan semua yang telah dipelajari—dari SSR, _lazy loading_, optimasi aset, hingga _monitoring_—untuk membangun sebuah **Frontend E-Commerce PWA** yang terinspirasi dari Quantum Flow. Proyek ini akan menantang Anda untuk membuat setiap interaksi, dari pemuatan halaman hingga penambahan ke keranjang, terasa instan.

### 🎯 Tujuan Utama Pembelajaran

Setelah menyelesaikan kursus ini, Anda akan mampu:

1. **Merancang Arsitektur E-Commerce Berperforma Tinggi:** Menggunakan strategi rendering hibrida (SSG/ISR untuk halaman produk, SSR untuk akun, CSR untuk keranjang).
2. **Mengimplementasikan Optimasi di Seluruh Alur:** Menerapkan _lazy loading_, optimasi gambar, dan _code splitting_ secara agresif di seluruh aplikasi.
3. **Membangun PWA yang _Offline-Capable_:** Menggunakan _service worker_ (via Workbox) untuk men-cache aset dan data, memungkinkan penjelajahan offline.
4. **Mengintegrasikan _Performance Monitoring_:** Menyiapkan _Real User Monitoring_ (RUM) dan _error tracking_ untuk memantau aplikasi di produksi.
5. **Mencapai dan Mempertahankan Skor Performa Elit:** Secara sistematis mengaudit dan mengoptimalkan aplikasi untuk mencapai skor Lighthouse >90.

### 🗺️ Alur Pembelajaran

Proyek ini akan dibangun dengan performa sebagai fitur utama, mengintegrasikan semua teknik optimasi dari awal hingga akhir.

```mermaid
graph TD
    A[Fase 1: Arsitektur Hibrida & Fondasi] --> B[Fase 2: Etalase Secepat Kilat (SSG/ISR)];
    B --> C[Fase 3: Interaksi Instan (Lazy Loading & CSR)];
    C --> D[Fase 4: Kemampuan Offline (Service Worker)];
    D --> E[Fase 5: Monitoring & Deployment];

    subgraph "Perencanaan & Infrastruktur"
        A
    end

    subgraph "Implementasi Fitur Berperforma"
        B
        C
        D
    end

    subgraph "Produksi & Pemeliharaan"
        E
    end
```

### 📚 Modul Pembelajaran

Berikut adalah rincian materi dari setiap modul/fase.

### 🏗️ Fase 1: Arsitektur Hibrida dan Fondasi

**Tujuan Fase:**

- Merancang arsitektur rendering hibrida untuk situs e-commerce.
- Mengatur proyek Next.js dengan TypeScript.
- Mengkonfigurasi _state management_ dan _layer_ API.
- Menerapkan _styling_ dasar dan sistem desain.

**Daftar Tugas:**

- **Tugas 1.1:** Buat diagram arsitektur rendering.
- **Tugas 1.2:** Inisialisasi proyek Next.js.
- **Tugas 1.3:** Konfigurasi _state management_ dan API.

**Aktivitas Utama:**

- Peserta memiliki kerangka proyek yang solid dengan rencana arsitektur performa yang jelas.

### ⚡ Fase 2: Etalase Secepat Kilat (SSG/ISR)

**Tujuan Fase:**

- Membangun halaman katalog dan detail produk menggunakan SSG (`getStaticProps`).
- Mengimplementasikan ISR (`revalidate`) untuk menjaga data produk tetap segar.
- Mengoptimalkan gambar produk secara agresif menggunakan komponen `<Image>` Next.js.
- Memastikan skor LCP dan CLS yang sangat baik untuk halaman-halaman ini.

**Daftar Tugas:**

- **Tugas 2.1:** Bangun halaman produk dengan SSG/ISR.
- **Tugas 2.2:** Terapkan optimasi gambar.
- **Tugas 2.3:** Audit performa halaman produk.

**Aktivitas Utama:**

- Peserta memiliki etalase produk yang dimuat secara instan, memberikan kesan pertama yang luar biasa.

### 🛒 Fase 3: Interaksi Instan (_Lazy Loading_ dan CSR)

**Tujuan Fase:**

- Membangun komponen keranjang belanja dan _flyout cart_ sebagai _islands_ yang di-_lazy load_.
- Menggunakan _state management_ sisi klien untuk interaksi keranjang yang instan.
- Menerapkan _memoization_ pada komponen-komponen yang sering di-_render_ ulang.
- Menggunakan _lazy loading_ untuk semua komponen non-kritis.

**Daftar Tugas:**

- **Tugas 3.1:** Bangun fungsionalitas keranjang belanja.
- **Tugas 3.2:** Terapkan _lazy loading_ pada komponen UI.
- **Tugas 3.3:** Lakukan _profiling_ dan terapkan _memoization_.

**Aktivitas Utama:**

- Peserta berhasil menciptakan pengalaman pengguna yang sangat responsif untuk interaksi-interaksi utama.

### ✈️ Fase 4: Kemampuan _Offline_ (_Service Worker_)

**Tujuan Fase:**

- Mengintegrasikan Workbox ke dalam proyek Next.js (menggunakan `next-pwa`).
- Mengkonfigurasi _precaching_ untuk _app shell_.
- Menerapkan strategi _caching runtime_ (_Stale-While-Revalidate_) untuk data produk.
- Membuat aplikasi dapat dijelajahi (_browsable_) saat offline.
- Menambahkan _Web App Manifest_ agar dapat diinstal.

**Daftar Tugas:**

- **Tugas 4.1:** Setup `next-pwa`.
- **Tugas 4.2:** Konfigurasi strategi _caching_.
- **Tugas 4.3:** Uji alur kerja offline.
- **Tugas 4.4:** Konfigurasi `manifest.json`.

**Aktivitas Utama:**

- Peserta berhasil mengubah situs e-commerce mereka menjadi PWA yang andal.

### 🛰️ Fase 5: _Monitoring_ dan _Deployment_

**Tujuan Fase:**

- Mengintegrasikan _Real User Monitoring_ (RUM) menggunakan Vercel Analytics atau `web-vitals`.
- Mengintegrasikan _error tracking_ dengan Sentry.
- Melakukan audit performa, aksesibilitas, dan SEO final dengan Lighthouse.
- Mendeploy aplikasi ke Vercel dan memantau data performa dari pengguna nyata.

**Daftar Tugas:**

- **Tugas 5.1:** Integrasikan RUM.
- **Tugas 5.2:** Integrasikan Sentry.
- **Tugas 5.3:** Lakukan audit final.
- **Tugas 5.4:** Deploy dan pantau.

**Aktivitas Utama:**

- **Proyek Akhir:** Peserta mendemonstrasikan PWA e-commerce mereka. Mereka harus bisa menunjukkan skor Lighthouse yang tinggi, fungsionalitas offline, dan menjelaskan pilihan arsitektur rendering mereka. Mereka juga harus bisa menunjukkan data awal yang masuk ke dasbor RUM dan Sentry mereka.

### 📖 Sumber Belajar Tambahan

- **Semua dokumentasi dari kursus-kursus sebelumnya.**
- **Library:**
- **Platform:**
