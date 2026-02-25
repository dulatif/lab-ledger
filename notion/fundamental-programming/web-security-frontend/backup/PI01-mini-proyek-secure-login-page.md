# 📘 Silabus: Mini Proyek: Secure Login Page (PI01)

**Judul Pembelajaran: Gerbang yang Kokoh: Membangun Halaman Login Terproteksi**

Proyek praktik pertama ini berfokus pada implementasi teknik keamanan fundamental pada fitur yang paling kritis dalam setiap aplikasi: **Autentikasi**. Anda akan membangun sebuah halaman login dari nol yang kebal terhadap serangan umum.

### 🎯 **Tujuan Proyek**

Setelah menyelesaikan proyek ini, Anda akan mampu:

1. Mengamankan transmisi data kredensial dari frontend ke backend.
2. Mengimplementasikan proteksi terhadap serangan brute-force di sisi klien (rate limiting/CAPTCHA).
3. Mencegah kebocoran informasi melalui pesan error (User Enumeration).
4. Mengelola state login secara aman di browser.

### 🛠️ **Tech Stack & Requirement**

- **Frontend:** React + TypeScript.
- **Validation:** Zod.
- **State:** React Context / Zustand.
- **Proteksi:** ReCAPTCHA v3 (Mock/Real).
- **Communication:** Axios dengan interceptors.

### 📝 **Detail Tugas**

#### 1. Validasi Input yang Ketat

- Pastikan input email memiliki format yang benar.
- Password harus memenuhi syarat kompleksitas minimal (tidak boleh hanya teks biasa).
- Gunakan Zod untuk validasi skema sebelum data dikirim.

#### 2. Proteksi Brute Force & Bot

- Implementasikan integrasi (simulasi) ReCAPTCHA.
- Berikan feedback "Account Locked" atau "Too Many Attempts" dengan benar tanpa membocorkan apakah user tersebut eksis atau tidak.

#### 3. Handling Response yang Aman

- Berikan pesan error yang ambigu (misal: "Email atau Password salah", bukan "Password salah untuk email ini").
- Tangani status 401 dan 403 secara khusus menggunakan Axios interceptors.

#### 4. Penyimpanan Kredensial

- Simpan access token hanya di memori.
- Simpan refresh token di HttpOnly Cookie (praktekkan cara pengirimannya).

### 🏁 **Kriteria Kelulusan**

- [ ] Tidak ada penggunaan `any` dalam kode TypeScript.
- [ ] Validasi form berjalan sebelum tombol "Submit" mengirimkan request.
- [ ] Pesan error login tidak deskriptif (mencegah enumerasi user).
- [ ] Implementasi interceptor untuk menangani token kadaluarsa.
- [ ] Kode bersih dari console.log rahasia.

---

### 📖 **Panduan Pengerjaan**

1. Setup proyek dengan `Vite` + `TypeScript`.
2. Buat UI Login yang bersih dan responsif.
3. Hubungkan dengan API (Mock menggunakan `msw` atau tool serupa).
4. Terapkan lapisan keamanan satu per satu.
