# 📚 Kurikulum Belajar Web Performance: Backend Focus (Node.js/TypeScript/ORM)

## 🎯 Tujuan

Kurikulum ini dirancang untuk software engineer yang mahir Node.js, TypeScript, dan ORM, untuk menguasai **web performance di backend** hingga level advanced. Fokus pada API optimization, database performance, dan scalability, dengan proyek akhir yang mengintegrasikan semua konsep. Relevan untuk backend seperti _Quantum Flow_.

## 🗂 Struktur

- **9 kursus**, 3 kategori: **Core Backend Performance (C)**, **Advanced Backend Performance (A)**, **Proyek Praktik (P)**.
- 3 tingkat: **Intermediate (I)**, **Advanced (A)**, **Expert (E)**.
- 3 periode, masing-masing 3 kursus.

## 📋 Daftar Kursus

### Core Backend Performance (C)

1. **CI01**: Pengantar Backend Performance - API response time, profiling; Node.js/TypeScript impl.
2. **CI02**: Database Optimization - Query optimization, indexing; TypeScript/TypeORM/PostgreSQL.
3. **CI03**: Caching Strategies - In-memory caching (Redis), query caching; TypeScript/Node.js.

### Advanced Backend Performance (A)

1. **AA01**: Load Balancing & Scalability - Horizontal scaling, load balancers; Node.js/TypeScript impl.
2. **AA02**: Async Processing - Message queues (RabbitMQ), async APIs; TypeScript/NestJS.
3. **AE01**: Performance Monitoring - APM (New Relic), logging; TypeScript/Node.js impl.

### Proyek Praktik (P)

1. **PI01**: Mini Proyek: Optimized API - Build fast Node.js API with TypeScript, caching.
2. **PA01**: Mini Proyek: Scalable Backend - Implement backend with Redis, RabbitMQ; TypeScript/NestJS.
3. **PE01**: Proyek Akhir: E-Commerce Backend - Build _Quantum Flow_style e-commerce backend (caching, async, APM) with Node.js/TypeScript/ORM.

## 📅 Jadwal

|Periode|Level|Kursus|Nama Kursus|Kategori|Prasyarat|
|---|---|---|---|---|---|
|**1**|Intermediate|CI01|Pengantar Backend Performance|C|-|
||Intermediate|AA01|Load Balancing & Scalability|A|-|
||Intermediate|PI01|Mini Proyek: Optimized API|P|CI01|
|**2**|Advanced|CI02|Database Optimization|C|CI01|
||Advanced|AA02|Async Processing|A|AA01|
||Advanced|PA01|Mini Proyek: Scalable Backend|P|PI01, CI02|
|**3**|Expert|CI03|Caching Strategies|C|CI02|
||Expert|AE01|Performance Monitoring|A|AA02|
||Expert|PE01|Proyek Akhir: E-Commerce Backend|P|PA01, CI03, AE01|

## 🚀 Tips Belajar

- **Waktu**: 1 periode ≈ 2-3 minggu, total ±2 bulan. Latihan 2-3 jam/minggu.
- **Praktek**:
    - **C**: Profile API (CI01), optimize queries (CI02).
    - **A**: Setup Redis caching (CI03), RabbitMQ (AA02).
    - **P**: Build optimized API (PI01), scalable backend (PA01), e-commerce backend (PE01).
- **Konteks Kerja**: Gunain Redis (CI03) untuk _Quantum Flow_ API, RabbitMQ (AA02) untuk async tasks, APM (AE01) untuk monitoring.
- **Tech**: Node.js, TypeScript, NestJS, TypeORM/Prisma, Redis, RabbitMQ, New Relic, PostgreSQL/MongoDB, VS Code.

## 🌟 What’s Next

- Deploy e-commerce backend (PE01) to AWS/Heroku.
- Dalemin: Baca _Designing Data-Intensive Applications_ (Kleppmann).
- Apply: Optimasi _Quantum Flow_ backend dengan caching and async.
- Proyek lanjutan: Tambah auto-scaling to e-commerce backend.