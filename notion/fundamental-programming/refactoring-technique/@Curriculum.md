# 📚 Kurikulum Belajar Refactoring dengan TypeScript

## 🎯 Tujuan

Kurikulum ini dirancang untuk software engineer yang mahir TypeScript, Node.js, React, dan fundamental programming (backend, ORM), untuk menguasai **refactoring techniques**, **code smells**, dan praktik clean code hingga level intermediate-advanced. Fokus pada identifikasi code smells, teknik refactoring (extract method, replace conditional, dll.), dan pengukuran kualitas kode, dengan proyek akhir yang mengintegrasikan semua konsep. Kurikulum relevan untuk optimasi kode di proyek seperti _Quantum Flow_ di Indonesia.

## 🗂 Struktur

- **9 kursus**, 3 kategori: **Code Smells & Principles (C)**, **Refactoring Techniques (R)**, **Proyek Praktik (P)**.
- 3 tingkat: **Dasar (D)**, **Menengah (M)**, **Tinggi (T)**.
- 3 periode, masing-masing 3 kursus.

## 📋 Daftar Kursus

### Code Smells & Principles (C)

1. **CD01**: Pengantar Code Smells & Clean Code - Identifikasi code smells (long method, duplicated code), SOLID principles, TypeScript impl.
2. **CM01**: Advanced Code Smells - Complex smells (large class, god object), DRY, dan TypeScript type safety.
3. **CT01**: Code Metrics & Testing - Code quality metrics (cyclomatic complexity), unit testing (Jest) untuk refactoring.

### Refactoring Techniques (R)

1. **RD01**: Basic Refactoring Techniques - Extract method, inline variable, rename; TypeScript impl. untuk readability.
2. **RM01**: Structural Refactoring - Extract class, move method, replace conditional with polymorphism; TypeScript patterns.
3. **RT01**: Advanced Refactoring - Replace inheritance with delegation, introduce parameter object; TypeScript generics.

### Proyek Praktik (P)

1. **PD01**: Mini Proyek: Refactor Legacy API - Refactor Node.js API dengan code smells (duplicated code) pake TypeScript.
2. **PM01**: Mini Proyek: Refactor Frontend Module - Refactor React module (large component) pake extract method dan SOLID.
3. **PT01**: Proyek Akhir: Refactor Task Management App - Refactor full-stack app (_Quantum Flow_style) dengan semua teknik refactoring.

## 📅 Jadwal

|Periode|Level|Kursus|Nama Kursus|Kategori|Prasyarat|
|---|---|---|---|---|---|
|**1**|Dasar|CD01|Pengantar Code Smells & Clean Code|C|-|
||Dasar|RD01|Basic Refactoring Techniques|R|-|
||Dasar|PD01|Mini Proyek: Refactor Legacy API|P|CD01, RD01|
|**2**|Menengah|CM01|Advanced Code Smells|C|CD01|
||Menengah|RM01|Structural Refactoring|R|RD01|
||Menengah|PM01|Mini Proyek: Refactor Frontend Module|P|PD01, RM01|
|**3**|Tinggi|CT01|Code Metrics & Testing|C|CM01|
||Tinggi|RT01|Advanced Refactoring|R|RM01|
||Tinggi|PT01|Proyek Akhir: Refactor Task Management App|P|PM01, CT01, RT01|

## 🚀 Tips Belajar

- **Waktu**: 1 periode ≈ 2-3 minggu, total ±2 bulan. Latihan 2-3 jam/minggu.
- **Praktek**:
    - **C**: Identifikasi code smells di proyek lama (CD01), hitung cyclomatic complexity (CT01).
    - **R**: Terapin extract method (RD01), replace conditional (RM01), delegation (RT01) pake TypeScript.
    - **P**: Refactor API (PD01), React module (PM01), full-stack app (PT01).
- **Konteks Kerja**: Gunain extract method (RD01) untuk bersihin kode _Quantum Flow_, SOLID (CM01) untuk modularitas, unit testing (CT01) untuk stability.
- **Tech**: TypeScript, Node.js, React, Jest, ESLint, Prettier, VS Code

## 🌟 What’s Next

- Apply refactoring di proyek nyata: Optimasi _Quantum Flow_ atau app baru.
- Dalemin: Baca _Refactoring_ (Martin Fowler), pelajari tools (SonarQube, ESLint).
- Latihan: Refactor kode lama pake teknik dari RM01/RT01.
- Proyek lanjutan: Tambah fitur (auth, API) ke task management app post-refactoring.