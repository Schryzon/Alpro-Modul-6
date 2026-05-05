# Laporan Praktikum Modul 6 — Regular Path
## CyroN Divine Interface: Manajemen Memori Neural Historia

### Deskripsi Program

Program ini merupakan implementasi terminal berbasis teks (TUI) bernama **CyroN Divine Interface** yang berperan sebagai sistem manajemen *neural buffer* untuk subjek Historia Koura (XMA-02A). Program dijalankan dengan satu argumen berupa NIM mahasiswa melalui `argv[1]` dalam format `F1D02xxxxxx`. NIM ini divalidasi oleh program dan digunakan untuk mengekstrak frekuensi operator dari tiga digit terakhirnya secara matematis tanpa fungsi konversi bawaan.

Manajemen memori pada program ini menggunakan operator modern C++: `new` untuk mengalokasikan buffer dinamis dan `delete[]` untuk membebaskannya saat program selesai. Seluruh data disimpan dalam struktur `Neural_Core` yang mempertahankan pointer buffer, batas kapasitas (`buffer_limit`), posisi kursor (`cursor`), dan array entri aktif (`Neural_Entry`).

---

### Validasi Argumen Program

Sebelum terminal diaktifkan, program memvalidasi argumen yang diberikan. Apabila NIM tidak disertakan, jumlahnya lebih dari satu, panjangnya bukan 11 karakter, atau tidak diawali `F1D02`, program menolak akses dan menampilkan pesan kesalahan yang sesuai, lalu berhenti dengan kode `return 1`.

```text
$ ./solution.exe
Error: Neural Link requires an Operator ID (Student ID).
Usage: ./solution.exe <Student_ID>
```
*Gambar 6.1 — Penanganan error saat program dijalankan tanpa argumen*

---

### Menu Utama

Setelah validasi berhasil dan `init_core()` dijalankan, program memasuki loop utama dan menampilkan antarmuka menu. Kutipan naratif dari karakter Xelisa ditampilkan di atas menu dan berganti setiap siklus menggunakan variabel statis sebagai penghitung.

```text
          CYRON TERMINAL: DIVINE SUPPRESSION

Xelisa: "Exquisite... the synchronization is perfect."
------------------------------------------------------------
1. View Neural Map (Status)
2. Inject Neural Thread (Add)
3. Purge Corrupted Link (Delete)
4. Expand Willpower (Resize)
0. Surrender (Exit)
------------------------------------------------------------
Select Operation:
```
*Gambar 6.2 — Tampilan menu utama CyroN Divine Interface*

---

### Fitur 2: Injeksi Neural Thread

Operasi injeksi memungkinkan penambahan dua jenis data ke buffer: *Willpower Pulse* untuk string teks (`char*`) dan *Thunder Discharge* untuk nilai integer (`int`). Data disalin secara manual byte-per-byte ke dalam buffer heap menggunakan pointer aritmatik, tanpa menggunakan fungsi seperti `memcpy` atau `strcpy`.

Setiap injeksi mencatat posisi awal data (`start_pos`), panjang alokasi (`len`), tipe data, dan status keaktifan pada `Neural_Entry` yang sesuai, lalu menggeser kursor sejauh ukuran data yang baru ditambahkan.

```text
Select Operation: 2
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 0
Enter Willpower Input: Absolute Obedience.
CyroN Command: "Subject resistance detected. Overriding feedback."
>> Press ENTER for next pulse...
```
*Gambar 6.3 — Injeksi Willpower Pulse (data teks) ke buffer Historia berhasil*

Apabila data yang akan diinjeksikan melebihi kapasitas buffer yang tersisa (`cursor + need > buffer_limit`), operasi dibatalkan tanpa mengubah state buffer, dan program menampilkan pesan error.

```text
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 0
Enter Willpower Input: [input yang panjangnya melebihi sisa kapasitas buffer]
!! OPTIMIZATION ERROR !! The vessel's ego rejected the thread!
>> Press ENTER for next pulse...
```
*Gambar 6.4 — Penanganan kegagalan injeksi akibat buffer penuh*

---

### Fitur 1: View Neural Map

Setelah beberapa injeksi berhasil dilakukan, isi buffer dapat diperiksa melalui opsi View Neural Map. Program menghitung **Persentase Stabilitas** dengan rumus `100 - (100 * cursor / buffer_limit)` dan menampilkan seluruh entri aktif beserta tipe, offset, alamat fisik di heap, dan nilai datanya.

```text
============================================================
NEURAL MAP: HISTORIA KOURA [STABILITY: 81%]
============================================================
[0] TYPE: Willpower Pulse | OFFSET: 0 | ADDR: 0x... | DATA: "Absolute Obedience."
[1] TYPE: Thunder Discharge | OFFSET: 20 | ADDR: 0x... | DATA: 777MW
------------------------------------------------------------
Cursor: 24 / 128 Bytes used.
------------------------------------------------------------
>> Press ENTER for next pulse...
```
*Gambar 6.5 — Neural Map setelah dua entri diinjeksikan, menunjukkan offset dan alamat fisik masing-masing*

---

### Fitur 3: Purge Corrupted Link

Operasi purge menonaktifkan entri berdasarkan indeks (`threads[idx].active = false`). Jika entri yang dihapus adalah entri aktif terakhir di buffer (entri "ekor"), sistem secara otomatis menghitung ulang posisi kursor mundur ke posisi akhir entri aktif tertinggi — proses ini disebut **Tail Reclamation**, yang memungkinkan ruang tersebut segera digunakan kembali untuk injeksi berikutnya.

```text
Select Operation: 3
Enter Link Index to purge: 1
Link 1 dissolved into ether.
Neural Core reclaimed space. Tail at: 20
>> Press ENTER for next pulse...
```
*Gambar 6.6 — Tail Reclamation: kursor bergeser mundur setelah penghapusan entri terakhir*

Sebaliknya, apabila masih ada entri aktif dengan indeks yang lebih tinggi, ruang yang ditinggalkan menjadi terfragmentasi dan tidak dapat direklamasi. Kondisi ini terjadi ketika pengguna menghapus entri di tengah buffer sementara entri di atasnya masih aktif.

```text
Enter Link Index to purge: 0
Link 0 dissolved into ether.
Fragmentation detected. Cannot reclaim space yet!
>> Press ENTER for next pulse...
```
*Gambar 6.7 — Fragmentasi memori akibat penghapusan entri non-ekor sementara entri berikutnya masih aktif*

---

### Fitur 4: Expand Willpower

Fitur Expand Willpower mengimplementasikan konsep inti modul ini: alur **New-Copy-Delete**. Program mengalokasikan blok buffer baru yang lebih besar dengan `new`, menyalin seluruh data lama ke buffer baru secara manual byte-per-byte, lalu membebaskan buffer lama dengan `delete[]`. Keberhasilan proses ini dibuktikan dari berubahnya alamat fisik buffer yang ditampilkan setelah operasi selesai.

```text
Select Operation: 4
Enter new buffer limit: 512
CyroN Command: "Stability increased. The vessel is now 100% compliant."
Allocated Frequency: 0x7f9a3c000b20
>> Press ENTER for next pulse...
```
*Gambar 6.8 — Ekspansi buffer berhasil; alamat fisik berubah membuktikan blok heap baru telah dialokasikan*

---

### Terminasi Program

Ketika operator memilih opsi 0, program menampilkan pesan penutup naratif, menunggu konfirmasi ENTER, lalu memanggil `destroy_core()` yang membebaskan seluruh heap dengan `delete[]` dan mengarahkan pointer ke `nullptr`. Program kemudian menampilkan pesan perpisahan yang menyertakan frekuensi operator yang diekstrak dari NIM.

```text
Select Operation: 0
Reality destabilizes...
>> Press ENTER for next pulse...
Connection terminated. Good bye, Operator 001.
```
*Gambar 6.9 — Urutan terminasi program dan pembebasan memori heap*
