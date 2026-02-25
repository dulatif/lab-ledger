# 📘 Silabus: Secure Authentication (AA01)

**Judul Pembelajaran: Gerbang Benteng Digital: Mengimplementasikan Autentikasi yang Aman di React**

Autentikasi adalah gerbang utama ke aplikasi Anda. Jika gerbang ini lemah, seluruh benteng akan runtuh. Kursus tingkat lanjut ini fokus pada implementasi alur kerja autentikasi yang aman dan modern di sisi klien. Kita akan membahas penyimpanan **JWT** yang aman, alur kerja **OAuth 2.0**, dan cara memvalidasi sesi pengguna dengan andal di aplikasi React.

### 🎯 **Tujuan Utama Pembelajaran**

Setelah menyelesaikan kursus ini, Anda akan mampu:

1. **Mengelola JWT dengan Aman di Klien:** Memahami _trade-off_ antara berbagai metode penyimpanan token (misalnya, `localStorage` vs. _cookies_) dan menerapkan solusi yang lebih aman.
2. **Mengimplementasikan Alur _Refresh Token_:** Membangun logika untuk secara otomatis memperbarui _access token_ yang berumur pendek menggunakan _refresh token_ yang berumur panjang.
3. **Mengintegrasikan Alur OAuth 2.0:** Mengimplementasikan alur login sosial (misalnya, "Login dengan Google") di aplikasi React.
4. **Melakukan Validasi Sesi secara Efektif:** Menggunakan _interceptor_ API untuk menangani token yang kedaluwarsa dan mengelola status sesi secara global.
5. **Membangun Komponen UI Autentikasi yang Kuat:** Membuat komponen _higher-order_ atau _hooks_ kustom untuk melindungi rute dan komponen.

### 🗺️ **Alur Pembelajaran**

Kita akan mulai dari cara menyimpan "kunci" (token) dengan aman, lalu belajar cara memperbaruinya, mengintegrasikan dengan penyedia pihak ketiga, dan terakhir, membangun sistem yang kuat di sekitar alur ini.

```
graph TD
    A[Modul 1: Penyimpanan Token yang Aman] --> B[Modul 2: Alur Refresh Token];
    B --> C[Modul 3: Login Sosial dengan OAuth 2.0];
    C --> D[Modul 4: Manajemen Sesi & Penanganan Token Kedaluwarsa];
    D --> E[Modul 5: Proteksi Rute & Komponen];
    E --> F[Proyek: Membangun Alur Autentikasi Lengkap];

    subgraph "Manajemen Token"
        A
        B
    end

    subgraph "Integrasi & Sesi"
        C
        D
    end

    subgraph "Aplikasi & Proteksi"
        E
        F
    end

```

### 📚 **Modul Pembelajaran**

Berikut adalah rincian materi dari setiap modul.

### **🔑 Modul 1: Penyimpanan Token yang Aman**

**Tujuan Modul:**

- Menganalisis kerentanan `localStorage` terhadap XSS.
- Memahami _HttpOnly cookies_ sebagai alternatif yang lebih aman untuk menyimpan token.
- Membahas strategi hibrida (menyimpan status login di `localStorage`, token di _cookie_).
- Mengkonfigurasi _backend_ untuk mengatur _HttpOnly cookies_.

**Daftar Lesson:**

- **Lesson 1.1:** `localStorage` vs. _Cookies_: Pertarungan Keamanan.
- **Lesson 1.2:** Keunggulan _HttpOnly Cookies_.
- **Lesson 1.3:** Strategi Penyimpanan Hibrida.
- **Lesson 1.4:** Konfigurasi Sisi Server untuk _Cookies_.

**Aktivitas Utama Modul:**

- 🗣️ **Diskusi:** Peserta berdebat tentang pro dan kontra dari setiap metode penyimpanan token.

### **🔄 Modul 2: Alur _Refresh Token_**

**Tujuan Modul:**

- Memahami arsitektur _access token_ (umur pendek) dan _refresh token_ (umur panjang).
- Membuat _endpoint_ API di _backend_ untuk me-_refresh_ token.
- Menggunakan _interceptor_ Axios atau _middleware_ API untuk secara otomatis menangani respons 401 (Unauthorized).
- Mengimplementasikan logika untuk mencoba kembali permintaan yang gagal dengan token baru.

**Daftar Lesson:**

- **Lesson 2.1:** Arsitektur _Access_ dan _Refresh Token_.
- **Lesson 2.2:** _Endpoint Refresh Token_ di _Backend_.
- **Lesson 2.3:** Menangani Token Kedaluwarsa dengan _Interceptor_.
- **Lesson 2.4:** Alur Kerja _Refresh_ Otomatis.

**Aktivitas Utama Modul:**

- 💻 **Latihan:** Peserta mengimplementasikan sebuah _interceptor_ Axios yang dapat menangani respons 401, memanggil _endpoint refresh_, dan mencoba kembali permintaan asli.

### **🌐 Modul 3: Login Sosial dengan OAuth 2.0**

**Tujuan Modul:**

- Memahami alur _Authorization Code_ OAuth 2.0.
- Mengintegrasikan _library_ klien untuk login sosial (misalnya, `@react-oauth/google`).
- Menangani _callback_ dari penyedia OAuth.
- Mengirim kode otorisasi ke _backend_ untuk ditukar dengan token JWT aplikasi Anda.

**Daftar Lesson:**

- **Lesson 3.1:** Pengantar OAuth 2.0.
- **Lesson 3.2:** Mengintegrasikan Klien Login Google/GitHub.
- **Lesson 3.3:** Menangani _Callback_ di _Frontend_.
- **Lesson 3.4:** Verifikasi di _Backend_ dan Penerbitan Token.

**Aktivitas Utama Modul:**

- 🌐 **Latihan:** Peserta mengintegrasikan tombol "Login with Google" ke halaman login mereka dan berhasil mendapatkan profil pengguna setelah login.

### **🚦 Modul 4: Manajemen Sesi dan Penanganan Token Kedaluwarsa**

**Tujuan Modul:**

- Menggunakan _state management_ (Redux/Zustand) untuk melacak status autentikasi secara global.
- Memvalidasi token saat aplikasi pertama kali dimuat.
- Mengimplementasikan logika _logout_ yang membersihkan _state_ dan token.
- Menangani kasus di mana _refresh token_ juga kedaluwarsa (memaksa _logout_).

**Daftar Lesson:**

- **Lesson 4.1:** _State_ Sesi Global.
- **Lesson 4.2:** Validasi Sesi Saat Aplikasi Dimuat.
- **Lesson 4.3:** Alur Kerja _Logout_ yang Bersih.
- **Lesson 4.4:** Menangani Kegagalan _Refresh_.

**Aktivitas Utama Modul:**

- 🚦 **Latihan:** Peserta mengintegrasikan logika autentikasi mereka dengan _store_ Redux/Zustand, memastikan UI diperbarui secara reaktif terhadap perubahan status login/logout.

### **🛡️ Modul 5: Proteksi Rute dan Komponen**

**Tujuan Modul:**

- Membuat komponen `<ProtectedRoute>` atau _Higher-Order Component_ (HOC) untuk melindungi rute.
- Menggunakan React Router untuk mengalihkan pengguna yang tidak sah.
- Membuat _hook_ kustom `useAuth()` untuk mengakses status autentikasi dengan mudah.
- Menampilkan atau menyembunyikan elemen UI secara kondisional berdasarkan peran atau status login.

**Daftar Lesson:**

- **Lesson 5.1:** Melindungi Rute di Sisi Klien.
- **Lesson 5.2:** Implementasi `<ProtectedRoute>`.
- **Lesson 5.3:** _Hook_ Kustom `useAuth()`.
- **Lesson 5.4:** UI yang Sadar-Otorisasi.

**Aktivitas Utama Modul:**

- 🚀 **Proyek: Membangun Alur Autentikasi Lengkap:** Peserta membangun sebuah aplikasi React kecil dengan halaman login, halaman publik, dan halaman dasbor yang dilindungi. Aplikasi ini harus mengimplementasikan alur _refresh token_ yang aman dan (opsional) login sosial dengan Google.

### 📖 **Sumber Belajar Tambahan**

- **Dokumentasi:**
    - [OWASP JWT Cheat Sheet](https://www.google.com/search?q=https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_Cheat_Sheet_for_Java.html)
    - [OAuth 2.0 Simplified](https://oauth.net/2/)
- **Library:**
    - `@react-oauth/google`, `axios`.
