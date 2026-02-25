# 📘 Silabus: Mini Proyek: Secure Dashboard (PA01)

**Judul Pembelajaran: Keamanan Panel Kontrol: Melindungi Dashboard dari Eksploitasi**

Setelah mengamankan pintu masuk (Login), tantangan berikutnya adalah mengamankan area di dalamnya. Dashboard adalah tempat di mana data sensitif ditampilkan dan aksi kritis dilakukan. Proyek ini akan menguji kemampuan Anda dalam mengelola otorisasi, proteksi data saat di-render, dan mitigasi ancaman tingkat lanjut.

### 🎯 **Tujuan Proyek**

Setelah menyelesaikan proyek ini, Anda akan mampu:

1. Mengimplementasikan Role-Based Access Control (RBAC) yang ketat di sisi klien.
2. Melindungi dashboard dari serangan Clickjacking dan Open Redirect.
3. Mengelola state aplikasi yang berisi data sensitif dengan enkripsi.
4. Menerapkan Content Security Policy (CSP) khusus untuk area dashboard.

### 🛠️ **Tech Stack & Requirement**

- **Frontend:** React + TypeScript.
- **State Management:** Zustand (dengan persistency terenkripsi).
- **Security Headers:** Simulasi CSP & X-Frame-Options.
- **Routing:** React Router (Protected Routes).

### 📝 **Detail Tugas**

#### 1. Implementasi RBAC (Role-Based Access Control)

- Buat sistem routing di mana beberapa halaman hanya bisa diakses oleh `admin`, sementara yang lain bisa diakses oleh `user`.
- Gunakan Higher-Order Components (HOC) atau Wrapper Components untuk menyembunyikan elemen UI (seperti tombol "Delete") berdasarkan role.

#### 2. Pertahanan terhadap Clickjacking

- Implementasikan simulasi header `X-Frame-Options: DENY` atau `SAMEORIGIN`.
- Buat script "Frame Busting" sederhana sebagai cadangan untuk browser lama.

#### 3. State Management Terenkripsi

- Gunakan `crypto-js` untuk mengenkripsi data yang disimpan di `localStorage` (melalui middleware Zustand).
- Pastikan data sensitif pengguna tidak terlihat sebagai teks biasa di tab Application DevTools.

#### 4. Audit & Sanitasi Data Dynamic

- Pastikan semua data yang datang dari API disanitasi sebelum ditampilkan di tabel atau grafik, terutama jika data tersebut bisa mengandung input dari user lain (mencegah Stored XSS).

### 🏁 **Kriteria Kelulusan**

- [ ] Route proteksi bekerja dengan benar; user tanpa akses diarahkan ke halaman "Unauthorized".
- [ ] Tombol aksi kritis (Admin Only) tidak hanya disembunyikan secara visual, tapi juga tidak dirender jika role tidak sesuai.
- [ ] Data di localStorage tersimpan dalam format ciphertext.
- [ ] Kebijakan CSP berhasil mencegah pemuatan script dari domain luar yang tidak dikenal (simulasi).
- [ ] Implementasi pencegahan redirect ke domain luar yang tidak sah (Safe Redirect logic).

---

### 📖 **Panduan Pengerjaan**

1. Lanjutkan dari codebase proyek PI01 atau buat base dashboard baru.
2. Definisikan mock user dengan berbagai role (`admin`, `editor`, `viewer`).
3. Bangun layout dashboard dengan sidebar dan area konten utama.
4. Terapkan lapisan keamanan RBAC dan Enkripsi State.
