# 📘 Silabus: Offline Capabilities dan Manifest (CM01)

**Judul Pembelajaran: Membuat Web Terasa Native: Kemampuan Offline dan Instalasi Aplikasi**

Setelah memahami dasar _service worker_, saatnya membuat PWA Anda benar-benar terasa seperti aplikasi native. Kursus ini fokus pada dua pilar utama: membuat aplikasi **dapat diinstal** di perangkat pengguna melalui **Web App Manifest**, dan mengimplementasikan strategi _caching_ yang lebih cerdas untuk **kemampuan offline** yang andal menggunakan **Workbox**.

### 🎯 **Tujuan Utama Pembelajaran**

Setelah menyelesaikan kursus ini, Anda akan mampu:

1. **Mengimplementasikan Web App Manifest:** Membuat dan mengkonfigurasi file `manifest.json` untuk membuat PWA dapat diinstal.
2. **Menyesuaikan Pengalaman Instalasi:** Mengatur nama, ikon, warna tema, dan mode tampilan aplikasi.
3. **Mengadopsi Pola Pikir _Offline-First_:** Merancang aplikasi dengan asumsi bahwa koneksi jaringan tidak dapat diandalkan.
4. **Menyederhanakan Manajemen _Service Worker_ dengan Workbox:** Mengintegrasikan Workbox untuk mengotomatiskan _precaching_ dan strategi _caching runtime_.
5. **Menerapkan Strategi _Caching_ Dinamis:** Menggunakan strategi seperti _Stale-While-Revalidate_ dan _Network-First_ untuk data API.

### 🗺️ **Alur Pembelajaran**

Kita akan mulai dari membuat aplikasi kita "terlihat" seperti aplikasi (Manifest), lalu mengubah cara berpikir kita (Offline-First), dan terakhir menggunakan alat canggih (Workbox) untuk mewujudkannya.

```
graph TD
    A[Modul 1: Membuat Aplikasi Dapat Diinstal (Manifest)] --> B[Modul 2: Paradigma Offline-First];
    B --> C[Modul 3: Pengantar Workbox: Service Worker Cerdas];
    C --> D[Modul 4: Strategi Caching Runtime dengan Workbox];
    D --> E[Proyek: PWA yang Dapat Diinstal & Bekerja Offline];

    subgraph "Instalasi & Paradigma"
        A
        B
    end

    subgraph "Implementasi Cerdas"
        C
        D
    end

    subgraph "Aplikasi"
        E
    end

```

### 📚 **Modul Pembelajaran**

Berikut adalah rincian materi dari setiap modul.

### **📲 Modul 1: Membuat Aplikasi Dapat Diinstal (_Web App Manifest_)**

**Tujuan Modul:**

- Membuat file `manifest.json`.
- Mendefinisikan properti kunci: `name`, `short_name`, `icons`, `start_url`, `display`, `theme_color`.
- Menghubungkan _manifest_ ke aplikasi melalui tag `<link>` di HTML.
- Menguji kriteria instalasi menggunakan Lighthouse.

**Daftar Lesson:**

- **Lesson 1.1:** Pengantar _Web App Manifest_.
- **Lesson 1.2:** Properti-Properti Kunci dalam _Manifest_.
- **Lesson 1.3:** Menghubungkan dan Memverifikasi _Manifest_.
- **Lesson 1.4:** Kustomisasi Tampilan dan _Splash Screen_.

**Aktivitas Utama Modul:**

- ✍️ **Latihan:** Peserta membuat file `manifest.json` untuk aplikasi mereka, menyiapkan beberapa ukuran ikon, dan memastikan Lighthouse mendeteksinya sebagai PWA yang dapat diinstal.

### **✈️ Modul 2: Paradigma _Offline-First_**

**Tujuan Modul:**

- Membedakan antara desain _online-first_ dan _offline-first_.
- Memahami pentingnya memberikan umpan balik instan kepada pengguna dari _cache_.
- Merancang UI yang dapat menangani status offline dengan anggun.
- Memisahkan _shell_ aplikasi (statis) dari konten dinamis.

**Daftar Lesson:**

- **Lesson 2.1:** Mengubah Pola Pikir Anda.
- **Lesson 2.2:** Arsitektur _App Shell_.
- **Lesson 2.3:** Merancang untuk Kondisi Offline.
- **Lesson 2.4:** Memberi Tahu Pengguna Saat Mereka Offline.

**Aktivitas Utama Modul:**

- 🗣️ **Diskusi Desain:** Peserta mendiskusikan bagaimana aplikasi seperti Google Maps atau aplikasi berita dapat didesain ulang dengan pendekatan _offline-first_.

### **📦 Modul 3: Pengantar Workbox: _Service Worker_ Cerdas**

**Tujuan Modul:**

- Memahami masalah dari menulis _service worker_ manual.
- Mengintegrasikan Workbox ke dalam proses _build_ aplikasi React (misalnya, melalui `cra-template-pwa`).
- Menggunakan Workbox untuk melakukan _precaching_ semua aset statis secara otomatis.
- Memahami bagaimana Workbox menyederhanakan logika _service worker_.

**Daftar Lesson:**

- **Lesson 3.1:** Kenapa Perlu Workbox?
- **Lesson 3.2:** Integrasi Workbox ke dalam Proyek React.
- **Lesson 3.3:** _Precaching_ Otomatis.
- **Lesson 3.4:** Membedah File _Service Worker_ yang Dihasilkan.

**Aktivitas Utama Modul:**

- 💻 **Latihan:** Peserta mengkonfigurasi Workbox untuk secara otomatis melakukan _precaching_ semua aset yang dihasilkan oleh proses _build_ React.

### **🔄 Modul 4: Strategi _Caching Runtime_ dengan Workbox**

**Tujuan Modul:**

- Mengkonfigurasi _runtime caching_ untuk menangani permintaan yang tidak di-_precache_.
- Menerapkan strategi _Stale-While-Revalidate_ untuk data API yang sering berubah.
- Menerapkan strategi _Cache-First_ untuk aset yang jarang berubah (misalnya, gambar avatar).
- Menerapkan strategi _Network-First_ untuk data yang harus selalu paling baru.

**Daftar Lesson:**

- **Lesson 4.1:** Menangani Permintaan Dinamis.
- **Lesson 4.2:** Strategi _Stale-While-Revalidate_.
- **Lesson 4.3:** Strategi _Cache-First_ untuk Aset.
- **Lesson 4.4:** Strategi _Network-First_ untuk Data Kritis.

**Aktivitas Utama Modul:**

- 🚀 **Proyek: PWA yang Dapat Diinstal dan Bekerja Offline:** Peserta mengambil aplikasi berita dari kursus sebelumnya. Mereka harus: (1) Menambahkan _Web App Manifest_ agar dapat diinstal. (2) Menggunakan Workbox untuk _precaching app shell_. (3) Menerapkan strategi _Stale-While-Revalidate_ untuk API berita, sehingga daftar berita terakhir masih bisa dilihat saat offline.

### 📖 **Sumber Belajar Tambahan**

- **Dokumentasi:**
    - [Workbox Documentation](https://developer.chrome.com/docs/workbox/)
    - [MDN Web Docs - Web App Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest)
- **Tools:**
    - Lighthouse di Chrome DevTools.