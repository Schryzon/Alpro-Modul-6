# 📖 Panduan Pengerjaan — Jalur Expert (Schryzan Resistance)

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

## 🛠️ Butuh Bantuan Teknis?

Jika kamu kesulitan memahami konsep **Pointer**, **Memory Alignment**, atau **Malloc/Free**, silakan baca panduan teknis lengkap kami:
- 🇮🇩 [Panduan Teknis (Bahasa Indonesia)](../Guidebooks/guidebook-indonesian.md)
- 🇬🇧 [Technical Guidebook (English)](../Guidebooks/guidebook-english.md)

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

Program ini harus memiliki **6 fitur utama** yang bisa diakses dari menu:

| No | Opsi Menu | Fungsi |
|:--:|:----------|:-------|
| 1 | **Show Historia's memories** | Tampilkan semua entri memori Historia |
| 2 | **Show Mira's memories** | Tampilkan semua entri memori Mira |
| 3 | **Show Victoria's memories** | Tampilkan semua entri memori Victoria |
| 4 | **Add memory to a sister** | Tambah entri baru (char\*, uint, double) |
| 5 | **Delete memory by index** | Hapus entri + tail reclamation |
| 6 | **Print pool diagnostics** | Tampilkan statistik penggunaan memori |
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

- [ ] `struct Gap` — representasi gap khusus tiap sister
- [ ] `struct Memory_Entry` — struktur data tiap entri memori
- [ ] `struct Sister` — struktur data satu sister (pool + bump + entries)
- [ ] `init_sister()` — inisialisasi pool dengan `malloc()`
- [ ] `free_sister()` — bebaskan pool dengan `free()`
- [ ] **Validasi `argc` & `argv`** — program harus menerima NIM lewat command line
- [ ] **Perhitungan gap dari NIM** — Historia, Mira, Victoria punya formula berbeda
- [ ] **Memory alignment** — `align_up()` untuk kalkulasi offset yang benar
- [ ] `add_memory_uint()` — tambah unsigned int (4 byte)
- [ ] `add_memory_double()` — tambah double (8 byte)
- [ ] `delete_memory()` — hapus entri + tail reclamation
- [ ] `show_memories()` — tampilkan semua entri + informasi offset & jump
- [ ] `print_pool_diagnostics()` — tampilkan statistik utilization%
- [ ] **Data awal (seed)** — 4 entri awal untuk 3 sister saat program pertama berjalan
- [ ] **Menu utama** dengan 7 opsi

### Aturan Penting
- **Wajib menggunakan `malloc()` dan `free()`** dari `<cstdlib>`. Jangan gunakan `new`/`delete`.
- **`memcpy()` diperbolehkan** untuk penyalinan data.
- **Tidak boleh menggunakan `std::vector`, `std::string`, atau STL container lainnya**.
- Tidak boleh menggunakan `atoi()`, `strtol()`, atau fungsi konversi string bawaan — harus manual.
- **Memory alignment wajib diimplementasikan** — Historia 16-byte, Mira 8-byte, Victoria 4-byte.
- **Special Gap wajib diterapkan** hanya pada alokasi `char*`, dihitung dari NIM.
- Pastikan `free()` dipanggil untuk semua pool sebelum program selesai.

---

## 4. Cara Mengcompile & Menjalankan Program

> ⚠️ **PERHATIAN: Program ini TIDAK bisa langsung di-F11 atau di-F9 di Dev-C++!**
> Program ini membutuhkan **argumen NIM** saat dijalankan. Baca panduan di bawah dengan teliti.

---

### 🧠 Kenapa Tidak Bisa Langsung Tekan F11?

Program ini menggunakan `argc` dan `argv` — mekanisme standar C++ untuk menerima input dari **command line sebelum program berjalan**.

```cpp
int main(int argc, char** argv) {
    // argc = jumlah argumen (termasuk nama program itu sendiri)
    // argv[0] = nama file executable, contoh: "solution.exe"
    // argv[1] = NIM kamu, contoh: "F1D02410053"
}
```

Ketika kamu menekan **F11 di Dev-C++**, program berjalan tanpa argumen apapun (`argc == 1`), sehingga program langsung menampilkan error:
```text
Usage: solution.exe <student_id>
Example: solution.exe F1D02410053
```
...lalu keluar. **Ini bukan bug di kodemu.** Ini memang desainnya.

---

### 🖥️ Metode 1: Dev-C++ (dengan Compiler Arguments)

Ini cara yang paling direkomendasikan kalau kamu tetap ingin pakai Dev-C++.

**Langkah-langkah:**

**1.** Buka Dev-C++ dan buka file `.cpp`-mu.

**2.** Klik menu **Execute** → **Parameters...**

```
Menu Bar → Execute → Parameters...
```

**3.** Pada kotak **"Parameters"**, ketik NIM kamu:
```
F1D02410053
```
> Ganti dengan NIM kamu sendiri. Pastikan formatnya benar: `F1D02` diikuti 6 digit angka.

**4.** Klik **OK** untuk menyimpan.

**5.** Sekarang compile dan jalankan seperti biasa dengan **F11**.

Program akan menerima NIM tersebut sebagai `argv[1]` dan berjalan dengan benar.

> ⚠️ **Ingat:** Setiap kali kamu membuka ulang Dev-C++, kamu mungkin perlu mengatur Parameter ini lagi.

---

### 💻 Metode 2: Command Prompt / Terminal (Direkomendasikan)

Cara ini lebih andal dan merupakan cara yang benar untuk program berbasis argumen.

**Windows (Command Prompt):**

**1.** Tekan `Win + R`, ketik `cmd`, lalu Enter.

**2.** Navigasi ke folder tempat file `.cpp`-mu berada menggunakan perintah `cd`:
```cmd
cd C:\Users\NamaMu\Documents\Alpro
```

**3.** Compile program:
```cmd
g++ -o solution NIM_NamaLengkap.cpp
```
Contoh nyata:
```cmd
g++ -o solution F1D02410053_AyuRahayu.cpp
```

**4.** Jalankan dengan NIM sebagai argumen:
```cmd
solution.exe F1D02410053
```
atau
```cmd
.\solution.exe F1D02410053
```

**Linux / WSL / Git Bash:**
```bash
g++ -o solution F1D02410053_AyuRahayu.cpp
./solution F1D02410053
```

---

### ✅ Verifikasi Output Awal

Jika berhasil, program harus menampilkan banner ini diikuti menu utama:
```text
============================================================
SCHRYZA RESISTANCE — RECOVERY PROTOCOL [TERMINAL: PHOENIX]
============================================================
You are CyroN's Memory Architect.
Heed the gods and heal the sisters.
------------------------------------------------------------
Menu
------------------------------------------------------------
1 - Show Historia's memories
...
```

---

### ❌ Error Umum dan Solusinya

| Error | Penyebab | Solusi |
|:------|:---------|:-------|
| `Usage: solution.exe <student_id>` | Program berjalan tanpa argumen (F11 tanpa parameter) | Atur **Execute → Parameters** di Dev-C++, atau pakai CMD |
| `Error: Student ID must start with F1D02` | NIM salah format | Pastikan formatnya persis `F1D02xxxxxx` (kapital, 11 karakter) |
| `Error: Student ID must be exactly 11 characters` | NIM terlalu pendek/panjang | Hitung ulang: `F1D02` + 6 digit = 11 karakter total |
| `'g++' is not recognized` | Compiler belum ter-install atau belum di PATH | Install [MinGW-w64](https://www.mingw-w64.org/) dan tambahkan ke PATH |
| Layar hitam langsung tutup | Program crash atau selesai terlalu cepat | Jalankan dari CMD, bukan klik `.exe` langsung |

> 💡 **Tip:** Kalau bingung di mana file `.cpp`-mu tersimpan, di Dev-C++ klik kanan nama file di editor → **"Open Containing Folder"**, lalu jalankan CMD dari folder tersebut dengan Shift+Klik kanan → **"Open PowerShell/Command Prompt here"**.

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
- [ ] Data awal (4 entri seed) muncul saat program pertama dijalankan
- [ ] Menu menampilkan 7 opsi (1–6 + 0 untuk exit)
- [ ] Fitur **Show Memories** (opsi 1/2/3) menampilkan offset, size, address, jump, dan value tiap entri
- [ ] **Memory alignment** diterapkan: Historia 16-byte, Mira 8-byte, Victoria 4-byte
- [ ] **Special Gap** diterapkan hanya pada alokasi `char*`
- [ ] Fitur **Add Memory** (opsi 4) bisa menambahkan 3 tipe data (char\*, uint, double)
- [ ] Error "Not enough space" muncul saat pool penuh
- [ ] Error "Entry overflow" muncul saat 128 slot terisi
- [ ] Fitur **Delete Memory** (opsi 5) bisa menghapus entri dan melakukan tail reclamation
- [ ] Pesan fragmentasi muncul saat menghapus entri non-tail
- [ ] Fitur **Diagnostics** (opsi 6) menampilkan utilisasi % dengan benar
- [ ] Program menampilkan ringkasan akhir dan memanggil `free()` sebelum keluar
- [ ] Tidak ada memory leak
- [ ] Nama file format: `NIM_NamaLengkap.cpp`

---

<p align="center">
  <a href="../README.md">
    <img src="https://img.shields.io/badge/⬅️_KEMBALI_KE_HALAMAN_UTAMA-555555?style=for-the-badge&logoColor=white" alt="Kembali">
  </a>
</p>
