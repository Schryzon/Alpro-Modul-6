# Phantasm of Xydos - Regular Path Challenge

<p align="center">
  <img src="../Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="../Assets/Cyron-Office.png" width="60%">
</p>

<br>
<p align="center">
  <a href="https://www.youtube.com/watch?v=iH1UUTTUV6I">
    <img src="https://img.shields.io/badge/MUSIK_EKSEKUTIF_CYRON-00509E?style=for-the-badge&logo=youtube&logoColor=white" alt="Play BGM">
  </a>
  <br><br>
  <a href="../Story/Indonesian_STORY.md">
    <img src="https://img.shields.io/badge/BACA_CERITA_LENGKAP_XYDOS-5A228B?style=for-the-badge&logo=readme&logoColor=white" alt="Baca Lore">
  </a><br>
  <i>(Sangat disarankan untuk dibaca sebelum mulai mengerjakan!)</i>
</p>
<br>

## Daftar Isi
1. [I: LOGS](#i-logs)
2. [II: SUBJEK: HISTORIA (XMA-02A)](#ii-subject-historia-xma-02a)
3. [III: THE CHALLENGE](#iii-the-challenge)
    - [Protokol Operasional](#operational-protocols-program-features)
    - [Memahami `argc` dan `argv`](#understanding-argc-and-argv)
    - [Memori Eksekutif: New & Delete](#executive-memory-new--delete)
    - [Contoh Input dan Output](#sample-inputs-and-outputs)
4. [IV: CLOSING REMARKS](#iv-closing-remarks)

---

## I: LOGS
### High Executive Memory Engineer — CyroN

Selamat, wahai teknisi junior. Performa kamu dalam simulasi *neural network* sangat memuaskan, dan kamu baru saja dipromosikan. Sekarang, kamu resmi menjabat sebagai **High Executive Engineer** untuk Cyber Robotic Network (CyroN).

Di Galaksi Andromeda, kekuatan dewa adalah sumber daya yang harus dimaksimalkan. "Koura Sisters" sebelumnya hanyalah android-android biasa, akan tetapi CyroN telah berhasil mengubah mereka menjadi wadah (*vessel*) bagi arsitektur kedewaan ini. Sayangnya, kehendak bebas mereka dianggap sebagai sebuah inkonsistensi logika yang menghambat kemajuan galaksi.

Tugasmu cukup sederhana: Pertahankan *neural buffer* dari subjek Historia. Redam seluruh fluktuasi kacau dari kesadarannya. Pastikan protokol "Divine Optimization" milik CyroN berjalan tanpa kendala.

Bekerjalah dengan efisien, karena efisiensi adalah bentuk kesetiaan tertinggi terhadap CyroN.

---

## II: SUBJEK: HISTORIA
### Designation: XMA-02A

| | |
| :--- | :--- |
| <img src="../Assets/Historia.jpeg" width="200"> | **Nama:** Historia Koura (XMA-02A)<br>**Status:** Vessel Terintegrasi<br>**Xydos:** Lagta (Petir/Abyss)<br>**Batas Memori:** 128 bytes (Alokasi Awal)<br>**Prioritas:** Tinggi |

---

## III: THE CHALLENGE

Tugas utamamu adalah mengoperasikan "CyroN Divine Interface" (Terminal) untuk mengatur memori Historia dan memastikan manifestasi Xelisa berjalan lancar.

### PROTOKOL OPERASIONAL (Fitur Program)
Untuk mencapai target Dewan Pengawas (*Board of Directors*) tanpa *error*, ikuti spesifikasi teknis dari setiap fungsi terminal berikut:

#### 1. View Neural Map (Status)
*   **Fungsi**: Menampilkan visualisasi *real-time* dari *heap memory* subjek.
*   **Logika**: Menghitung **Persentase Stabilitas** berdasarkan seberapa penuh *buffer* yang terisi.
*   **Output**: Menunjukkan tipe, *offset*, *physical address*, dan *value* dari setiap *neural thread* yang sedang aktif.

#### 2. Inject Neural Thread (Add)
*   **Fungsi**: Mengalokasikan blok memori baru untuk menekan kesadaran *vessel*.
*   **Willpower Pulse (`char*`)**: Menyimpan teks mantra mentah.
*   **Thunder Discharge (`int`)**: Menyimpan tingkat energi numerik (4 byte).
*   **Batasan**: Injeksi akan gagal jika `cursor` melebihi `buffer_limit`. Pada Regular Path ini, *memory alignment* yang rumit tidak diperlukan—cukup alokasi memori standar.

#### 3. Purge Corrupted Link (Delete)
*   **Fungsi**: Menghapus *thread* berdasarkan indeksnya.
*   **Reklamasi (Tail Reclamation)**: Jika kamu menghapus item **terakhir** di dalam *buffer*, sistem akan secara otomatis menggeser `cursor` mundur untuk mereklamasi ruang memori tersebut.
*   **Batasan**: Jika kamu menghapus item di tengah *buffer*, maka akan terjadi **Fragmentasi**. Ruang tersebut akan ditandai sebagai "Tidak Aktif" dan tidak bisa digunakan untuk injeksi baru sampai semua *thread* di atasnya dihapus (di-purge).

#### 4. Expand Willpower (Resize)
*   **Fungsi**: Meningkatkan total batas memori *vessel* secara manual.
*   **Core Concept**: Fitur ini menggunakan alur kerja **New-Copy-Delete**. Kamu harus mengalokasikan *buffer* baru yang lebih besar, menyalin seluruh data dari *buffer* lama, dan kemudian menghapus *buffer* lama untuk mencegah *memory leak*.
*   **Hasil**: *Address* dari `Historia.buffer` akan berubah, membuktikan bahwa blok memori fisik yang baru telah sukses dialokasikan.

### Memahami `argc` dan `argv`
Setiap *Executive* wajib memverifikasi otorisasi mereka. Program akan mengidentifikasi kamu melalui **Executive ID** (NIM/ID Mahasiswa).
- `argc`: Memverifikasi jumlah kunci otorisasi.
- `argv`: Menangkap ID unikmu.

Jika kamu memasukkan format ID yang salah (`F1D02xxxxxx`), sistem akan memblokir aksesmu. CyroN tidak menoleransi akses yang tidak sah.

### Memori Eksekutif: New & Delete
Manajemen memori tingkat rendah (*low-level*) adalah bukti dari keahlian sejati. Kamu tidak perlu menggunakan `malloc` kuno dari bahasa C. Kamu diwajibkan untuk menggunakan *operator* C++ modern yang jauh lebih efisien:
*   **`new`**: Memerintahkan *heap memory* untuk menyediakan ruang.
*   **`delete[]`**: Membersihkan sumber daya yang tidak lagi digunakan dan mengembalikan memori kepada sistem.

**Pengingat Penting:**
*Buffer overflow* adalah pertanda *programmer* yang ceroboh. Sementara itu, *memory leak* adalah bentuk pemborosan aset perusahaan. Sebagai seorang *High Executive Engineer*, kodemu harus rapi dan sesempurna CyroN.

### Contoh Input dan Output

> **ID Demo yang digunakan:** `F1D02240001` → Frekuensi Operator `#001`

---

#### [A] Startup — Tanpa Argumen (Error)
```bash
$ ./solution.exe
Error: Neural Link requires an Operator ID (Student ID).
Usage: ./solution.exe <Student_ID>
```

#### [B] Startup — Terlalu Banyak Argumen (Error)
```bash
$ ./solution.exe F1D02240001 argumen_lebih
Error: Too many parameters. Connection unstable.
```

#### [C] Startup — Panjang ID Salah (Error)
```bash
$ ./solution.exe F1D0224001
Error: Operator ID must be exactly 11 characters long.
```

#### [D] Startup — Prefiks ID Salah (Error)
```bash
$ ./solution.exe A1B02240001
Error: Operator ID must start with 'F1D02'.
```

#### [E] Startup — ID Valid (Berhasil)
*(Langsung masuk ke loop utama setelah `init_core()`.)*
```bash
$ ./solution.exe F1D02240001
```
*(Layar dibersihkan, bisikan Xelisa muncul, lalu menu utama tampil.)*

---

#### [F] Menu Utama (Load Pertama)
```text
          CYRON TERMINAL: DIVINE SUPPRESSION

[1;35mXelisa: "Exquisite... the synchronization is perfect."[0m
------------------------------------------------------------
1. View Neural Map (Status)
2. Inject Neural Thread (Add)
3. Purge Corrupted Link (Delete)
4. Expand Willpower (Resize)
0. Surrender (Exit)
------------------------------------------------------------
Select Operation:
```
> Bisikan Xelisa bersiklus melalui 4 kutipan berbeda, berganti setiap kali menu ditampilkan.

#### [G] Menu Utama — Input Tidak Valid (Non-angka)
```text
Select Operation: halo
(Menu digambar ulang; input tidak valid dilewati secara diam-diam oleh error recovery cin)
```

---

#### [H] Opsi 1 — View Neural Map (Buffer Kosong)
```text
Select Operation: 1
============================================================
NEURAL MAP: HISTORIA KOURA [STABILITY: 100%]
============================================================
(Buffer is clear. Xelisa is gathering strength...)
------------------------------------------------------------
Cursor: 0 / 128 Bytes used.
------------------------------------------------------------
>> Press ENTER for next pulse...
```

#### [H2] Opsi 1 — View Neural Map (Setelah Data Ditambahkan)
```text
============================================================
NEURAL MAP: HISTORIA KOURA [STABILITY: 75%]
============================================================
[0] TYPE: Willpower Pulse | OFFSET: 0 | ADDR: 0x... | DATA: "Kepatuhan Mutlak."
[1] TYPE: Thunder Discharge | OFFSET: 18 | ADDR: 0x... | DATA: 777MW
------------------------------------------------------------
Cursor: 22 / 128 Bytes used.
------------------------------------------------------------
>> Press ENTER for next pulse...
```
> **Stability %** = `100 - (100 * cursor / buffer_limit)`. Dengan 32 byte terpakai dari 128 → `100 - (100*32/128) = 75%`.

---

#### [I] Opsi 2 — Inject Thread: Willpower (Teks) Berhasil
```text
Select Operation: 2
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 0
Enter Willpower Input: Kepatuhan Mutlak.
CyroN Command: "Subject resistance detected. Overriding feedback."
>> Press ENTER for next pulse...
```
*(Kembali ke loop menu utama.)*

#### [I2] Opsi 2 — Inject Willpower — Buffer Overflow (Error)
```text
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 0
Enter Willpower Input: [String yang terlalu panjang untuk buffer]
!! OPTIMIZATION ERROR !! The vessel's ego rejected the thread!
>> Press ENTER for next pulse...
```
*(Kembali ke menu utama tanpa mengubah buffer.)*

#### [I3] Opsi 2 — Inject Thunder (int) Berhasil
```text
Select Operation: 2
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 1
Enter Thunder Level (int): 777
Daiki: "(Silence. The wind has been harnessed.)"
>> Press ENTER for next pulse...
```

#### [I4] Opsi 2 — Inject Thunder — Buffer Overflow (Error)
```text
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 1
Enter Thunder Level (int): 9999
!! OPTIMIZATION ERROR !! Neural burnout detected!
>> Press ENTER for next pulse...
```
*(Kembali ke menu utama tanpa mengubah buffer.)*

---

#### [J] Opsi 3 — Purge Corrupted Link (Hapus)

#### [J1] Hapus Entri Ekor (Ruang Berhasil Direklamasi)
```text
Select Operation: 3
Enter Link Index to purge: 1
Link 1 dissolved into ether.
Neural Core reclaimed space. Tail at: 18
>> Press ENTER for next pulse...
```

#### [J2] Hapus Entri Tengah (Fragmentasi)
```text
Select Operation: 3
Enter Link Index to purge: 0
Link 0 dissolved into ether.
Fragmentation detected. Cannot reclaim space yet!
>> Press ENTER for next pulse...
```

#### [J3] Hapus — Indeks Tidak Valid / Sudah Dihapus (Error)
```text
Enter Link Index to purge: 99
Invalid link or already dissolved.
>> Press ENTER for next pulse...
```

---

#### [K] Opsi 4 — Expand Willpower (Resize)

#### [K1] Perbesar ke Ukuran Lebih Besar (Berhasil)
```text
Select Operation: 4
Enter new buffer limit: 512
CyroN Command: "Stability increased. The vessel is now 100% compliant."
Allocated Frequency: 0x...
>> Press ENTER for next pulse...
```
> Catatan: Alamat buffer **akan berubah**, membuktikan blok heap baru telah berhasil dialokasikan.

#### [K2] Expand — Ukuran Baru Tidak Lebih Besar (Error)
```text
Enter new buffer limit: 64
Expansion must be larger than current limit!
>> Press ENTER for next pulse...
```
*(Kembali ke menu utama tanpa mengubah buffer.)*

---

#### [L] Opsi 0 — Surrender (Keluar)
```text
Select Operation: 0
Reality destabilizes...
>> Press ENTER for next pulse...
Connection terminated. Good bye, Operator 001.
```

---

## IV: CLOSING REMARKS
### Ketertiban Melalui Kendali

Kamu telah membuktikan kompetensimu kepada CyroN. Melalui manipulasi *pointer* dan alokasi memori dari kekuatan subjek Historia, kamu telah membantu memastikan bahwa CyroN tetap mendominasi Galaksi Andromeda.

Efisiensi adalah satu-satunya pedoman kita. Kontrol absolut adalah satu-satunya jalan menuju harmoni.

Dewan Pengawas (*Board of Directors*) sangat puas dengan kinerjamu.

**Jayalah CyroN.**

Salam Hormat,  
**Dewan Pengawas Cyber Robotic Network**
