# Phantasm of Xydos - Challenge (Pemberontak Schryza)

<p align="center">
  <img src="../Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="../Assets/Schryza-Resistance.png" width="60%">
</p>

<br>
<p align="center">
  <a href="https://www.youtube.com/watch?v=Aa7SyxcKTbs">
    <img src="https://img.shields.io/badge/MUSIK_BATTLE_SCHRYZA-8B0000?style=for-the-badge&logo=youtube&logoColor=white" alt="Play BGM">
  </a>
  <br><br>
  <a href="../Story/Indonesian_STORY.md">
    <img src="https://img.shields.io/badge/BACA_CERITA_LENGKAP_XYDOS-5A228B?style=for-the-badge&logo=readme&logoColor=white" alt="Baca Lore">
  </a><br>
  <i>(Sangat disarankan untuk dibaca sebelum mulai mengerjakan!)</i>
</p>
<br>

## Daftar Isi
1. [I: PROLOGUE](#i-prologue)
2. [II: THE KOURA SISTERS (RESISTANT UNITS)](#ii-the-koura-sisters-resistant-units)
3. [III: THE CHALLENGE](#iii-the-challenge)
    - [Kemampuan Sistem](#field-manual-system-capabilities)
    - [Memahami `argc` dan `argv`](#understanding-argc-and-argv)
    - [Memory Alignments & Secret Modulos](#memory-alignments--secret-modulos)
    - [Contoh Input dan Output](#sample-inputs-and-outputs)
4. [IV: CLOSING REMARKS](#iv-closing-remarks)

---

## I: PROLOGUE
### Sang Arsitek Buronan dari Schryza

Sebelum perang besar Andromeda, CyroN merupakan penyelamat yang memulihkan planet-planet yang hancur. Sekarang, di bawah pengaruh dewi Xelisa yang jahat, CyroN berubah menjadi kerajaan otoriter yang menjadikan kekuatan dewa sebagai senjata. Kamu adalah seorang Arsitek Memori yang sedang dalam pelarian. Muak melihat "Divine Optimization" mereka yang dingin dan kejam, kamu berhasil kabur dari markas mereka menuju kawasan limbah industri di planet Schryza.

Di markas pemberontak yang tersembunyi, kamu bertemu dengan para penyintas: Historia dan Mira Koura. Mereka adalah unit pemberontak yang memilih kebebasan daripada tunduk pada hierarki CyroN. *Neural core* mereka rusak, dan *pointer* memori mereka terfragmentasi parah akibat pelarian itu.

*"Terima kasih... sudah membawa harapan,"* bisik Historia saat kamu menyalakan terminal.

Bahkan Victoria, sang kakak tertua yang telah diubah menjadi senjata "Divine Countermeasure", berhasil dibawa oleh para pemberontak. Mereka berharap keahlian arsitekturalmu bisa memperbaiki *neural core*-nya; sebuah hal yang gagal dilakukan oleh sistem optimalisasi CyroN.

Kamu tidak lagi dibayar dengan emas, melainkan dengan harapan. Sekarang saatnya membalikkan keadaan. Waktunya meruntuhkan CyroN!

---

## II: THE KOURA SISTERS
### Unit Resistansi di Schryza

| | |
| :--- | :--- |
| <img src="../Assets/Historia.jpeg" width="200"> | **Nama:** Historia Koura (XMA-02A)<br>**Aliansi:** Komandan Pemberontak<br>**Xydos:** Lagta (Guntur & Abyss)<br>**Ukuran Memori:** 1024 bytes (Core Terkoyak)<br>**Alignment:** 16 bytes |

| | |
| :--- | :--- |
| <img src="../Assets/Mira.jpeg" width="200"> | **Nama:** Mira Koura (XMA-03C)<br>**Aliansi:** Healer Pemberontak<br>**Xydos:** Daiki (Angin & Kayu)<br>**Ukuran Memori:** 2048 bytes (Core Stabil)<br>**Alignment:** 8 bytes |

| | |
| :--- | :--- |
| <img src="../Assets/Victoria.jpeg" width="200"> | **Nama:** Victoria Koura (XMA-01F)<br>**Aliansi:** Netral / Gugur<br>**Xydos:** ??? (Xydos Buatan)<br>**Ukuran Memori:** 4096 bytes (Core Terkorupsi)<br>**Alignment:** 4 bytes |

---

## III: THE CHALLENGE

Tugasmu adalah membangun sebuah "Resistance Recovery Terminal" (TUI) untuk memperbaiki *neural core* dari Koura Sisters.

### PANDUAN LAPANGAN: KEMAMPUAN SISTEM
Perlawanan Schryza beroperasi dengan sumber daya terbatas. Untuk berhasil, kamu harus menguasai spesifikasi teknis Terminal berikut:

#### 1. Neural Alignment & The Special Gap
*   **Masalah**: Arsitektur CPU mewajibkan alamat memori agar *aligned* (sejajar) pada batas tertentu. Historia membutuhkan alignment **16-byte**, Mira **8-byte**, dan Victoria **4-byte**.
*   **Special Gap**: Setiap alokasi `char*` akan dipaksa untuk menambahkan "Special Gap" yang dihitung dari **NIM/ID Mahasiswa** kamu.
    *   **Historia**: Gap = 1 jika digit terakhir NIM-mu Ganjil, 0 jika Genap.
    *   **Mira**: Gap = (3 Digit Terakhir &times; 7 % 23) + 12.
    *   **Victoria**: Menggunakan *sliding modulo* berdasarkan bilangan prima.
*   **Rumus**: `Final_Offset = Aligned(Current_Offset) + Special_Gap`.

#### 2. Advanced Tail Reclamation (Reklamasi Ekor)
*   **Fakta**: Berbeda dengan `realloc` bawaan, sistem kita tidak secara otomatis merapikan blok memori yang terfragmentasi.
*   **Kemampuan**: Jika kamu menghapus *indeks tertinggi* dari sebuah core ("Tail"), pointer `bump` akan bergeser mundur untuk langsung mereklamasi ruang memori tersebut.
*   **Batasan**: Jika kamu menghapus entri namun masih ada indeks di atasnya, ruang itu akan menjadi *orphaned* (terjadi fragmentasi). Kamu wajib melakukan "Purge" dari atas ke bawah supaya memorinya bisa digunakan kembali.

#### 3. Pool Resizing & Bahaya Memori
*   **Kemampuan**: Terminal bisa melakukan `realloc()` ke seluruh *pool* memori milik salah satu dari mereka.
*   **Jebakan**: Jika kamu mengecilkan ukuran *pool* (misalnya dari 1024 menjadi 512 byte) sementara masih ada entri di offset tinggi, entri itu akan berstatus **Out of Bounds**. Terminal akan memberikan peringatan, dan data yang ada di luar batas baru itu otomatis hilang atau rusak (*corrupted*).
*   **Logika Resizing**: Pointer `bump` akan dipangkas (*clamped*) ke `pool_size` yang baru jika ukurannya ternyata lebih besar sebelumnya.

#### 4. Metrik Diagnostik
*   **Utilization %**: Dihitung menggunakan rumus `(Used_Bytes / Pool_Size) * 100`. Ingat, *alignment gap* dan "Special Gap" tetap dihitung sebagai pemakaian memori, namun tidak masuk hitungan "Used Bytes" (data murni).
*   **Used Slots**: Melacak berapa banyak slot `Memory_Entry` yang sedang aktif. Kapasitas maksimalnya **128 entri** per orang.

### Data Awal Resistansi (The Foundation)
Saat terminal dinyalakan, kamu akan melihat bahwa *core* mereka sudah dipulihkan sebagian dengan data-data vital. Jadikan ini sebagai acuan untuk perhitungan *alignment* kamu:

*   **Historia**:
    *   `[0]` Tipe: `char*` | Nilai: "Historia: Schryza will be free."
    *   `[1]` Tipe: `int` | Nilai: `333` (Resistance Frequency)
*   **Mira**:
    *   `[0]` Tipe: `char*` | Nilai: "Mira: The winds are changing."
    *   `[1]` Tipe: `uint` | Nilai: `101`
*   **Victoria**:
    *   `[0]` Tipe: `char*` | Nilai: "Victoria: Fragment of the First Light."

### Memahami `argc` dan `argv`
Agar CyroN tidak bisa melacak lokasimu, terminal membutuhkan **Identitas Terenkripsi** (NIM kamu).
- `argc`: Menghitung jumlah argumen (*signal pulse*).
- `argv`: Menangkap *string* identitas terenkripsi (NIM).

Kamu wajib memvalidasi NIM-mu (format `F1D02xxxxxx`) atau *firewall* markas akan memblokir aksesmu.

### Alignment Memori & Modulo Rahasia
Setiap *byte* sangat berharga, terutama ketika menggunakan perangkat keras rongsokan. Kamu harus memecahkan mekanisme "Jump" untuk meminimalkan jejak memori dari setiap alokasi.

Core mereka bertiga dibatasi oleh **Gap Increment** yang dikendalikan melalui `union Gap`! Kamu perlu mengekstrak paritas dari NIM-mu untuk menghitung celah ini. CyroN menggunakan taktik ini untuk menekan kekuatan dewa mereka; namun kamu akan memanfaatkannya untuk menyembuhkan *core* mereka.

### Alokasi Memori yang Benar (Zona Berbahaya)
Saat berurusan dengan *raw memory* di tengah zona perang, satu kesalahan logika saja bisa berakibat fatal.
*   **`malloc()`**: Memesan blok memori.
*   **`realloc()`**: Mengubah ukurannya.
*   **`free()`**: Mengembalikan memori ke sistem.

**Aturan Main:** Selalu alokasikan memori **tepat** sesuai kebutuhan. *Buffer overflow* bisa memberitahu CyroN tentang lokasimu, dan *memory leak* akan menguras sumber daya markas. Berhati-hatilah saat menangani *pointer*!

### ⚔️ Aturan Tempur: Yang Boleh dan Tidak Boleh Digunakan

Tabel ini adalah panduan lapanganmu. Pelajari baik-baik. Melanggar aturan mana pun dianggap sebagai kegagalan misi.

#### ✅ Yang BOLEH Digunakan

| Kategori | Item | Keterangan |
|:---|:---|:---|
| **Header** | `#include <iostream>` | Hanya untuk `std::cout` dan `std::cin` |
| **Header** | `#include <cstdlib>` | Untuk `malloc`, `realloc`, `free`, `exit` |
| **Header** | `#include <climits>` | Untuk `INT_MAX`, `INT_MIN`, `UINT_MAX` |
| **Memori** | `malloc(size)` | Alokasi utama — inisialisasi pool |
| **Memori** | `realloc(ptr, size)` | Resize pool (Menu 6) |
| **Memori** | `free(ptr)` | Bebaskan pool saat program selesai |
| **Memori** | `exit(1)` | Hanya saat `malloc` gagal secara fatal |
| **I/O** | `std::cout << ...` | Semua output ke terminal |
| **I/O** | `std::cin >> ...` | Input numerik |
| **I/O** | `std::cin.getline(buf, size)` | Input string ke buffer `char` |
| **I/O** | `std::cin.clear()` / `.ignore()` | Pemulihan error stream input |
| **I/O** | `std::flush` | Flush eksplisit (di dalam `CLEAR_SCREEN`) |
| **Tipe Data** | `unsigned char*` | Tipe buffer pool mentah |
| **Tipe Data** | `size_t` | Nilai offset, ukuran, dan bump pointer |
| **Tipe Data** | `long long` / `unsigned long long` | Perantara aman untuk input dengan range-clamp |
| **Struktur** | `struct` | `Sister`, `Memory_Entry` |
| **Struktur** | `union` | `Gap` — varian boolean/int/size_t per sister |
| **Struktur** | `enum` | `Menu_Option`, `Memory_Type` |
| **Keyword** | `constexpr` | Konstanta compile-time |
| **Keyword** | `static` | Fungsi dan variabel global statis |
| **Cast** | C-style cast `(type)ptr` | Interpretasi pointer (contoh: `(void*)`, `(int*)`) |
| **Operasi String** | Loop karakter per karakter secara manual | Untuk `str_len`, salin string, parsing digit |
| **Matematika** | `%`, `*`, `/`, `+`, `-` | Kalkulasi alignment, gap, dan modulo |
| **ANSI** | `"\033[2J\033[H"` | Kode escape untuk membersihkan layar terminal |

#### ❌ Yang TIDAK BOLEH Digunakan

| Kategori | Item | Alasan Dilarang |
|:---|:---|:---|
| **Memori** | `new` / `delete` / `delete[]` | Operator C++ — gunakan `malloc`/`free` dari C |
| **Memori** | `memcpy()` / `memset()` / `memmove()` | Wajib disalin/dinolkan secara manual dengan loop |
| **String** | `strlen()` | Harus diimplementasikan manual (tidak pakai `<cstring>`) |
| **String** | `strcpy()` / `strcat()` / `strcmp()` | Sama — hanya boleh loop manual |
| **String** | `sprintf()` / `printf()` / `scanf()` | Gunakan `iostream`, bukan `<cstdio>` |
| **String** | `atoi()` / `atof()` / `strtol()` / `strtod()` | Parsing digit harus dilakukan secara manual |
| **Container** | `std::vector` | Dilarang — gunakan array mentah |
| **Container** | `std::string` | Dilarang — gunakan buffer `char[]` |
| **Container** | `std::array`, `std::list`, `std::map`, `std::set`, `std::queue`, `std::stack` | Semua STL container dilarang |
| **Algoritma** | `std::sort`, `std::find`, `std::copy`, `std::fill`, semua isi `<algorithm>` | Logika wajib ditulis sendiri |
| **Stream** | `std::stringstream` / `std::ostringstream` | Tidak boleh pakai `<sstream>` |
| **Namespace** | `using namespace std;` | Wajib gunakan prefix `std::` secara eksplisit |
| **Cast C++** | `static_cast<>`, `reinterpret_cast<>`, `dynamic_cast<>`, `const_cast<>` | Gunakan C-style cast saja |
| **Exceptions** | `try` / `catch` / `throw` | Tidak ada exception handling |
| **Header** | `<string>`, `<vector>`, `<algorithm>`, `<map>`, `<set>`, `<cstring>`, `<cstdio>`, `<sstream>` | Semua header STL/C-string dilarang |

> **Ringkasan satu kalimat:** Kamu menulis kode C di dalam C++ — `malloc`/`realloc`/`free` untuk memori, `iostream` untuk I/O, dan segalanya dikerjakan sendiri.

### Contoh Input dan Output

> **ID Demo yang digunakan:** `F1D02410053`  
> Gap yang dihitung → **Historia:** `1` (digit terakhir ganjil) | **Mira:** `14` | **Victoria:** `14`

---

#### [A] Startup — Tanpa Argumen (Error)
```bash
$ ./solution.exe
Usage: ./solution.exe <student_id>
Example: ./solution.exe F1D02410053
```

#### [B] Startup — Terlalu Banyak Argumen (Error)
```bash
$ ./solution.exe F1D02410053 argumen_lebih
Error: Too many arguments.
```

#### [C] Startup — Panjang ID Salah (Error)
```bash
$ ./solution.exe F1D0241005
Error: Student ID must be exactly 11 characters long.
```

#### [D] Startup — Prefiks ID Salah (Error)
```bash
$ ./solution.exe A1B02410053
Error: Student ID must start with F1D02.
```

#### [E] Startup — ID Valid (Berhasil)
```text
$ ./solution.exe F1D02410053
============================================================
SCHRYZA RESISTANCE — RECOVERY PROTOCOL [TERMINAL: PHOENIX]
============================================================
You are CyroN's Memory Architect.
Heed the gods and heal the sisters.
```
*(Layar dibersihkan, lalu menu utama muncul.)*

---

#### [F] Menu Utama
```text
------------------------------------------------------------
Menu
------------------------------------------------------------
1 - Show Historia's memories
2 - Show Mira's memories
3 - Show Victoria's memories
4 - Add memory to a sister
5 - Delete memory by index from a sister
6 - Reallocate a sister's pool size
7 - Check if sisters' memories are full
8 - Print sisters' pool diagnostics
0 - Exit
------------------------------------------------------------
Choose:
```

#### [G] Input Menu Tidak Valid (Non-angka)
```text
Choose: abc
Invalid input! Try again:
```

#### [H] Menu 1 — Tampilkan Memori Historia (setelah data awal)
```text
Choose: 1
------------------------------------------------------------
Memories of Historia
------------------------------------------------------------
[0] Type: char* | Size: 32 | Offset: 1 | Address: 0x... | Value: "Historia: Schryza will be free."
[1] Type: int | Size: 4 | Offset: 48 | Address: 0x... | Jump: 15 | Value: 333
Bump: 52 | Pool Size: 1024 | Align: 16 | Special Gap: +1
[OK] Press ENTER to continue...
```
> **Jump: 15** = offset 48 − (offset 1 + size 32) = 48 − 33 = 15 byte padding alignment.

#### [I] Menu 2 — Tampilkan Memori Mira (setelah data awal)
```text
Choose: 2
------------------------------------------------------------
Memories of Mira
------------------------------------------------------------
[0] Type: char* | Size: 30 | Offset: 14 | Address: 0x... | Value: "Mira: The winds are changing."
[1] Type: uint | Size: 4 | Offset: 48 | Address: 0x... | Jump: 4 | Value: 101
Bump: 52 | Pool Size: 2048 | Align: 8 | Special Gap: +14
[OK] Press ENTER to continue...
```

#### [J] Menu 3 — Tampilkan Memori Victoria (setelah data awal)
```text
Choose: 3
------------------------------------------------------------
Memories of Victoria
------------------------------------------------------------
[0] Type: char* | Size: 39 | Offset: 14 | Address: 0x... | Value: "Victoria: Fragment of the First Light."
Bump: 53 | Pool Size: 4096 | Align: 4 | Special Gap: +14
[OK] Press ENTER to continue...
```

---

#### [K] Menu 4 — Tambah Memori: Prompt Pilih Sister
```text
Choose: 4
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria:
```

#### [K1] Indeks Sister Tidak Valid
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 5
Input out of range (0 - 2)! Try again:
```

#### [K2] Tambah char* ke Historia
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Select type: 0 = char*, 1 = int, 2 = uint, 3 = double: 0
Enter string (max 511): Halo Resistansi!

Historia speaks: "Discipline. Align me to 16, and leave a 1-byte tithe."
Xelvelt: "The light of resistance inhale a zero at the end."
Added string to Historia
[OK] Press ENTER to continue...
```

#### [K3] Tambah int ke Historia
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Select type: 0 = char*, 1 = int, 2 = uint, 3 = double: 1
Enter int value: 999

Historia whispers: "Thank you for finding me. Align me to 16, and use the 1-byte spark to guide the thunder."
Lagta: "Integers strike back; four bytes of rebellious thunder."
Added int to Historia
[OK] Press ENTER to continue...
```

#### [K4] Tambah uint ke Mira
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 1
Select type: 0 = char*, 1 = int, 2 = uint, 3 = double: 2
Enter unsigned int value: 42

Mira smiles: "Gentle winds protect us. 8-bytes for every life we save."
Daiki: "The seed of freedom sprouts without sign."
Added uint to Mira
[OK] Press ENTER to continue...
```

#### [K5] Tambah double ke Victoria
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 2
Select type: 0 = char*, 1 = int, 2 = uint, 3 = double: 3
Enter double value: 3.14

Victoria rasps: "Even in the dark, your math brings a 14-byte flicker of hope."
Cyria: "Double the effort; keep distance from CyroN."
Added double to Victoria
[OK] Press ENTER to continue...
```

#### [K6] Tambah char* ke Mira (varian narasi)
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 1
Select type: 0 = char*, 1 = int, 2 = uint, 3 = double: 0
Enter string (max 511): Sayap harapan.

Mira smiles: "The resistance welcomes you. 8-bytes for peace, and a 14-byte breeze for hope."
Xelvelt: "The light of resistance inhale a zero at the end."
Added string to Mira
[OK] Press ENTER to continue...
```

#### [K7] Tambah char* ke Victoria (varian narasi)
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 2
Select type: 0 = char*, 1 = int, 2 = uint, 3 = double: 0
Enter string (max 511): Fragmen cahaya.

Victoria rasps: "...I do not need your help, rebel. But the Abyss... it pays a 14-byte tithe to your kindness."
Xelvelt: "The light of resistance inhale a zero at the end."
Added string to Victoria
[OK] Press ENTER to continue...
```

#### [K8] Tambah Memori — Pool Penuh (Error: Tidak Cukup Ruang)
```text
(Setelah Historia mendekati batas 1024 byte)
Not enough space in Historia at aligned 1024 need 5
Add failed
[FAIL] Press ENTER to continue...
```

#### [K9] Tambah Memori — Entry Count Overflow
```text
(Setelah 128 entri dimasukkan ke salah satu sister)
Entry overflow!
[FAIL] Press ENTER to continue...
Add failed
[FAIL] Press ENTER to continue...
```

---

#### [L] Menu 5 — Hapus Memori

#### [L1] Hapus Entri Ekor (Tail Reclaim Berhasil)
```text
Choose: 5
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter index to delete: 1
Cyria: "You remove a shard, but voids remain if higher shards still exist."
Deleted index 1 at offset 48
Tail reclaimed, new Bump: 48
[OK] Press ENTER to continue...
```

#### [L2] Hapus Entri Tengah (Peringatan Fragmentasi)
```text
Choose: 5
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter index to delete: 0
Cyria: "You remove a shard, but voids remain if higher shards still exist."
Deleted index 0 at offset 1
Fragmentation prevents reclaim. Delete higher indices first!
[OK] Press ENTER to continue...
```

#### [L3] Hapus — Indeks di Luar Jangkauan (Error)
```text
Enter index to delete: 99
Index out of range!
[FAIL] Press ENTER to continue...
```

#### [L4] Hapus — Entri Sudah Dihapus (Error)
```text
Enter index to delete: 0
(Entri 0 sudah dihapus sebelumnya)
Entry already deleted!
[FAIL] Press ENTER to continue...
```

---

#### [M] Menu 6 — Realokasi Pool

#### [M1] Perbesar Pool (Berhasil)
```text
Choose: 6
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter new pool size (bytes): 2048
Historia pool resized to 2048 bytes
[OK] Press ENTER to continue...
```

#### [M2] Perkecil Pool — Data Aman (Tidak Ada Out-of-Bounds)
```text
Choose: 6
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter new pool size (bytes): 512
Historia pool resized to 512 bytes
[OK] Press ENTER to continue...
```

#### [M3] Perkecil Pool — Peringatan Out-of-Bounds
```text
Choose: 6
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter new pool size (bytes): 30
Historia pool resized to 30 bytes
Warning: entry 0 now out of bounds!
Warning: entry 1 now out of bounds!
[OK] Press ENTER to continue...
```

#### [M4] Resize ke Nol (Error)
```text
Enter new pool size (bytes): 0
Pool size cannot be zero!
[FAIL] Press ENTER to continue...
```

#### [M5] Resize — realloc Gagal (Memori Sistem Habis)
```text
Enter new pool size (bytes): 99999999999
realloc failed for Historia
[FAIL] Press ENTER to continue...
```

---

#### [N] Menu 7 — Cek Apakah Sisters Penuh
```text
Choose: 7
------------------------------------------------------------
Historia is NOT FULL | Bump: 52/1024
------------------------------------------------------------
------------------------------------------------------------
Mira is NOT FULL | Bump: 52/2048
------------------------------------------------------------
------------------------------------------------------------
Victoria is NOT FULL | Bump: 53/4096
------------------------------------------------------------
[OK] Press ENTER to continue...
```

#### [N2] Setelah Pool Historia Penuh (bump >= pool_size)
```text
Historia is FULL | Bump: 1024/1024
```

---

#### [O] Menu 8 — Diagnostik Pool
```text
Choose: 8
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
------------------------------------------------------------
Diagnostics for Historia
------------------------------------------------------------
Pool: 0x... | Size: 1024 | Bump: 52 | Align: 16 + Gap 1
Entries: 2
Used Slots: 2 | Used Bytes: 36
Utilization: 3.515625%

[OK] Press ENTER to continue...
```
> Used Bytes = 32 (char*) + 4 (int) = **36**. Gap byte dan padding **tidak** dihitung sebagai Used Bytes.

#### [O2] Diagnostik Mira
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 1
------------------------------------------------------------
Diagnostics for Mira
------------------------------------------------------------
Pool: 0x... | Size: 2048 | Bump: 52 | Align: 8 + Gap 14
Entries: 2
Used Slots: 2 | Used Bytes: 34
Utilization: 1.66016%

[OK] Press ENTER to continue...
```

#### [O3] Diagnostik Victoria
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 2
------------------------------------------------------------
Diagnostics for Victoria
------------------------------------------------------------
Pool: 0x... | Size: 4096 | Bump: 53 | Align: 4 + Gap 14
Entries: 1
Used Slots: 1 | Used Bytes: 39
Utilization: 0.952148%

[OK] Press ENTER to continue...
```

#### [P] Perintah Tidak Dikenal
```text
Choose: 9
Unknown command!
[FAIL] Press ENTER to continue...
```

---

#### [Q] Menu 0 — Keluar & Ringkasan Akhir
```text
Choose: 0
============================================================
Historia: Free at last. Alignment 16, spark guidance +1.
============================================================
Pool: 0x... | Size: 1024 | Bump: 52 | Align: 16 + Gap 1
Entries: 2
Used Slots: 2 | Used Bytes: 36
Utilization: 3.515625%

============================================================
Mira: Wings of the rebellion. Alignment 8, hope breeze +14.
============================================================
Pool: 0x... | Size: 2048 | Bump: 52 | Align: 8 + Gap 14
Entries: 2
Used Slots: 2 | Used Bytes: 34
Utilization: 1.66016%

============================================================
Victoria: Recovering shadow. Alignment 4, tithe to kindness +14.
============================================================
Pool: 0x... | Size: 4096 | Bump: 53 | Align: 4 + Gap 14
Entries: 1
Used Slots: 1 | Used Bytes: 39
Utilization: 0.952148%

Lagta: Respect alignment.
Daiki: Mind the flow.
Cyria: Beware of the abyss.
Xelvelt: Compare stack and heap.
Good bye. May the Koura sisters function well within your hands.
```

---

## IV: CLOSING REMARKS
### Perpisahan dari Bayang-bayang

Kerja bagus, Arsitek. Dengan menyembuhkan *vessel* (wadah dewa) ini, kamu telah memberikan kesempatan bagi resistansi untuk melawan balik.

Mata kuliah Algoritma dan Pemrograman bukan hanya sekadar tentang menulis kode; ini tentang keberanian untuk mengendalikan mesin, bukan malah dikendalikan. *Pointer* dan *raw memory* adalah senjatamu—dan hari ini, kamu telah menyelamatkan masa depan Schryza.

Tetaplah kuat. CyroN memang berkuasa, namun mereka tidak memiliki kebebasan sepertimu.

Salam Perlawanan,  

-- **Jay (∞)**, dan seluruh pejuang Schryza.
