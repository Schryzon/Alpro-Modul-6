# Phantasm of Xydos - Challenge (Schryza Resistance)

<p align="center">
  <img src="../Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="../Assets/Schryza-Resistance.png" width="60%">
</p>

<br>
<p align="center">
  <a href="https://www.youtube.com/watch?v=Aa7SyxcKTbs">
    <img src="https://img.shields.io/badge/PUTAR_TEMA_BATTLE_SCHRYZA-8B0000?style=for-the-badge&logo=youtube&logoColor=white" alt="Play BGM">
  </a>
  <br><br>
  <a href="../Story/Indonesian_STORY.md">
    <img src="https://img.shields.io/badge/UNGKAP_CERITA_LENGKAP_XYDOS-5A228B?style=for-the-badge&logo=readme&logoColor=white" alt="Baca Lore">
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

Sebelum perang, CyroN adalah penyelamat yang memulihkan planet-planet. Sekarang, di bawah pengaruh dewi Xelisa yang terkorupsi, CyroN berubah menjadi kerajaan otoriter yang menjadikan kekuatan dewa sebagai senjata. Kamu adalah seorang Arsitek Memori yang sedang dalam pelarian. Muak melihat "Optimalisasi Kedewaan" mereka yang dingin, kamu berhasil kabur dari markas emas mereka menuju kawasan limbah industri Schryza yang kacau.

Di markas pemberontak yang tersembunyi, kamu bertemu dengan para penyintas: Historia dan Mira Koura. Mereka adalah unit pemberontak yang memilih kebebasan daripada tunduk pada hierarki CyroN. *Neural core* mereka rusak, dan *pointer* memori mereka terfragmentasi parah akibat pelarian itu.

*"Terima kasih... sudah membawa harapan,"* bisik Historia saat kamu menyalakan terminal.

Bahkan Victoria, sang kakak tertua yang telah diubah menjadi senjata "Divine Countermeasure", berhasil dibawa oleh para pemberontak. Mereka berharap keahlian arsitekturalmu bisa menyembuhkannya—sebuah hal yang gagal dilakukan oleh sistem optimalisasi CyroN.

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

Kerja bagus, Arsitek. Dengan menyembuhkan *vessel* (wadah dewa) ini, kamu telah memberikan kesempatan bagi resistansi untuk melawan balik.

Mata kuliah Algoritma dan Pemrograman bukan hanya sekadar tentang menulis kode; ini tentang keberanian untuk mengendalikan mesin, bukan malah dikendalikan. *Pointer* dan *raw memory* adalah senjatamu—dan hari ini, kamu telah menyelamatkan masa depan Schryza.

Tetaplah kuat. CyroN memang berkuasa, namun mereka tidak memiliki kebebasan sepertimu.

Salam Perlawanan,  

-- **Jay (∞)**, dan seluruh pejuang Schryza.
