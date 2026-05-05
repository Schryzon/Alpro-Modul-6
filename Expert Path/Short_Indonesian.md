# Laporan Praktikum Modul 6 — Expert Path
## Schryza Recovery Terminal: Manajemen Pool Memori untuk Koura Sisters

### Deskripsi Program

Program ini merupakan implementasi terminal berbasis teks bernama **Schryza Recovery Terminal** [RECOVERY PROTOCOL: PHOENIX] yang berperan sebagai sistem manajemen memori multi-pool untuk tiga karakter: Historia, Mira, dan Victoria Koura. Setiap karakter memiliki pool memori terpisah yang dikelola menggunakan fungsi C standar `malloc()`, `realloc()`, dan `free()` dari `<cstdlib>`.

Program menerima satu argumen berupa NIM mahasiswa melalui `argv[1]` dalam format `F1D02xxxxxx`. NIM divalidasi formatnya, lalu tiga digit terakhirnya diekstrak secara matematis untuk menghitung nilai **Special Gap** yang unik bagi masing-masing karakter. Gap ini memengaruhi tata letak memori dan diimplementasikan melalui `union Gap` yang menyimpan representasi berbeda per karakter.

---

### Inisialisasi dan Data Awal

Setelah validasi NIM berhasil, program menginisialisasi tiga pool memori dengan `malloc()` menggunakan ukuran yang telah ditetapkan: Historia (1024 byte), Mira (2048 byte), Victoria (4096 byte). Setiap pool memiliki *alignment requirement* yang berbeda serta gap yang dihitung dari NIM. Segera setelah inisialisasi, program mengisi data awal (*seed data*) pada ketiga pool sebagai fondasi demonstrasi.

Setelah data awal tersedia, program menampilkan menu utama yang menyediakan delapan operasi.

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
*Gambar 6.1 — Banner inisialisasi dan menu utama Schryza Recovery Terminal*

---

### Fitur 1–3: Tampilkan Memori (Memory Alignment)

Inti teknis program ini adalah implementasi **memory alignment**. Setiap alokasi ditempatkan pada offset yang disejajarkan dengan kelipatan tertentu menggunakan fungsi `align_up()`. Khusus untuk alokasi bertipe `char*`, program menambahkan **Special Gap** ekstra sebelum data ditempatkan — gap ini dihitung dari NIM mahasiswa.

Dengan NIM `F1D02410053` (tiga digit terakhir: `053`):
- **Historia** (align 16, gap `1`): alokasi `char*` dimulai di offset `align_up(bump, 16) + 1`
- **Mira** (align 8, gap `14`): alokasi `char*` dimulai di offset `align_up(bump, 8) + 14`
- **Victoria** (align 4, gap `14`): alokasi `char*` dimulai di offset `align_up(bump, 4) + 14`

Kolom **Jump** pada output menunjukkan selisih antara akhir entri sebelumnya dan awal entri saat ini — inilah padding alignment yang sebenarnya terjadi di memori.

```text
Choose: 1
------------------------------------------------------------
Memories of Historia
------------------------------------------------------------
[0] Type: char* | Size: 32 | Offset: 1 | Address: 0x... | Value: "Historia: Schryza will be free."
[1] Type: int   | Size: 4  | Offset: 48 | Address: 0x... | Jump: 15 | Value: 333
Bump: 52 | Pool Size: 1024 | Align: 16 | Special Gap: +1
[OK] Press ENTER to continue...
```
*Gambar 6.2 — Tampilan memori Historia menunjukkan offset alignment, Special Gap, dan nilai Jump antar entri*

---

### Fitur 4: Tambah Memori

Saat menambahkan entri baru, program terlebih dahulu menghitung offset setelah alignment dan menambahkan gap apabila tipenya `char*`. Setelah data disalin byte-per-byte ke pool, narasi karakter ditampilkan sebagai respons kontekstual.

```text
Choose: 4
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 1
Select type: 0 = char*, 1 = int, 2 = uint, 3 = double: 0
Enter string (max 511): Wings of hope.

Mira smiles: "The resistance welcomes you. 8-bytes for peace, and a 14-byte breeze for hope."
Xelvelt: "The light of resistance inhale a zero at the end."
Added string to Mira
[OK] Press ENTER to continue...
```
*Gambar 6.3 — Penambahan entri char\* ke Mira disertai narasi gap dan null terminator*

Apabila pool tidak memiliki ruang yang cukup untuk menampung data baru setelah alignment dan gap dihitung, operasi dibatalkan dan program menampilkan pesan kegagalan.

```text
Not enough space in Historia at aligned 1024 need 5
Add failed
[FAIL] Press ENTER to continue...
```
*Gambar 6.4 — Kegagalan penambahan memori akibat pool habis (pool full error)*

---

### Fitur 5: Hapus Memori dan Tail Reclamation

Penghapusan memori dilakukan dengan menonaktifkan entri (`used = false`) tanpa menghapus data fisiknya dari pool. Apabila entri yang dihapus adalah entri aktif terakhir (ekor), pointer `bump` direklamasi mundur ke posisi akhir entri aktif tertinggi yang tersisa.

```text
Choose: 5
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter index to delete: 1
Cyria: "You remove a shard, but voids remain if higher shards still exist."
Deleted index 1 at offset 48
Tail reclaimed, new Bump: 48
[OK] Press ENTER to continue...
```
*Gambar 6.5 — Tail Reclamation berhasil: bump bergeser mundur setelah entri ekor dihapus*

Apabila masih ada entri aktif dengan indeks yang lebih tinggi dari entri yang dihapus, terjadi fragmentasi dan `bump` tidak dapat direklamasi.

```text
Enter index to delete: 0
Cyria: "You remove a shard, but voids remain if higher shards still exist."
Deleted index 0 at offset 1
Fragmentation prevents reclaim. Delete higher indices first!
[OK] Press ENTER to continue...
```
*Gambar 6.6 — Fragmentasi pada pool Historia: penghapusan entri non-ekor tidak dapat mereklamasi bump*

---

### Fitur 6: Realokasi Pool

Pool memori dapat diubah ukurannya menggunakan `realloc()`. Apabila pool diperkecil dan entri yang sudah ada berada di luar batas ukuran baru, program menampilkan peringatan per entri yang terdeteksi *out-of-bounds*.

```text
Choose: 6
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter new pool size (bytes): 30
Historia pool resized to 30 bytes
Warning: entry 0 now out of bounds!
Warning: entry 1 now out of bounds!
[OK] Press ENTER to continue...
```
*Gambar 6.7 — Peringatan out-of-bounds saat pool diperkecil melampaui offset entri yang sudah ada*

---

### Fitur 8: Diagnostik Pool

Menu Diagnostics menampilkan statistik lengkap penggunaan pool: jumlah slot terpakai, total byte yang terisi oleh data aktif, dan persentase utilisasi pool. Perlu diperhatikan bahwa byte alignment padding dan Special Gap tidak dihitung sebagai "Used Bytes" meskipun ikut mengonsumsi ruang pool.

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
*Gambar 6.8 — Output diagnostik Historia: utilisasi pool dengan perhitungan byte data aktif saja*

---

### Terminasi Program dan Ringkasan Akhir

Saat memilih opsi 0, program keluar dari loop utama dan mencetak ringkasan diagnostik ketiga pool beserta banner naratif masing-masing karakter. Selanjutnya, `free()` dipanggil untuk setiap pool dan program mencetak pesan penutup.

```text
============================================================
Historia: Free at last. Alignment 16, spark guidance +1.
============================================================
Pool: 0x... | Size: 1024 | Bump: 52 | Align: 16 + Gap 1
Entries: 2
Used Slots: 2 | Used Bytes: 36
Utilization: 3.515625%

Lagta: Respect alignment.
Daiki: Mind the flow.
Cyria: Beware of the abyss.
Xelvelt: Compare stack and heap.
Good bye. May the Koura sisters function well within your hands.
```
*Gambar 6.9 — Ringkasan akhir program: diagnostik Historia dan pembebasan seluruh pool dengan free()*
