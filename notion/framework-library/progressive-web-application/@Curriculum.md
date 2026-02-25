# 📚 Kurikulum Belajar Progressive Web Apps (PWA) dengan React dan TypeScript

## 🎯 Tujuan

Kurikulum ini dirancang untuk software engineer yang mahir TypeScript, Node.js, React, dan backend (ORM), untuk menguasai **Progressive Web Apps (PWA)** hingga level advanced. Fokus pada service workers, offline capabilities, dan PWA optimization, menggunakan React dan TypeScript, dengan proyek akhir yang mengintegrasikan semua konsep. Relevan untuk pengembangan aplikasi modern seperti _Quantum Flow_ di Indonesia.

## 🗂 Struktur

- **9 kursus**, 3 kategori: **Core PWA (C)**, **Advanced PWA (A)**, **Proyek Praktik (P)**.
- 3 tingkat: **Dasar (D)**, **Menengah (M)**, **Tinggi (T)**.
- 3 periode, masing-masing 3 kursus.

## 📋 Daftar Kursus

### Core PWA (C)

1. **CD01**: Pengantar PWA & Service Workers - PWA basics, service worker lifecycle, caching strategies; React/TypeScript setup.
2. **CM01**: Offline Capabilities & Manifest - Web app manifest, offline-first, TypeScript integration with Workbox.
3. **CT01**: Push Notifications & Background Sync - Push API, background sync; TypeScript/React impl.

### Advanced PWA (A)

1. **AD01**: Advanced Service Workers - Custom caching, fetch interception; TypeScript/Workbox impl.
2. **AM01**: Performance Optimization - Lazy loading, code splitting, Lighthouse audits; React/TypeScript.
3. **AT01**: PWA Architecture & Scalability - Modular PWA design, state management; TypeScript/React patterns.

### Proyek Praktik (P)

1. **PD01**: Mini Proyek: Offline Task Manager - Build offline-capable task manager with React, TypeScript, service workers.
2. **PM01**: Mini Proyek: Notification Dashboard - Implement dashboard with push notifications, React/TypeScript.
3. **PT01**: Proyek Akhir: E-Commerce PWA - Build _Quantum Flow_style e-commerce PWA (offline, push, caching) with React and TypeScript.

## 📅 Jadwal

|Periode|Level|Kursus|Nama Kursus|Kategori|Prasyarat|
|---|---|---|---|---|---|
|**1**|Dasar|CD01|Pengantar PWA & Service Workers|C|-|
||Dasar|AD01|Advanced Service Workers|A|-|
||Dasar|PD01|Mini Proyek: Offline Task Manager|P|CD01|
|**2**|Menengah|CM01|Offline Capabilities & Manifest|C|CD01|
||Menengah|AM01|Performance Optimization|A|AD01|
||Menengah|PM01|Mini Proyek: Notification Dashboard|P|PD01, CM01|
|**3**|Tinggi|CT01|Push Notifications & Background Sync|C|CM01|
||Tinggi|AT01|PWA Architecture & Scalability|A|AM01|
||Tinggi|PT01|Proyek Akhir: E-Commerce PWA|P|PM01, CT01, AT01|

## 🚀 Tips Belajar

- **Waktu**: 1 periode ≈ 2-3 minggu, total ±2 bulan. Latihan 2-3 jam/minggu.
- **Praktek**:
    - **C**: Setup service worker (CD01), web manifest (CM01), push notifications (CT01).
    - **A**: Implement custom caching (AD01), Lighthouse optimization (AM01).
    - **P**: Build task manager (PD01), notification dashboard (PM01), e-commerce PWA (PT01).
- **Konteks Kerja**: Gunain service workers (CD01) untuk offline _Quantum Flow_, push notifications (CT01) untuk engagement, modular design (AT01) untuk skalabilitas.
- **Tech**: React, TypeScript, Workbox, Vite, Lighthouse, Node.js, Firebase (push), ESLint, VS Code.

## 🌟 What’s Next

- Deploy e-commerce PWA (PT01) ke Vercel/Netlify.
- Dalemin: Baca _Building Progressive Web Apps_ (Tal Ater).
- Apply: Integrasi PWA ke _Quantum Flow_ untuk offline support.
- Proyek lanjutan: Tambah i18n atau background sync ke e-commerce PWA.