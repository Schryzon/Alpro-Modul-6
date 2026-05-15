# Alpro Modul 6: Pointer dan Rekayasa Memori
## Phantasm of Xydos: Andromeda I

<p align="center">
  <img src="Assets/Banner-Xydos.jpg" width="80%">
</p>

Selamat datang di repositori resmi **Algoritma & Pemrograman Modul 6**. Modul ini berfokus pada aspek C++ yang paling kuat (dan paling berbahaya): **Pointer dan Manajemen Memori Dinamis**.

Disusun sebagai pilihan antara dua jalur yang berlawanan, tugas ini menantang mahasiswa untuk membangun antarmuka manajemen memori sambil menavigasi konflik distopia Galaksi Andromeda.

---

## 🌗 Dua Jalur

Mahasiswa harus memilih satu jalur berdasarkan ambisi teknis dan keberpihakan naratifnya:

### 🏢 Jalur Regular: Loyalis CyroN (Eksekutif)
*   **Narasi**: Melayani Cyber Robotic Network sebagai High Executive Engineer. Efisiensi dan kendali adalah satu-satunya tujuanmu.
*   **Fokus Teknis**:
    - Aritmatika Pointer Dasar.
    - Operator Memori Modern C++ (`new`, `delete[]`).
    - Resize Buffer Manual (Alur New-Copy-Delete).
    - Manajemen buffer tunggal untuk Historia.
*   **Skenario**: [OPERATION: DIVINE OPTIMIZATION]

### ⚔️ Jalur Expert: Perlawanan Schryza (Pemberontak)
*   **Narasi**: Bergabunglah dengan Perlawanan di Planet Schryza. Pertaruhkan segalanya untuk membebaskan vessel-vessel dewa dari belenggu CyroN.
*   **Fokus Teknis**:
    - Aritmatika Pointer Tingkat Lanjut.
    - Manajemen Memori C Standar (`malloc`, `free`).
    - Manajemen multi-pool heap.
    - **Memory Alignment (16/8/4-byte)** dan Kalkulasi Offset Berbasis NIM.
    - Tail Reclamation & Manajemen Cluster.
*   **Skenario**: [RECOVERY PROTOCOL: PHOENIX]

---

## 📁 Struktur Repositori

```text
public/
├── Regular Path/          # Jalur Eksekutif: Kode & Dokumentasi Lengkap
├── Expert Path/           # Jalur Perlawanan: Kode & Dokumentasi Lengkap
├── Story/                 # Narasi lore lengkap (EN & ID)
├── Additional-Info/       # File lore tambahan dan konsep pendukung
└── Assets/                # Visual karakter dan banner
```

---

## 🚀 Cara Mengakses Soal

> Kamu sedang membaca halaman ini di GitHub. Ikuti langkah-langkah di bawah untuk menemukan soal dan panduan pengerjaanmu.

### Langkah 1 — Pilih Jalurmu

Klik salah satu tombol di bawah sesuai jalur yang kamu ambil:

<p align="center">
  <a href="Regular Path/README.md">
    <img src="https://img.shields.io/badge/🏢_JALUR_REGULAR_(CyroN_Executive)-00509E?style=for-the-badge&logoColor=white" alt="Jalur Regular">
  </a>
  &nbsp;&nbsp;
  <a href="Expert Path/README.md">
    <img src="https://img.shields.io/badge/⚔️_JALUR_EXPERT_(Schryza_Resistance)-8B0000?style=for-the-badge&logoColor=white" alt="Jalur Expert">
  </a>
</p>

### Langkah 2 — Baca Soal (Pilih Bahasa)

Setelah masuk ke folder jalurmu, soal tersedia dalam dua bahasa:

| Jalur | 🇬🇧 English | 🇮🇩 Indonesian |
|:---|:---|:---|
| **Regular Path** | [English.md](Regular%20Path/English.md) | [Indonesian.md](Regular%20Path/Indonesian.md) |
| **Expert Path** | [English.md](Expert%20Path/English.md) | [Indonesian.md](Expert%20Path/Indonesian.md) |

### Langkah 3 — Baca Panduan Teknis (Technical Guidebooks)
Jika kamu mengalami kesulitan dalam memahami konsep Pointer, Alignment, atau Dynamic Memory, kamu dapat membaca panduan teknis lengkap yang telah disediakan:

| 🇬🇧 English Version | 🇮🇩 Versi Indonesia |
| :--- | :--- |
| [Technical Guidebook (EN)](Guidebooks/guidebook-english.md) | [Panduan Teknis (ID)](Guidebooks/guidebook-indonesian.md) |

### Langkah 4 — Kerjakan & Kumpulkan
Kumpulkan hasil kerjamu ke **Google Classroom** yang sudah disediakan. Lihat panduan lengkap pengerjaan dan pengumpulan di dalam folder jalurmu masing-masing (file `README.md`).

---

## 🛠️ Persyaratan Teknis

*   **Compiler**: `g++` (MinGW-w64 direkomendasikan) atau compiler kompatibel C++11/14/17 lainnya.
*   **Standard Library**: Penggunaan pustaka standar dibatasi secara ketat untuk mendorong manipulasi memori secara manual.
*   **Flag**: `-O3` direkomendasikan untuk mengamati performa saat diagnostik.

---

## 📜 Lore

Galaksi Andromeda sedang runtuh. Saat **CyroN** (Cyber Robotic Network) berupaya mengindustrialkan para dewa, "Perlawanan Historia" berjuang demi masa depan yang berlandaskan kehendak bebas. Apapun pilihanmu — mengoptimalkan ketertiban atau memulihkan kebebasan — kodemu akan menentukan nasib **Koura Sisters**.

Untuk membaca sejarah arsitektur lengkap galaksi ini, baca narasi terkompilasi di [Direktori Story](Story/):
- 📖 [English_STORY.md](Story/English_STORY.md)
- 📖 [Indonesian_STORY.md](Story/Indonesian_STORY.md)

---

> "Dalam memory, tidak ada jaring pengaman. Hanya pointer yang menentukan nasibmu."  
> — **Jay (∞)**
