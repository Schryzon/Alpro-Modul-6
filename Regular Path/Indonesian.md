# Phantasm of Xydos - Regular Path Challenge

<p align="center">
  <img src="../Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="../Assets/Cyron-Office.png" width="60%">
</p>

## Daftar Isi
1. [I: LOG](#i-log)
2. [II: SUBJEK: HISTORIA (XMA-02A)](#ii-subject-historia-xma-02a)
3. [III: TUGAS](#iii-the-assignment)
    - [Protokol Operasional](#operational-protocols)
    - [Alur Kerja Memory Engineering](#memory-engineering-workflow)
    - [Output yang Diharapkan](#expected-output)
4. [IV: KEPATUHAN FINAL](#iv-final-compliance)

---

## I: LOG
### High Executive Memory Engineer — CyroN

Selamat, Warga. Performa Anda dalam simulasi penyusunan neural telah membuahkan promosi bagi Anda. Anda sekarang adalah seorang **High Executive Engineer** untuk Cyber Robotic Network (CyroN).

Di Galaksi Andromeda, ketuhanan adalah sumber daya yang harus dioptimalkan. "Koura Sisters" dulunya adalah manusia, namun CyroN telah mengangkat mereka menjadi wadah mesin ketuhanan. Namun, kehendak bebas mereka adalah bug— inkonsistensi logis yang memperlambat kemajuan galaksi.

Tugas Anda sederhana: Pertahankan neural buffer dari Subjek Historia. Tekan fluktuasi kacau dari kesadarannya. Pastikan protokol "Optimalisasi Ilahi" dari CyroN tetap absolut.

Bekerjalah dengan giat. Efisiensi adalah bentuk pemujaan tertinggi.

---

## II: SUBJEK: HISTORIA
### Designation: XMA-02A

| | |
| :--- | :--- |
| <img src="../Assets/Historia.jpeg" width="200"> | **Nama:** Historia Koura (XMA-02A)<br>**Status:** Wadah Terintegrasi<br>**Xydos:** Lagta (Guntur/Abyss)<br>**Ambang Memori:** 1024 bytes (Alokasi Awal)<br>**Prioritas:** Alpha |

---

## III: THE CHALLENGE

Kamu akan mengoperasikan "CyroN Divine Interface" untuk mengelola memori Historia dan memastikan manifestasi penuh Xelisa.

### PROTOKOL OPERASIONAL (Fitur Program)
Untuk memastikan tujuan Dewan Pengawas tercapai tanpa kesalahan, ikuti spesifikasi teknis untuk setiap fungsi terminal berikut:

#### 1. View Neural Map (Status)
*   **Kemampuan**: Memberikan visualisasi real-time dari heap memory subjek.
*   **Logika**: Menghitung **Persentase Stabilitas** berdasarkan seberapa banyak buffer yang terisi.
*   **Visibilitas**: Menampilkan tipe, offset, alamat fisik, dan nilai dari setiap *neural thread* yang aktif.

#### 2. Inject Neural Thread (Add)
*   **Kemampuan**: Mengalokasikan blok memori baru untuk menekan vessel.
*   **Willpower Pulse (`char*`)**: Menyimpan mantra teks mentah.
*   **Thunder Discharge (`int`)**: Menyimpan tingkat energi numerik (4 byte).
*   **Batasan**: Injeksi akan gagal jika `cursor` melebihi `buffer_limit`. Tidak ada *alignment* memori yang diperlukan di Regular Path—hanya alokasi mentah yang murni.

#### 3. Purge Corrupted Link (Delete)
*   **Kemampuan**: Melarutkan thread berdasarkan indeksnya.
*   **Reklamasi (Tail Reclamation)**: Jika kamu menghapus item *terakhir* di buffer, sistem secara otomatis menggeser `cursor` kembali untuk mengambil ruang tersebut.
*   **Batasan**: Menghapus item di tengah buffer menciptakan **Fragmentasi**. Ruang tersebut ditandai sebagai "Tidak Aktif" tetapi tidak dapat digunakan untuk injeksi baru sampai semua thread di atasnya dibersihkan.

#### 4. Expand Willpower (Resize)
*   **Kemampuan**: Meningkatkan total batas memori vessel secara manual.
*   **Pelajaran Utama**: Ini menggunakan alur kerja **New-Copy-Delete**. Kamu harus mengalokasikan buffer baru yang lebih besar, menyalin semua data yang ada dari buffer lama, dan kemudian menghapus buffer lama untuk mencegah kebocoran memori.
*   **Hasil**: Alamat `Historia.buffer` akan berubah, membuktikan bahwa blok memori fisik yang baru telah diamankan.

### Memahami `argc` dan `argv`
Setiap Eksekutif harus memverifikasi otorisasi mereka. Programmu akan mengidentifikasimu melalui **Executive ID** (NIM/ID Mahasiswa).
- `argc`: Memverifikasi jumlah kunci otorisasi.
- `argv`: Menangkap ID unikmu.

Jika kamu gagal memberikan format ID yang benar (`F1D02xxxxxx`), sistem akan mengunci aksesmu. CyroN tidak menoleransi akses yang tidak sah.

### Memori Eksekutif: New & Delete
Manajemen memori tingkat rendah adalah bukti kekuatan tertinggi. Kamu tidak akan menggunakan `malloc` yang kuno. Kamu akan menggunakan operator C++ modern yang efisien:
*   **`new`**: Memerintah *heap* untuk menyerahkan ruang.
*   **`delete[]`**: Membersihkan sisa-sisa yang tidak perlu dan mengklaim kembali kekuatan.

**Pengingat untuk Keunggulan:**
*Buffer overflow* adalah tanda dari pikiran yang ceroboh. *Memory leak* adalah pemborosan sumber daya korporasi. Sebagai High Engineer, kodemu harus sempurna seperti hierarki CyroN.

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

Kamu telah membuktikan nilaimu kepada jaringan. Dengan menguasai manipulasi *pointer* dan alokasi kekuatan dewa mentah, kamu telah memastikan bahwa CyroN tetap menjadi kekuatan dominan di galaksi Andromeda.

Efisiensi adalah satu-satunya moralitas kita. Kendali adalah satu-satunya harmoni kita.

Dewan pengawas puas dengan kemajuanmu.

**Kejayaan bagi CyroN.**

Salam Hormat,  
**Dewan Pengawas Cyber Robotic Network (∞)**
