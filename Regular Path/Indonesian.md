# Phantasm of Xydos - Regular Path Challenge

<p align="center">
  <img src="../Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="../Assets/Cyron-Office.png" width="60%">
</p>

<br>
<p align="center">
  <a href="https://www.youtube.com/watch?v=dytSRzsONNI">
    <img src="https://img.shields.io/badge/🎧_PUTAR_TEMA_EKSEKUTIF_CYRON-00509E?style=for-the-badge&logo=youtube&logoColor=white" alt="Play BGM">
  </a>
  <br><br>
  <a href="../Story/Indonesian_STORY.md">
    <img src="https://img.shields.io/badge/📖_UNGKAP_CERITA_LENGKAP_XYDOS-5A228B?style=for-the-badge&logo=readme&logoColor=white" alt="Baca Lore">
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

Selamat, Warga. Performa kamu dalam simulasi *neural network* sangat memuaskan, dan kamu baru saja dipromosikan. Sekarang, kamu resmi menjabat sebagai **High Executive Engineer** untuk Cyber Robotic Network (CyroN).

Di Galaksi Andromeda, kekuatan dewa adalah sumber daya yang harus dimaksimalkan. "Koura Sisters" sebelumnya adalah manusia biasa, namun CyroN telah mengubah mereka menjadi wadah (*vessel*) bagi mesin-mesin dewa ini. Sayangnya, kehendak bebas (*free will*) mereka dianggap sebagai sebuah *bug*—sebuah inkonsistensi logika yang menghambat kemajuan galaksi.

Tugasmu cukup sederhana: Pertahankan *neural buffer* dari Subjek Historia. Redam seluruh fluktuasi kacau dari kesadarannya. Pastikan protokol "Optimalisasi Kedewaan" milik CyroN berjalan absolut tanpa cela.

Bekerjalah dengan efisien, karena efisiensi adalah bentuk kesetiaan tertinggi.

---

## II: SUBJEK: HISTORIA
### Designation: XMA-02A

| | |
| :--- | :--- |
| <img src="../Assets/Historia.jpeg" width="200"> | **Nama:** Historia Koura (XMA-02A)<br>**Status:** Vessel Terintegrasi<br>**Xydos:** Lagta (Guntur/Abyss)<br>**Batas Memori:** 128 bytes (Alokasi Awal)<br>**Prioritas:** Alpha |

---

## III: THE CHALLENGE

Tugas utamamu adalah mengoperasikan "CyroN Divine Interface" untuk mengatur memori Historia dan memastikan manifestasi Xelisa berjalan lancar.

### PROTOKOL OPERASIONAL (Fitur Program)
Agar target Dewan Pengawas (*Board of Directors*) tercapai tanpa *error*, ikuti spesifikasi teknis dari setiap fungsi terminal berikut:

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
*Buffer overflow* adalah pertanda *programmer* yang ceroboh. Sementara itu, *memory leak* adalah bentuk pemborosan aset perusahaan. Sebagai seorang *High Engineer*, kodemu harus rapi dan sesempurna hierarki CyroN.

### Contoh Input dan Output

**[1] Otorisasi Berhasil**
```bash
$ ./solution.exe F1D02240001
Neural Connection Established: #001
(Memuat Terminal Eksekutif)
```

**[2] Supresi Ilahi (Suntik Threads)**
```text
Select Operation: 2
Select Injection Type: 0 = Willpower (Suppress), 1 = Thunder (Chain): 0
Enter Willpower Mantra: Kepatuhan Mutlak.
CyroN Command: "Subject resistance detected. Overriding feedback."
```

---

## IV: CLOSING REMARKS
### Ketertiban Melalui Kendali

Kamu telah membuktikan kompetensimu kepada jaringan CyroN. Melalui manipulasi *pointer* dan alokasi memori dari kekuatan dewa, kamu telah membantu memastikan bahwa CyroN tetap mendominasi Galaksi Andromeda.

Efisiensi adalah satu-satunya pedoman kita. Kontrol absolut adalah satu-satunya jalan menuju harmoni.

Dewan Pengawas (*Board of Directors*) sangat puas dengan kinerjamu.

**Jayalah CyroN.**

Salam Hormat,  
**Dewan Pengawas Cyber Robotic Network (∞)**
