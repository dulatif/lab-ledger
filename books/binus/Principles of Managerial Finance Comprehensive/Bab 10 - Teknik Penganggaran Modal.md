Berikut adalah rangkuman Bab 10 dengan format _Notion-style_ yang terstruktur, _to the point_, dan mudah dipahami:

# 📘 Bab 10: Teknik Penganggaran Modal (_Capital Budgeting Techniques_)

## 🎯 1. Gambaran Umum Penganggaran Modal (_Overview of Capital Budgeting_)

- **Apa itu Penganggaran Modal?** Proses mengevaluasi dan memilih investasi jangka panjang (misalnya: membangun pabrik baru, membeli mesin, peluncuran produk baru) yang bernilai lebih besar dari biayanya sehingga dapat menciptakan kekayaan bagi investor.
- **Proses Penganggaran Modal (5 Langkah):**
    1. Pembuatan proposal.
    2. Tinjauan dan analisis.
    3. Pengambilan keputusan.
    4. Implementasi.
    5. Tindak lanjut (_Follow-up_) untuk membandingkan hasil aktual dengan proyeksi.
- **Terminologi Penting:**
    - _Independent vs. Mutually Exclusive Projects:_ Proyek **independen** adalah proyek yang jika dipilih tidak menyingkirkan proyek lain. Proyek **mutually exclusive** adalah proyek yang saling bersaing; memilih satu berarti harus membuang yang lain.
    - _Unlimited Funds vs. Capital Rationing:_ **Dana tak terbatas** berarti perusahaan bisa mendanai semua proyek yang menguntungkan. **Penjatahan modal** berarti dana perusahaan terbatas sehingga harus memilih proyek-proyek dengan hasil terbaik saja.

---

## ⏱️ 2. Periode Pengembalian (_Payback Period_)

Metode yang paling sering digunakan oleh perusahaan kecil atau sebagai alat tambahan.

- **Definisi:** Waktu yang dibutuhkan suatu investasi untuk menghasilkan arus kas masuk yang cukup untuk menutupi/mengembalikan modal awal.
- **Kriteria Keputusan:** Proyek diterima jika periode pengembaliannya **lebih singkat** dari batas waktu maksimal yang ditetapkan perusahaan.
- **Kelebihan:** Sangat mudah dihitung, sederhana, dan secara kasar memperhitungkan risiko (karena semakin cepat uang kembali, semakin baik).
- **Kekurangan Utama:**
    1. Tidak terhubung dengan tujuan utama memaksimalkan kekayaan pemegang saham.
    2. **Mengabaikan Nilai Waktu Uang (_Time Value of Money_)**: Uang yang masuk di tahun ke-1 dianggap sama nilainya dengan uang di tahun ke-3.
    3. **Mengabaikan arus kas setelah titik impas**: Proyek yang butuh waktu lama untuk balik modal tapi menghasilkan untung raksasa setelahnya bisa saja ditolak oleh metode ini.
- **Modifikasi - _Discounted Payback_:** Menghitung _payback_ dengan mendiskontokan arus kas terlebih dahulu. Ini menyelesaikan masalah "nilai waktu uang", tetapi tetap mengabaikan arus kas setelah periode impas tercapai,.

---

## 💰 3. Nilai Sekarang Bersih (_Net Present Value / NPV_)

Ini adalah metode "Standar Emas" (_gold standard_) yang digunakan oleh sebagian besar perusahaan besar.

- **Definisi:** Dihitung dengan mengurangi modal awal proyek dengan total Nilai Sekarang (_Present Value_) dari seluruh arus kas masuk masa depan yang diharapkan. Arus kas didiskontokan menggunakan biaya modal perusahaan (_Cost of Capital_),.
- **Kriteria Keputusan:**
    - Jika **NPV > $0**, maka **TERIMA** proyek. Proyek ini akan menambah nilai/kekayaan perusahaan.
    - Jika **NPV < $0**, maka **TOLAK** proyek.
- **Indeks Profitabilitas (_Profitability Index / PI_):** Variasi dari NPV. Rasio antara Present Value arus kas masuk dibagi dengan modal awal. Proyek diterima jika **PI > 1.0** (Keputusannya akan selalu sejalan dengan NPV),.
- **_Economic Value Added (EVA)_:** Menilai proyek dari segi "Laba Ekonomi Murni" (_pure economic profit_), yaitu laba dikurangi biaya modal. Mendiskontokan nilai EVA tahunan akan menghasilkan nilai yang **sama persis** dengan NPV,,.

---

## 📈 4. Tingkat Pengembalian Internal (_Internal Rate of Return / IRR_)

Sangat populer di kalangan manajer karena hasilnya berupa "persentase".

- **Definisi:** Tingkat diskonto yang membuat NPV dari suatu investasi sama dengan **Nol ($0)**. IRR pada dasarnya menunjukkan tingkat pengembalian majemuk tahunan rata-rata yang sebenarnya akan dihasilkan oleh proyek tersebut.
- **Kriteria Keputusan:**
    - Jika **IRR > Biaya Modal (_Cost of Capital_)**, maka **TERIMA** proyek.
    - Jika **IRR < Biaya Modal**, maka **TOLAK** proyek.

---

## ⚔️ 5. Membandingkan NPV vs. IRR

Walaupun NPV dan IRR biasanya memberikan kesimpulan yang sama untuk proyek tunggal, keduanya bisa memberikan **peringkat yang bertentangan (_conflicting rankings_)** ketika mengevaluasi proyek _Mutually Exclusive_ (harus milih salah satu).

- **Profil NPV (_NPV Profiles_):** Grafik yang memetakan NPV proyek pada berbagai tingkat diskonto yang berbeda. Sangat berguna untuk membandingkan kapan NPV suatu proyek bisa lebih besar atau lebih kecil dari proyek lain saat suku bunga berubah,.
- **Penyebab Konflik Peringkat:**
    1. **Perbedaan Waktu Arus Kas:** Proyek dengan arus kas besar di depan vs. proyek dengan arus kas yang stabil namun berjangka lebih panjang.
    2. **Perbedaan Besaran (_Magnitude_):** IRR bisa mengunggulkan proyek bermodal sangat kecil dengan _return_ persentase tinggi (misal: modal $2 jadi $3 = IRR 50%), padahal NPV mengunggulkan proyek besar yang menghasilkan lebih banyak uang kas absolut (misal: modal $1.000 jadi $1.100 = IRR 10%, tapi untungnya $100),.
- **Masalah IRR Ganda (_Multiple IRRs_):** Jika sebuah proyek memiliki arus kas yang tidak biasa (misal: keluar uang di awal, masuk di tengah, lalu keluar uang lagi di akhir untuk penutupan), secara matematis proyek itu bisa memiliki **lebih dari satu nilai IRR**. Dalam kondisi ini, IRR tidak bisa digunakan,.
- **Solusi IRR Ganda - _Modified IRR (MIRR)_:** Memodifikasi arus kas dengan menarik semua arus kas keluar ke titik nol dan memajemukkan arus kas masuk ke akhir proyek, sehingga hanya tercipta satu nilai IRR,.
- **Kesimpulan: Mana yang lebih baik?** Secara teori keuangan, **NPV adalah metode yang paling superior** karena tidak memiliki masalah IRR ganda dan selalu memilih proyek yang paling besar dampaknya terhadap kekayaan pemegang saham,. Namun, dalam praktiknya, para manajer eksekutif tetap lebih menyukai IRR karena jauh lebih mudah dikomunikasikan ke pihak lain secara intuitif (karena berupa rasio persentase return).