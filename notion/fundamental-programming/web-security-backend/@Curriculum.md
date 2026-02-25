# 📚 Kurikulum Belajar Web Security: Backend Focus (Node.js/TypeScript/ORM)

## 🎯 Tujuan

Kurikulum ini dirancang untuk software engineer yang mahir Node.js, TypeScript, dan ORM, untuk menguasai **web security di backend** hingga level advanced. Fokus pada mitigasi ancaman seperti SQL injection, API abuse, dan server-side vulnerabilities, dengan proyek akhir yang mengintegrasikan semua konsep. Relevan untuk backend seperti _Quantum Flow_.

## 🗂 Struktur

- **9 kursus**, 3 kategori: **Core Backend Security (C)**, **Advanced Backend Security (A)**, **Proyek Praktik (P)**.
- 3 tingkat: **Intermediate (I)**, **Advanced (A)**, **Expert (E)**.
- 3 periode, masing-masing 3 kursus.

## 📋 Daftar Kursus

### Core Backend Security (C)

1. **CI01**: Pengantar Backend Security - SQL injection prevention, input sanitization; Node.js/TypeScript/ORM.
2. **CI02**: Authentication & Authorization - JWT, OAuth2, role-based access; TypeScript/NestJS impl.
3. **CI03**: Secure API Design - Rate limiting, CORS, API key; TypeScript/Node.js.

### Advanced Backend Security (A)

1. **AA01**: Data Protection & Encryption - Database encryption, secure password hashing; TypeScript/ORM.
2. **AA02**: Secure Middleware & Logging - Secure Express middleware, audit logging; TypeScript/NestJS.
3. **AE01**: Advanced Threat Mitigation - Dependency vulnerabilities, server hardening; TypeScript/Node.js.

### Proyek Praktik (P)

1. **PI01**: Mini Proyek: Secure API Endpoint - Build secure Node.js API with TypeScript/ORM, anti-injection.
2. **PA01**: Mini Proyek: Authenticated Backend - Implement backend with JWT, rate limiting; TypeScript/NestJS.
3. **PE01**: Proyek Akhir: Secure E-Commerce Backend - Build _Quantum Flow_style e-commerce backend (auth, encryption, hardening) with Node.js/TypeScript/ORM.

## 📅 Jadwal

|Periode|Level|Kursus|Nama Kursus|Kategori|Prasyarat|
|---|---|---|---|---|---|
|**1**|Intermediate|CI01|Pengantar Backend Security|C|-|
||Intermediate|AA01|Data Protection & Encryption|A|-|
||Intermediate|PI01|Mini Proyek: Secure API Endpoint|P|CI01|
|**2**|Advanced|CI02|Authentication & Authorization|C|CI01|
||Advanced|AA02|Secure Middleware & Logging|A|AA01|
||Advanced|PA01|Mini Proyek: Authenticated Backend|P|PI01, CI02|
|**3**|Expert|CI03|Secure API Design|C|CI02|
||Expert|AE01|Advanced Threat Mitigation|A|AA02|
||Expert|PE01|Proyek Akhir: Secure E-Commerce Backend|P|PA01, CI03, AE01|

## 🚀 Tips Belajar

- **Waktu**: 1 periode ≈ 2-3 minggu, total ±2 bulan. Latihan 2-3 jam/minggu.
- **Praktek**:
    - **C**: Implement anti-injection (CI01), JWT auth (CI02).
    - **A**: Setup encryption (AA01), audit logging (AA02).
    - **P**: Build secure API (PI01), authenticated backend (PA01), e-commerce backend (PE01).
- **Konteks Kerja**: Gunain JWT (CI02) untuk _Quantum Flow_ auth, encryption (AA01) untuk data safety, hardening (AE01) untuk server protection.
- **Tech**: Node.js, TypeScript, NestJS, TypeORM/Prisma, Express, PostgreSQL/MongoDB, VS Code.

## 🌟 What’s Next

- Deploy e-commerce backend (PE01) ke AWS/Heroku.
- Dalemin: Baca OWASP API Security Top 10.
- Apply: Integrasi security ke _Quantum Flow_ backend.
- Proyek lanjutan: Tambah WAF (e.g., Cloudflare) ke e-commerce backend.