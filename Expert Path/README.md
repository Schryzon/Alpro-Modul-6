# 📖 Panduan Pengerjaan — Jalur Expert (Schryza Resistance)

<p align="center">
  <img src="../Assets/Banner-Xydos.jpg" width="70%">
</p>
<p align="center">
  <img src="../Assets/Schryza-Resistance.png" width="70%">
</p>

<p align="center">
  <a href="Indonesian.md">
    <img src="https://img.shields.io/badge/📄_BACA_SOAL_(INDONESIA)-8B0000?style=for-the-badge&logoColor=white" alt="Baca Soal ID">
  </a>
  &nbsp;
  <a href="English.md">
    <img src="https://img.shields.io/badge/📄_READ_CHALLENGE_(ENGLISH)-1A1A2E?style=for-the-badge&logoColor=white" alt="Read Challenge EN">
  </a>
</p>

---

## Daftar Isi
1. [Apa yang Harus Dibuat?](#1-apa-yang-harus-dibuat)
2. [Cara Membaca Soal di GitHub](#2-cara-membaca-soal-di-github)
3. [Cara Mengerjakan](#3-cara-mengerjakan)
4. [Cara Mengcompile & Menjalankan Program](#4-cara-mengcompile--menjalankan-program)
5. [Cara Mengumpulkan ke Google Classroom](#5-cara-mengumpulkan-ke-google-classroom)
6. [Checklist Sebelum Kumpul](#6-checklist-sebelum-kumpul)

---

## 1. Apa yang Harus Dibuat?

Kamu diminta membuat sebuah **program C++** (satu file `.cpp`) yang mensimulasikan "Resistance Recovery Terminal" — sebuah terminal berbasis teks (TUI) untuk memperbaiki *neural core* milik tiga Koura Sisters: **Historia, Mira, dan Victoria**.

Program ini harus memiliki **8 fitur utama** yang bisa diakses dari menu:

| No | Opsi Menu | Fungsi |
|:--:|:----------|:-------|
| 1 | **Show Historia's memories** | Tampilkan semua entri memori Historia |
| 2 | **Show Mira's memories** | Tampilkan semua entri memori Mira |
| 3 | **Show Victoria's memories** | Tampilkan semua entri memori Victoria |
| 4 | **Add memory to a sister** | Tambah entri baru (char\*, int, uint, double) |
| 5 | **Delete memory by index** | Hapus entri + tail reclamation |
| 6 | **Reallocate a sister's pool** | Resize pool menggunakan `realloc()` |
| 7 | **Check if sisters are full** | Cek status penuh/tidak tiap sister |
| 8 | **Print pool diagnostics** | Tampilkan statistik penggunaan memori |
| 0 | **Exit** | Keluar dan tampilkan ringkasan akhir |

> **Baca soal lengkapnya di sini:**

<p align="center">
  <a href="Indonesian.md">
    <img src="https://img.shields.io/badge/📋_SOAL_LENGKAP_(BAHASA_INDONESIA)-8B0000?style=for-the-badge&logoColor=white" alt="Soal Lengkap">
  </a>
</p>

---

## 2. Cara Membaca Soal di GitHub

Kamu sedang ada di GitHub. Berikut cara navigasinya:

1. **Kamu sekarang ada di folder `Expert Path/`** — ini folder jalurmu.
2. Klik file **`Indonesian.md`** di atas (atau tombol merah di atas) untuk membaca soal lengkap dalam Bahasa Indonesia.
3. Kalau mau baca dalam Bahasa Inggris, klik **`English.md`**.
4. Kalau mau baca cerita latar belakangnya (sangat disarankan!), klik tombol ungu yang ada di halaman soal.

> 💡 **Tips:** Kalau kamu bingung kenapa tampilannya cuma teks biasa, pastikan kamu membuka file `.md`, bukan file `.cpp`.

---

## 3. Cara Mengerjakan

### Persiapan
1. **Install compiler C++** — Gunakan [MinGW-w64](https://www.mingw-w64.org/) atau [TDM-GCC](https://jmeubank.github.io/tdm-gcc/). Kalau sudah punya, lewati langkah ini.
2. **Buat file baru** bernama `NIM_nama.cpp` (contoh: `F1D02410053_AyuRahayu.cpp`) di komputer kamu.
3. **Tulis kodemu** di file tersebut menggunakan IDE atau teks editor apapun (VS Code, Code::Blocks, Notepad++, dll).

### Yang Harus Diimplementasikan
Berdasarkan soal di [`Indonesian.md`](Indonesian.md), implementasikan:

- [ ] `union Gap` — representasi gap khusus tiap sister
- [ ] `struct Memory_Entry` — struktur data tiap entri memori
- [ ] `struct Sister` — struktur data satu sister (pool + bump + entries)
- [ ] `init_sister()` — inisialisasi pool dengan `malloc()`
- [ ] `free_sister()` — bebaskan pool dengan `free()`
- [ ] **Validasi `argc` & `argv`** — program harus menerima NIM lewat command line
- [ ] **Perhitungan gap dari NIM** — Historia, Mira, Victoria punya formula berbeda
- [ ] **Memory alignment** — `align_up()` untuk kalkulasi offset yang benar
- [ ] `add_memory_char()` — tambah string (dengan gap + null terminator)
- [ ] `add_memory_int()` — tambah integer (4 byte)
- [ ] `add_memory_uint()` — tambah unsigned int (4 byte)
- [ ] `add_memory_double()` — tambah double (8 byte)
- [ ] `delete_memory()` — hapus entri + tail reclamation
- [ ] `resize_sister_pool()` — resize pool dengan `realloc()` + deteksi out-of-bounds
- [ ] `show_memories()` — tampilkan semua entri + informasi offset & jump
- [ ] `print_pool_diagnostics()` — tampilkan statistik utilization%
- [ ] **Data awal (seed)** — 5 entri awal untuk 3 sister saat program pertama berjalan
- [ ] **Menu utama** dengan 9 opsi

### Aturan Penting
- **Wajib menggunakan `malloc()`, `realloc()`, dan `free()`** dari `<cstdlib>`. Jangan gunakan `new`/`delete`.
- **Tidak boleh menggunakan `std::vector`, `std::string`, atau STL container lainnya**.
- Tidak boleh menggunakan `atoi()`, `strtol()`, atau fungsi konversi string bawaan — harus manual.
- **Memory alignment wajib diimplementasikan** — Historia 16-byte, Mira 8-byte, Victoria 4-byte.
- **Special Gap wajib diterapkan** hanya pada alokasi `char*`, dihitung dari NIM.
- Pastikan `free()` dipanggil untuk semua pool sebelum program selesai.

---

## 4. Cara Mengcompile & Menjalankan Program

Buka **Command Prompt** atau **Terminal**, lalu jalankan perintah berikut:

### Compile
```bash
g++ -o solution NIM_nama.cpp
```
Contoh:
```bash
g++ -o solution F1D02410053_AyuRahayu.cpp
```

### Jalankan
```bash
./solution F1D02410053
```
Ganti `F1D02410053` dengan **NIM kamu yang sebenarnya**. NIM inilah yang menentukan nilai gap untuk ketiga sister.

### Verifikasi output awal
Saat berhasil dijalankan, program harus menampilkan:
```text
============================================================
SCHRYZA RESISTANCE — RECOVERY PROTOCOL [TERMINAL: PHOENIX]
============================================================
```
Diikuti menu utama.

### Kalau muncul error saat compile
- Pastikan file `.cpp`-mu ada di direktori yang sama dengan tempat kamu menjalankan perintah.
- Pastikan `g++` sudah ter-install dan bisa diakses dari terminal (ketik `g++ --version` untuk cek).
- Pastikan kamu menyertakan header yang dibutuhkan: `<climits>`, `<cstdlib>`, `<iostream>`.

> 💡 **Contoh output lengkap** (termasuk semua menu dan error handler) bisa kamu lihat di bagian **"Contoh Input dan Output"** di dalam file [`Indonesian.md`](Indonesian.md).

---

## 5. Cara Mengumpulkan ke Google Classroom

<p align="center">
  <a href="https://classroom.google.com/c/ODQwMjgxMjEwMDQw">
    <img src="https://img.shields.io/badge/🎓_BUKA_GOOGLE_CLASSROOM-4285F4?style=for-the-badge&logo=google-classroom&logoColor=white" alt="Google Classroom">
  </a>
</p>

1. **Buka Google Classroom** di [classroom.google.com](https://classroom.google.com) atau klik tombol di atas.
2. Masuk ke kelas **Algoritma & Pemrograman** yang sesuai.
3. Cari tugas **"Modul 6 — Pointers and Memory Engineering"**.
4. Klik **"Tambahkan atau Buat"** → **"File"**.
5. Upload file `.cpp`-mu dengan nama format: **`NIM_NamaLengkap.cpp`**
   - Contoh: `F1D02410053_AyuRahayu.cpp`
6. Klik **"Serahkan"** (tombol biru).

> ⚠️ **Pastikan kamu mengumpulkan file `.cpp`**, bukan `.exe`, `.o`, atau file lainnya.

---

## 6. Checklist Sebelum Kumpul

Centang semua sebelum mengumpulkan:

- [ ] Program bisa di-compile tanpa error
- [ ] Program menerima NIM lewat `argv[1]` dan memvalidasinya (format `F1D02xxxxxx`)
- [ ] **Gap dihitung dengan benar** dari 3 digit terakhir NIM untuk masing-masing sister
- [ ] Data awal (5 entri seed) muncul saat program pertama dijalankan
- [ ] Menu menampilkan 9 opsi (1–8 + 0 untuk exit)
- [ ] Fitur **Show Memories** (opsi 1/2/3) menampilkan offset, size, address, jump, dan value tiap entri
- [ ] **Memory alignment** diterapkan: Historia 16-byte, Mira 8-byte, Victoria 4-byte
- [ ] **Special Gap** diterapkan hanya pada alokasi `char*`
- [ ] Fitur **Add Memory** (opsi 4) bisa menambahkan 4 tipe data (char\*, int, uint, double)
- [ ] Error "Not enough space" muncul saat pool penuh
- [ ] Error "Entry overflow" muncul saat 128 slot terisi
- [ ] Fitur **Delete Memory** (opsi 5) bisa menghapus entri dan melakukan tail reclamation
- [ ] Pesan fragmentasi muncul saat menghapus entri non-tail
- [ ] Fitur **Reallocate Pool** (opsi 6) menggunakan `realloc()` + menampilkan warning out-of-bounds jika perlu
- [ ] Fitur **Check Full** (opsi 7) menampilkan status ketiga sister sekaligus
- [ ] Fitur **Diagnostics** (opsi 8) menampilkan utilisasi % dengan benar
- [ ] Program menampilkan ringkasan akhir dan memanggil `free()` sebelum keluar
- [ ] Tidak ada memory leak
- [ ] Nama file format: `NIM_NamaLengkap.cpp`

---

<p align="center">
  <a href="../README.md">
    <img src="https://img.shields.io/badge/⬅️_KEMBALI_KE_HALAMAN_UTAMA-555555?style=for-the-badge&logoColor=white" alt="Kembali">
  </a>
</p>
