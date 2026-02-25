# 📘 Silabus: Proyek Akhir: Secure E-Commerce Frontend (PE01)

**Judul Pembelajaran: Benteng Perdagangan Digital: Membangun Frontend E-Commerce Standar Industri**

Ini adalah puncak dari kurikulum Frontend Security. Anda akan membangun sebuah aplikasi E-Commerce skala penuh (frontend-only dengan mock API) yang menerapkan **seluruh** lapisan keamanan yang telah dipelajari. Fokus utama adalah pada keamanan transaksi, proteksi data pembayaran, dan integritas aplikasi secara keseluruhan.

### 🎯 **Tujuan Proyek**

Setelah menyelesaikan proyek ini, Anda akan mampu:

1. Membangun SPA (Single Page Application) yang "Security-First" dari arsitektur awal.
2. Mengamankan alur checkout dan transaksi tanpa menyimpan data sensitif secara tidak aman.
3. Mengimplementasikan Subresource Integrity (SRI) untuk menjamin kode pihak ketiga tidak dimodifikasi.
4. Melakukan "Security Audit" mandiri terhadap aplikasi yang dibangun.

### 🛠️ **Tech Stack & Requirement**

- **Framework:** Next.js (App Router) atau Vite + React.
- **Security:** Web Crypto API, DOMPurify, Zod.
- **Tooling:** Snyk (Audit), Bundle Analyzer.
- **Integrasi:** Mock Payment Gateway (Stripe-like flow).

### 📝 **Detail Tugas**

#### 1. Arsitektur Pertahanan Berlapis

- Implementasikan CSP yang sangat ketat melalui `next.config.js` atau Meta tags.
- Gunakan `Strict-Transport-Security` (HSTS) dalam simulasi.
- Terapkan SRI (Subresource Integrity) pada script external (seperti link Google Fonts atau library CDN).

#### 2. Secure Checkout Flow

- Pastikan data kartu kredit (mock) diproses melalui "Tokenization" (simulasi).
- Hindari menyimpan data sensitif di state global setelah checkout selesai.
- Implementasikan "Idempotency Key" di frontend untuk mencegah double-payment akibat klik berulang.

#### 3. Integritas Aplikasi & Dependency

- Lakukan audit dependency menggunakan `npm audit` atau Snyk.
- Pastikan tidak ada "Prototype Pollution" pada fungsi utility buatan sendiri.
- Gunakan `Object.freeze()` pada konfigurasi konstanta aplikasi agar tidak bisa dimutasi di runtime.

#### 4. Monitoring & Error Reporting

- Implementasikan sistem logging error keamanan (simulasi pengiriman log ke endpoint `/api/security-logs`).
- Berikan feedback yang user-friendly tapi tidak membocorkan detail teknis saat terjadi error sistem.

### 🏁 **Kriteria Kelulusan**

- [ ] Aplikasi lulus audit dasar menggunakan tool automated scan (seperti Lighthouse Security section atau ZAP).
- [ ] Tidak ada penggunaan `eval()` atau `inline scripts` tanpa nonce/hash.
- [ ] Alur login, dashboard, dan checkout terintegrasi secara aman.
- [ ] Dokumentasi menyertakan "Security Manifest" yang menjelaskan fitur keamanan apa saja yang diimplementasikan.
- [ ] Kode terorganisir mengikuti Clean Architecture (UI, Logic, Data layers).

---

### 📖 **Panduan Pengerjaan**

1. Inisialisasi proyek baru yang bersih.
2. Rancang struktur folder yang memisahkan antara `components`, `hooks`, `services`, dan `utils`.
3. Gunakan `list-courses.md` sebagai checklist untuk memastikan setiap poin keamanan (dari CI01 hingga AE01) sudah tercover.
4. Buatlah Video Demo atau Write-up teknis yang menjelaskan bagaimana aplikasi Anda bertahan dari serangan umum.
