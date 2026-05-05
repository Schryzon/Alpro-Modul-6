# 📖 Panduan Pengerjaan — Jalur Regular (CyroN Executive)

<p align="center">
  <img src="../Assets/Banner-Xydos.jpg" width="70%">
</p>
<p align="center">
  <img src="../Assets/Cyron-Office.png" width="70%">
</p>

<p align="center">
  <a href="Indonesian.md">
    <img src="https://img.shields.io/badge/📄_BACA_SOAL_(INDONESIA)-00509E?style=for-the-badge&logoColor=white" alt="Baca Soal ID">
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

Kamu diminta membuat sebuah **program C++** (satu file `.cpp`) yang mensimulasikan "CyroN Divine Interface" — sebuah terminal berbasis teks (TUI) untuk mengelola *neural buffer* dari subjek Historia.

Program ini harus memiliki **5 fitur utama**:

| No | Fitur | Fungsi |
|:--:|:------|:-------|
| 1 | **View Neural Map** | Tampilkan semua data yang ada di buffer beserta statusnya |
| 2 | **Inject Neural Thread** | Tambahkan data baru ke buffer (teks atau angka integer) |
| 3 | **Purge Corrupted Link** | Hapus data berdasarkan indeks |
| 4 | **Expand Willpower** | Perbesar ukuran buffer menggunakan `new` dan `delete[]` |
| 0 | **Surrender** | Keluar dari program |

> **Baca soal lengkapnya di sini:**

<p align="center">
  <a href="Indonesian.md">
    <img src="https://img.shields.io/badge/📋_SOAL_LENGKAP_(BAHASA_INDONESIA)-00509E?style=for-the-badge&logoColor=white" alt="Soal Lengkap">
  </a>
</p>

---

## 2. Cara Membaca Soal di GitHub

Kamu sedang ada di GitHub. Berikut cara navigasinya:

1. **Kamu sekarang ada di folder `Regular Path/`** — ini folder jalurmu.
2. Klik file **`Indonesian.md`** di atas (atau tombol biru di atas) untuk membaca soal lengkap dalam Bahasa Indonesia.
3. Kalau mau baca dalam Bahasa Inggris, klik **`English.md`**.
4. Kalau mau baca cerita latar belakangnya (opsional tapi keren), klik tombol ungu yang ada di halaman soal.

> 💡 **Tips:** Kalau kamu bingung kenapa tampilannya cuma teks biasa, pastikan kamu membuka file `.md`, bukan file `.cpp`.

---

## 3. Cara Mengerjakan

### Persiapan
1. **Install compiler C++** — Gunakan [MinGW-w64](https://www.mingw-w64.org/) atau [TDM-GCC](https://jmeubank.github.io/tdm-gcc/). Kalau sudah punya, lewati langkah ini.
2. **Buat file baru** bernama `NIM_nama.cpp` (contoh: `F1D02240001_BudiSantoso.cpp`) di komputer kamu.
3. **Tulis kodemu** di file tersebut menggunakan IDE atau teks editor apapun (VS Code, Code::Blocks, Notepad++, dll).

### Yang Harus Diimplementasikan
Berdasarkan soal di [`Indonesian.md`](Indonesian.md), implementasikan:

- [ ] `struct Neural_Core` — struktur data utama buffer Historia
- [ ] `struct Neural_Entry` — struktur data tiap entri di dalam buffer
- [ ] `init_core()` — inisialisasi buffer dengan `new`
- [ ] `destroy_core()` — bebaskan memori dengan `delete[]`
- [ ] **Validasi `argc` & `argv`** — program harus menerima NIM lewat command line
- [ ] **Menu utama** dengan 4 opsi + opsi keluar
- [ ] `show_status()` — tampilkan isi buffer + persentase stabilitas
- [ ] `inject_thread()` — tambah data ke buffer (Willpower = teks, Thunder = int)
- [ ] `purge_link()` — hapus entri + tail reclamation
- [ ] `expand_willpower()` — resize buffer dengan alur New-Copy-Delete

### Aturan Penting
- **Wajib menggunakan `new` dan `delete[]`**, bukan `malloc`/`free`.
- **Tidak boleh menggunakan `std::vector`, `std::string`, atau STL container lainnya**.
- Pastikan program menerima NIM sebagai argumen (`argc`/`argv`) dan memvalidasi formatnya (`F1D02xxxxxx`).
- **Tidak ada memory leak** — setiap `new` harus ada pasangan `delete[]`-nya.

---

## 4. Cara Mengcompile & Menjalankan Program

Buka **Command Prompt** atau **Terminal**, lalu jalankan perintah berikut:

### Compile
```bash
g++ -o solution NIM_nama.cpp
```
Contoh:
```bash
g++ -o solution F1D02240001_BudiSantoso.cpp
```

### Jalankan
```bash
./solution F1D02240001
```
Ganti `F1D02240001` dengan **NIM kamu yang sebenarnya**.

### Kalau muncul error saat compile
- Pastikan file `.cpp`-mu ada di direktori yang sama dengan tempat kamu menjalankan perintah.
- Pastikan `g++` sudah ter-install dan bisa diakses dari terminal (ketik `g++ --version` untuk cek).

> 💡 **Contoh output yang diharapkan** bisa kamu lihat di bagian **"Contoh Input dan Output"** di dalam file [`Indonesian.md`](Indonesian.md).

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
   - Contoh: `F1D02240001_BudiSantoso.cpp`
6. Klik **"Serahkan"** (tombol biru).

> ⚠️ **Pastikan kamu mengumpulkan file `.cpp`**, bukan `.exe` atau file lainnya.

---

## 6. Checklist Sebelum Kumpul

Centang semua sebelum mengumpulkan:

- [ ] Program bisa di-compile tanpa error
- [ ] Program menerima NIM lewat `argv[1]` dan memvalidasinya
- [ ] Menu utama menampilkan 4 opsi + opsi keluar
- [ ] Fitur **View Neural Map** bekerja dan menampilkan data + persentase stabilitas
- [ ] Fitur **Inject Thread** bisa menambahkan teks (`Willpower`) dan integer (`Thunder`)
- [ ] Buffer overflow ditangani dengan pesan error yang sesuai
- [ ] Fitur **Purge Link** bisa menghapus entri + melakukan tail reclamation jika perlu
- [ ] Fitur **Expand Willpower** memperbesar buffer menggunakan alur **New-Copy-Delete**
- [ ] Tidak ada memory leak (`delete[]` dipanggil sebelum program selesai)
- [ ] Nama file format: `NIM_NamaLengkap.cpp`

---

<p align="center">
  <a href="../README.md">
    <img src="https://img.shields.io/badge/⬅️_KEMBALI_KE_HALAMAN_UTAMA-555555?style=for-the-badge&logoColor=white" alt="Kembali">
  </a>
</p>
