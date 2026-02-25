# 📚 Kurikulum Belajar Web Performance: Frontend Focus (React/TypeScript)

## 🎯 Tujuan

Kurikulum ini dirancang untuk software engineer yang mahir TypeScript dan React, untuk menguasai **web performance di frontend** hingga level advanced. Fokus pada optimasi rendering, asset delivery, dan user experience, menggunakan React dan TypeScript, dengan proyek akhir yang mengintegrasikan semua konsep. Relevan untuk aplikasi seperti _Quantum Flow_.

## 🗂 Struktur

- **9 kursus**, 3 kategori: **Core Frontend Performance (C)**, **Advanced Frontend Performance (A)**, **Proyek Praktik (P)**.
- 3 tingkat: **Intermediate (I)**, **Advanced (A)**, **Expert (E)**.
- 3 periode, masing-masing 3 kursus.

## 📋 Daftar Kursus

### Core Frontend Performance (C)

1. **CI01**: Pengantar Frontend Performance - Lighthouse audits, performance metrics (FCP, LCP); React/TypeScript impl.
2. **CI02**: Rendering Optimization - Lazy loading, memoization, React hooks; TypeScript impl.
3. **CI03**: Asset Optimization - Image compression, code splitting; React/TypeScript with Vite.

### Advanced Frontend Performance (A)

1. **AA01**: Advanced Rendering Techniques - Virtualization, server-side rendering (SSR); Next.js/TypeScript impl.
2. **AA02**: Network Optimization - CDN, prefetching, service workers; React/TypeScript/Workbox.
3. **AE01**: Performance Monitoring - Real User Monitoring (RUM), error tracking; TypeScript/React tools.

### Proyek Praktik (P)

1. **PI01**: Mini Proyek: Optimized Landing Page - Build fast-loading landing page with React, TypeScript, lazy loading.
2. **PA01**: Mini Proyek: Performant Dashboard - Implement dashboard with SSR, code splitting; React/TypeScript.
3. **PE01**: Proyek Akhir: E-Commerce PWA - Build _Quantum Flow_style e-commerce PWA (SSR, lazy loading, RUM) with React/TypeScript.

## 📅 Jadwal

|Periode|Level|Kursus|Nama Kursus|Kategori|Prasyarat|
|---|---|---|---|---|---|
|**1**|Intermediate|CI01|Pengantar Frontend Performance|C|-|
||Intermediate|AA01|Advanced Rendering Techniques|A|-|
||Intermediate|PI01|Mini Proyek: Optimized Landing Page|P|CI01|
|**2**|Advanced|CI02|Rendering Optimization|C|CI01|
||Advanced|AA02|Network Optimization|A|AA01|
||Advanced|PA01|Mini Proyek: Performant Dashboard|P|PI01, CI02|
|**3**|Expert|CI03|Asset Optimization|C|CI02|
||Expert|AE01|Performance Monitoring|A|AA02|
||Expert|PE01|Proyek Akhir: E-Commerce PWA|P|PA01, CI03, AE01|

## 🚀 Tips Belajar

- **Waktu**: 1 periode ≈ 2-3 minggu, total ±2 bulan. Latihan 2-3 jam/minggu.
- **Praktek**:
    - **C**: Run Lighthouse audits (CI01), implement lazy loading (CI02).
    - **A**: Setup SSR with Next.js (AA01), service workers (AA02).
    - **P**: Build landing page (PI01), dashboard (PA01), e-commerce PWA (PE01).
- **Konteks Kerja**: Gunain lazy loading (CI02) untuk _Quantum Flow_ UI, SSR (AA01) untuk SEO, RUM (AE01) untuk analytics.
- **Tech**: React, TypeScript, Next.js, Workbox, Vite, Lighthouse, Sentry, ESLint, VS Code.

## 🌟 What’s Next

- Deploy e-commerce PWA (PE01) ke Vercel/Netlify.
- Dalemin: Baca _High Performance Browser Networking_ (Ilya Grigorik).
- Apply: Optimasi _Quantum Flow_ frontend dengan SSR dan RUM.
- Proyek lanjutan: Tambah Web Vitals tracking ke e-commerce PWA.