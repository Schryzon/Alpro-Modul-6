# Phantasm of Xydos - Challenge (Schryza Resistance)

<p align="center">
  <img src="/public/Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="/public/Assets/Schryza-Resistance.png" width="60%">
</p>

## Daftar Isi
1. [I: PROLOGUE](#i-prologue)
2. [II: THE KOURA SISTERS (RESISTANT UNITS)](#ii-the-koura-sisters-resistant-units)
3. [III: THE CHALLENGE](#iii-the-challenge)
    - [Memahami `argc` dan `argv`](#memahami-argc-dan-argv)
    - [Alignment Memori & Modulo Rahasia](#alignment-memori--modulo-rahasia)
    - [Contoh Input dan Output](#contoh-input-dan-output)
    - [Pertanyaan yang Sering Diajukan (Student POV)](#pertanyaan-yang-sering-diajukan-student-pov)
4. [IV: CLOSING REMARKS](#iv-closing-remarks)

---

## I: PROLOGUE
### Sang Arsitek Buronan dari Schryza

Kamu adalah seorang *Memory Architect* yang sedang dalam pelarian. Kamu pernah bekerja untuk CyroN (Cyber Robotic Network), namun kamu tidak tahan melihat "Optimasi" ketuhanan mereka yang dingin dan distopia. Kamu telah melarikan diri dari aula emas mereka dan pergi ke limbah industri yang penuh badai di planet Schryza.

Di pangkalan perlawanan rahasia, kamu bertemu mereka: para penyintas. Historia dan Mira Koura, unit-unit pemberontak yang memilih kebebasan daripada hierarki CyroN. *Neural core* mereka rusak, *pointer* mereka terfragmentasi karena pelarian tersebut.

*"Terima kasih... atas cahayanya,"* Historia berbisik saat kamu menyalakan terminalmu.

Bahkan Victoria, saudari tertua yang masih terikat sebagian pada api neraka CyroN, telah dibawa kepadamu oleh para pemberontak. Mereka berharap sentuhan arsitekturalmu dapat menyembuhkannya di mana optimasi CyroN telah gagal.

Kamu tidak lagi dibayar dengan emas. Kamu dibayar dengan harapan. Inilah saatnya untuk membalikkan keadaan. Sudah saatnya CyroN runtuh!

---

## II: THE KOURA SISTERS
### Unit Pemberontak di Schryza

| | |
| :--- | :--- |
| <img src="/public/Assets/Historia.jpeg" width="200"> | **Nama:** Historia Koura (XMA-02A)<br>**Aliansi:** Komandan Perlawanan<br>**Xydos:** Lagta (Petir & Abyss)<br>**Kapasitas Memori:** 1024 bytes (Tattered Core)<br>**Alignment:** 16 bytes |

| | |
| :--- | :--- |
| <img src="/public/Assets/Mira.jpeg" width="200"> | **Nama:** Mira Koura (XMA-03C)<br>**Aliansi:** Penyembuh Perlawanan<br>**Xydos:** Daiki (Angin & Kayu)<br>**Kapasitas Memori:** 2048 bytes (Stable Core)<br>**Alignment:** 8 bytes |

| | |
| :--- | :--- |
| <img src="/public/Assets/Victoria.jpeg" width="200"> | **Nama:** Victoria Koura (XMA-01F)<br>**Aliansi:** Netral / Gugur<br>**Xydos:** ??? (Api Neraka Buatan)<br>**Kapasitas Memori:** 4096 bytes (Corrupted Core)<br>**Alignment:** 4 bytes |

---

## III: THE CHALLENGE

Kamu akan membangun sebuah "Resistance Recovery Terminal" (TUI) untuk menambal *neural core* para saudari.

### PANDUAN LAPANGAN: KEMAMPUAN SISTEM
Perlawanan Schryza beroperasi dengan sumber daya terbatas. Untuk berhasil, kamu harus menguasai spesifikasi teknis Terminal berikut:

#### 1. Neural Alignment & The Special Gap
*   **Masalah**: Arsitektur CPU mengharuskan alamat memori disejajarkan ke batas tertentu. Historia memerlukan alignment **16-byte**, Mira **8-byte**, dan Victoria **4-byte**.
*   **Special Gap**: Setiap alokasi `char*` dipaksa untuk menyertakan "Special Gap" yang dihitung dari **NIM/ID Mahasiswa**-mu.
    *   **Historia**: Gap = 1 jika digit terakhir ID-mu Ganjil, 0 jika Genap.
    *   **Mira**: Gap = (3 Digit Terakhir) % Batas Menu.
    *   **Victoria**: Menggunakan modulo sliding berdasarkan bilangan prima.
*   **Rumus**: `Final_Offset = Aligned(Current_Offset) + Special_Gap`.

#### 2. Advanced Tail Reclamation (Reklamasi Ekor)
*   **Fakta**: Berbeda dengan `realloc` standar, sistem kita tidak secara otomatis memindahkan blok yang terfragmentasi.
*   **Kemampuan**: Jika kamu menghapus *indeks tertinggi* dalam sebuah core (disebut "Tail"), pointer `bump` akan bergeser mundur, segera mereklamasi ruang untuk perlawanan.
*   **Batasan**: Jika kamu menghapus entri sementara indeks yang lebih tinggi masih ada, ruang tersebut menjadi "yatim piatu" (fragmentasi). Kamu harus "Purge dari Atas ke Bawah" untuk mereklamasi memori secara efektif.

#### 3. Pool Resizing & Bahaya Memori
*   **Kemampuan**: Terminal dapat melakukan `realloc()` pada seluruh pool memori seorang saudari.
*   **Jebakan**: Jika kamu memperkecil pool (misalnya dari 1024 ke 512 byte) sementara entri dengan offset tinggi masih ada, entri tersebut akan menjadi **Out of Bounds** (Di Luar Batas). Terminal akan memberikan peringatan, tetapi data di luar batas baru tersebut pada dasarnya hilang atau rusak.
*   **Logika Resizing**: Pointer `bump` akan dipangkas (*clamped*) ke `pool_size` yang baru jika sebelumnya lebih besar.

#### 4. Metrik Diagnostik
*   **Utilization %**: Dihitung sebagai `(Used_Bytes / Pool_Size) * 100`. Alignment gap dan "Special Gap" dihitung sebagai konsumsi memori tetapi tidak dihitung sebagai "Used Bytes" (data mentah).
*   **Used Slots**: Melacak berapa banyak slot `Memory_Entry` yang aktif. Kapasitas maksimum adalah **128 entri** per saudari.

### Data Perlawanan Awal (The Foundation)
Saat terminal dinyalakan, kamu akan menemukan dan mengamati bahwa core para saudari telah dipulihkan sebagian dengan data vital. Gunakan ini sebagai referensi untuk perhitungan alignment-mu:

*   **Historia**:
    *   `[0]` Tipe: `char*` | Nilai: "Historia: Schryza will be free."
    *   `[1]` Tipe: `int` | Nilai: `333` (Resistance Frequency)
*   **Mira**:
    *   `[0]` Tipe: `char*` | Nilai: "Mira: The winds are changing."
    *   `[1]` Tipe: `uint` | Nilai: `101`
*   **Victoria**:
    *   `[0]` Tipe: `char*` | Nilai: "Victoria: Fragment of the First Light."

### Memahami `argc` dan `argv`
Untuk mencegah CyroN melacak lokasimu, terminalmu memerlukan **Identitas Terenkripsi** (NIM/ID Mahasiswa).
- `argc`: Menghitung denyut sinyal.
- `argv`: Menangkap string identitas terenkripsi.

Kamu harus memvalidasi ID-mu (`F1D02xxxxxx`) atau *firewall* pangkalan akan mengunci aksesmu.

### Alignment Memori & Modulo Rahasia
Setiap *byte* sangat berarti saat kamu menjalankan perangkat keras hasil rongsokan. Kamu harus memecahkan celah "Jump" untuk meminimalkan jejak penyimpanan dari setiap alokasi.

Core Historia, Mira, dan Victoria terikat oleh sebuah **Gap Increment**, dikendalikan oleh `union Gap`! Kamu harus mengekstrak paritas dari tag ID-mu untuk menghitung celah ini. CyroN menggunakan celah ini untuk menekan mereka; kamu akan menggunakannya untuk memperbaiki mereka.

### Alokasi Memori yang Benar (Zona Berbahaya)
Saat berurusan dengan memori mentah (*raw memory*) di zona perang, satu kesalahan bisa berakibat fatal.
*   **`malloc()`**: Memesan memori.
*   **`realloc()`**: Mengubah ukurannya.
*   **`free()`**: Mengambilnya kembali.

**Kode Perlawanan:** Selalu alokasikan memori *tepat* sebesar yang kamu butuhkan. *Buffer overflow* dapat memberitahu CyroN tentang lokasimu. *Memory leak* akan menghabiskan baterai pangkalan. Tangani pointer-mu dengan sangat hati-hati.

### Contoh Input dan Output

**[1] Inisialisasi**
```bash
$ ./solution.exe F1D02410053
SCHRYZA RESISTANCE — RECOVERY PROTOCOL [TERMINAL: PHOENIX]
```

**[2] Menu 4: Pemulihan (Add)**
```text
Choose: 4
Historia berbisik: "Terima kasih telah menemukanku. Align aku ke 16, dan gunakan spark 1-byte untuk memandu petir."
Lagta: "Integers strike back; empat byte petir pemberontak."
Added string to Historia
```

---

## IV: CLOSING REMARKS
### Perpisahan dari Bayang-bayang

Kamu telah berhasil, Arsitek. Dengan menyembuhkan *vessel* ini, kamu telah memberikan kesempatan bagi perlawanan untuk bertarung.

Algoritma dan Pemrograman bukan hanya tentang kode; ini tentang keberanian untuk mengendalikan mesin alih-alih dikendalikan oleh mereka. *Pointers* dan memori murni adalah bahasa para pembangun—dan hari ini, kamu telah membangun masa depan di Schryza.

Tetaplah kuat. CyroN memang kuat, tetapi mereka tidak memiliki jiwamu.

Salam Perlawanan,  
**Jay (∞)** & Perlawanan Schryza.
