# PANDUAN PRAKTIKUM PEMROGRAMAN: MODUL 6 (POINTER)
## ALGORITMA DAN PEMROGRAMAN

---

### **DAFTAR ISI**
1. [IDENTITAS PRAKTIKUM](#identitas-praktikum)
2. [MODUL 6: POINTER](#modul-6-pointer)
3. [I. CAPAIAN PEMBELAJARAN LUARAN](#i-capaian-pembelajaran-luaran-modul-6)
4. [II. CAPAIAN PEMBELAJARAN MATA KULIAH](#ii-capaian-pembelajaran-mata-kuliah-modul-6)
5. [III. SUB – CAPAIAN PEMBELAJARAN MK](#iii-sub--capaian-pembelajaran-mk-modul-6)
6. [IV. TUJUAN MODUL 6](#iv-tujuan-modul-6)
7. [V. PRASYARAT MODUL 6](#v-prasyarat-modul-6)
8. [VI. PERSIAPAN SEBELUM PRAKTIKUM](#vi-persiapan-sebelum-praktikum-modul-6)
9. [VII. ALUR PELAKSANAAN PRAKTIKUM](#vii-alur-pelaksanaan-praktikum-modul-6)
10. [VIII. TROUBLESHOOTING](#viii-troubleshooting)
11. [IX. KESIMPULAN](#ix-kesimpulan)

---

### **IDENTITAS PRAKTIKUM**

*   **MATA KULIAH**: ALGORITMA DAN PEMROGRAMAN
*   **MODUL/PERTEMUAN**: POINTER
*   **TOPIK**: PEMROGRAMAN
*   **WAKTU PELAKSANAAN**: (Diisi sesuai jadwal)
*   **SOFTWARE YANG DIGUNAKAN**: Dev C++, VS Code
*   **JUMLAH ASISTEN PRAKTIKUM**: 3

#### **CAPAIAN PEMBELAJARAN LUARAN (CPL)**
*   **CPL II**: Menunjukkan sikap bertanggung jawab dan profesional atas pekerjaan di bidang keahliannya secara mandiri dan tim, bertanggung jawab terhadap unjuk kerja personal/tim, dan menyadari kebutuhan pembelajar sepanjang hayat dan menerapkannya dalam kehidupan profesional.
*   **CPL VI**: Mampu menerapkan pemikiran secara logis, kritis, sistematis dan inovatif, dan melakukan riset berdasarkan pengetahuan dan teknologi yang dipelajari.
*   **CPL VII**: Memiliki pengetahuan dasar (matematika, komputasi, sistem komputer dan jaringan) yang kuat dan dapat menyelesaikan masalah yang kompleks di bidang teknik informatika.

#### **CAPAIAN PEMBELAJARAN MK (CPMK)**
*   **CPMK V**: Mampu menemukan kesalahan dan memperbaiki dalam pemrograman yang berhubungan dengan struct serta pointer.

---

### **MODUL 6 : POINTER**

#### **I. CAPAIAN PEMBELAJARAN LUARAN MODUL 6**
*   Bertanggung jawab dan profesional dalam unjuk kerja.
*   Menerapkan pemikiran logis dalam riset teknologi.
*   Mampu menyelesaikan masalah kompleks di bidang informatika.

#### **II. CAPAIAN PEMBELAJARAN MATA KULIAH MODUL 6**
*   Menguasai pengelolaan memori dinamis dan perbaikan bug terkait pointer.

#### **III. SUB – CAPAIAN PEMBELAJARAN MK MODUL 6**
*   Memproyeksikan penggunaan Pointer pada permasalahan algoritma.

#### **IV. TUJUAN MODUL 6**
1.  Memahami definisi dan mekanisme pointer.
2.  Memahami hubungan antara `struct` dan `pointer`.
3.  Memahami konsep `array-to-pointer decay`.
4.  Mampu melakukan alokasi dan manipulasi memori secara dinamis.
5.  Melakukan transfer data antar fungsi secara efisien.

#### **V. PRASYARAT MODUL 6**
*   Memahami konsep pemrograman dasar.
*   Menguasai sintaks dasar C++.
*   Telah menginstal Compiler/IDE.

#### **VI. PERSIAPAN SEBELUM PRAKTIKUM MODUL 6**
*   Menginstal software pendukung (DevC++/VS Code).
*   Membaca materi teori singkat.
*   Mengerjakan pre-test.

---

### **VII. ALUR PELAKSANAAN PRAKTIKUM MODUL 6**

#### **(0) Penjelasan Topik & Ekspektasi Challenge**
**i. Tentang Manajemen Memori**  
Manajemen memori adalah proses mengontrol dan mengoordinasikan cara program mengakses memori komputer (RAM). Dalam Modul 6 ini, kita akan keluar dari zona aman variabel lokal (*Stack*) dan masuk ke area **Heap**. Di sini, Anda memiliki kebebasan penuh untuk menentukan berapa banyak memori yang dibutuhkan saat program sedang berjalan (*Run-time*), namun kebebasan ini datang dengan tanggung jawab untuk membebaskannya kembali agar tidak terjadi kebocoran memori (*Memory Leak*).

**ii. Apa yang Diharapkan dalam Challenge?**  

*   **Jalur Reguler (C++ Style)**:  
    Anda akan fokus pada pola **New-Copy-Delete**. Tantangan utamanya adalah mengelola buffer tunggal yang ukurannya dapat membesar (*resize*). Anda akan belajar cara memindahkan data lama ke alamat baru tanpa merusak isinya.
*   **Jalur Expert (C Style & Low-Level)**:  
    Anda akan masuk ke level perangkat keras. Tantangan utamanya adalah **Memory Alignment** (meratakan data ke kelipatan 4, 8, atau 16 byte) dan **Special Gaps** (pergeseran manual berbasis NIM). Anda harus memastikan CPU dapat membaca data dengan performa maksimal.

#### **(1) SetUp Awal**
1.  Buka VS Code.
2.  Buat file baru dengan ekstensi `.cpp`.

#### **(2) Eksperimen Inti (Regular Path)**

##### **a) Penggunaan “new” dan “delete”**
**i. Penjelasan**:
`new` mengalokasikan memori di heap, sementara `delete` membebaskannya. Untuk array, gunakan `new[]` dan `delete[]`.

**ii. Sintaks**:
```cpp
#include <iostream>
using namespace std;

int main() {
    // Alokasi Tunggal
    int* p = new int;
    *p = 100;
    cout << "Nilai: " << *p << endl;
    delete p;

    // Alokasi Array
    int* arr = new int[3];
    arr[0] = 10; arr[1] = 20; arr[2] = 30;
    cout << "Indeks 1: " << arr[1] << endl;
    delete[] arr;
    return 0;
}
```
**iii. Contoh Output**:
```text
Nilai: 100
Indeks 1: 20
```

##### **b) Tail Reclamation**
**i. Penjelasan**:
Jika elemen terakhir di buffer dihapus, `cursor` dapat bergerak mundur untuk menggunakan kembali ruang tersebut.

**ii. Sintaks**:
```cpp
// Contoh logika Tail Reclamation
if (!any_active_at_higher_index) {
    cursor = last_active_entry_end;
    cout << "Memori tail berhasil direklamasi ke posisi: " << cursor << endl;
} else {
    cout << "Terjadi fragmentasi. Tidak dapat reklamasi." << endl;
}
```

##### **c) Expand Memory (New-Copy-Delete)**
**i. Penjelasan**:
Proses mengubah ukuran buffer dengan mengalokasikan blok baru, menyalin data, dan menghapus blok lama.

**ii. Sintaks**:
```cpp
// 1. Alokasi buffer baru yang lebih besar
unsigned char* new_buf = new unsigned char[new_limit];

// 2. Salin data dari buffer lama
for(size_t i = 0; i < cursor; i++) new_buf[i] = old_buffer[i];

// 3. Hapus buffer lama dan arahkan ke yang baru
delete[] old_buffer;
old_buffer = new_buf;
```

#### **(3) Eksperimen Inti (Expert Path)**

##### **a) Pemahaman `void*` (Generic Pointer)**
**i. Penjelasan**:
`void*` adalah tipe pointer khusus yang dapat menyimpan alamat memori dari tipe data apa pun (int, char, struct, dll.). Namun, `void*` memiliki batasan:
*   **Tidak bisa didereference langsung**: Anda tidak bisa menggunakan `*p` jika `p` adalah `void*`.
*   **Harus di-cast**: Sebelum digunakan, harus diubah ke tipe spesifik (misal: `(int*)p`).
*   **malloc()**: Fungsi ini mengembalikan `void*` karena ia hanya tahu jumlah byte yang dipesan, bukan tipe datanya.

##### **b) Tabel Referensi Ukuran & Tipe Data**
Penting untuk menghitung *alignment* dan *pointer arithmetic*:

| Tipe Data | Ukuran (Typical 64-bit) | Deskripsi |
| :--- | :--- | :--- |
| `char` | 1 Byte | Karakter tunggal / Raw byte |
| `int` | 4 Byte | Bilangan bulat |
| `double` | 8 Byte | Bilangan desimal presisi ganda |
| `pointer` | 8 Byte | Alamat memori (apapun tipenya) |

##### **c) Penyelarasan Memori (Alignment)**
**i. Penjelasan**:
Menjamin alamat memori berada pada kelipatan tertentu (misal 8 atau 16) untuk performa optimal CPU.

**ii. Visualisasi Peta Memori (Memory Map)**:
Bayangkan alokasi string "Halo" (5 byte termasuk `\0`) diikuti oleh integer `123`:

**Tanpa Alignment (Rapat)**:
`[H][a][l][o][\0][1][2][3][4]` -> Integer dimulai di alamat ganjil (tidak efisien).

**Dengan Alignment 4-byte**:
`[H][a][l][o][\0][pad][pad][pad] [1][2][3][4]` -> Integer dimulai di alamat kelipatan 4 (cepat).

**iii. Sintaks**:
```cpp
size_t align_up(size_t off, size_t align) {
    if (align == 0) return off;
    size_t remainder = off % align;
    if (remainder == 0) return off;
    return off + (align - remainder);
}
```

##### **b) Mekanisme "Special Gap" (Formula Offset)**
**i. Penjelasan**:
Pergeseran manual tambahan berdasarkan algoritma unik (misal: paritas ID mahasiswa).

**ii. Contoh Formula**:
```cpp
// Berdasarkan 3 digit terakhir ID mahasiswa (base_gap)
size_t gap_layanan_a = (base_gap % 2 != 0) ? 1 : 0;
size_t gap_layanan_b = (base_gap * 7 % 23) + 12;

size_t offset_final = align_up(cursor, 16) + gap_layanan_a;
```

##### **c) Alokasi Dinamis Array of Structs**
**i. Penjelasan**:
Mengelola banyak pool memori menggunakan array struktur yang dialokasikan di heap.

**ii. Sintaks**:
```cpp
struct MemoryPool {
    unsigned char* pool;
    size_t size;
};

// Mengalokasikan array berisi 3 struct MemoryPool
MemoryPool* list = (MemoryPool*) malloc(3 * sizeof(MemoryPool));

// Setiap struct mengalokasikan buffer-nya sendiri
for(int i = 0; i < 3; i++) {
    list[i].pool = (unsigned char*) malloc(1024);
}
```

##### **d) Diagnostik & Utilitas**
**i. Penjelasan**:
Menghitung penggunaan memori secara real-time.

**ii. Rumus**:
`Utilization % = (Used_Bytes / Pool_Size) * 100`

#### **(4) Kode Eksperimen: Sandbox Manajemen Memori**

##### **A) Jalur Reguler (C++ Style)**
**i. Penjelasan**:
Fokus pada penggunaan `new` dan `delete[]` untuk mengelola buffer dinamis sederhana. Mahasiswa belajar cara menggeser `cursor` secara sekuensial.

**ii. Kode Program**:
```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main() {
    size_t kapasitas = 100;
    unsigned char* buffer = new unsigned char[kapasitas];
    size_t cursor = 0;

    cout << "--- Sandbox Reguler (C++) ---" << endl;

    // Menyimpan String
    const char* nama = "Praktikan";
    size_t len = strlen(nama) + 1;
    for(size_t i = 0; i < len; i++) buffer[cursor + i] = nama[i];
    cursor += len;

    // Menyimpan Integer (Casting manual)
    int nilai = 85;
    *(int*)(buffer + cursor) = nilai;
    cursor += sizeof(int);

    cout << "Data tersimpan. Cursor saat ini: " << cursor << endl;
    cout << "Isi String: " << (char*)buffer << endl;
    cout << "Isi Integer: " << *(int*)(buffer + len) << endl;

    delete[] buffer;
    return 0;
}
```

##### **B) Jalur Expert (C Style & Alignment)**
**i. Penjelasan**:
Menggunakan `malloc`, `free`, dan teknik *Memory Alignment*. Mahasiswa belajar cara meratakan alamat memori ke batas byte tertentu (misal kelipatan 8) untuk efisiensi sistem.

**ii. Kode Program**:
```cpp
#include <iostream>
#include <cstdlib>
#include <cstring>

using namespace std;

size_t align_up(size_t addr, size_t align) {
    return (addr + (align - 1)) & ~(align - 1);
}

int main() {
    size_t pool_size = 256;
    unsigned char* pool = (unsigned char*)malloc(pool_size);
    size_t bump = 0;

    cout << "--- Sandbox Expert (C & Alignment) ---" << endl;

    // Alokasi 1: String dengan Alignment 8
    size_t start1 = align_up(bump, 8);
    const char* msg = "Data Expert";
    memcpy(pool + start1, msg, strlen(msg) + 1);
    bump = start1 + strlen(msg) + 1;

    // Alokasi 2: Integer dengan Alignment 16 + Gap Manual 2
    size_t start2 = align_up(bump, 16) + 2;
    int secret = 12345;
    memcpy(pool + start2, &secret, sizeof(int));
    
    cout << "Offset 1 (Aligned 8): " << start1 << endl;
    cout << "Offset 2 (Aligned 16 + Gap 2): " << start2 << endl;
    cout << "Data: " << (char*)(pool + start1) << " | " << *(int*)(pool + start2) << endl;

    free(pool);
    return 0;
}
```

#### **(5) Latihan Mandiri (Hands-On Task)**

##### **A) Tugas Jalur Reguler**
**Target**: Membangun sistem pencatatan nilai sederhana.
1.  Gunakan `argc` dan `argv` untuk menerima **Nama Mahasiswa** saat program dijalankan.
2.  Alokasikan array `int` di heap menggunakan `new[]` untuk menyimpan 3 nilai awal.
3.  Implementasikan fitur **Resize**: Jika pengguna ingin menambah nilai ke-4, alokasikan buffer baru, salin data lama, dan hapus buffer lama.
4.  Pastikan tidak ada *Memory Leak* (gunakan `delete[]`).

##### **B) Tugas Jalur Expert**
**Target**: Membangun pengelola memori tingkat rendah (*Low-Level Allocator*).
1.  Gunakan `argc` dan `argv` untuk menerima **System ID** (angka) dari terminal.
2.  Gunakan `malloc` untuk membuat pool memori sebesar 256 byte.
3.  Gunakan fungsi `align_up` untuk menyimpan data string pada alamat yang terselaras 8-byte.
4.  Tambahkan "Custom Gap" pada offset memori berdasarkan `(SystemID % 5)`.
5.  Tampilkan alamat memori dan offset setiap data yang disimpan.

---

#### **(6) Tutorial: Menjalankan Program dengan Argumen (argc/argv)**

Agar program dapat menerima input `argv`, Anda harus menjalankannya dengan parameter:

##### **A) Melalui Terminal / Command Prompt**
1.  Buka terminal di folder file `.cpp` Anda.
2.  Compile program:
    ```bash
    g++ nama_file.cpp -o program.exe
    ```
3.  Jalankan dengan argumen:
    ```bash
    .\program.exe ArgumenSatu ArgumenDua
    ```

##### **B) Melalui Dev-C++**
1.  Klik menu **Execute** di bagian atas.
2.  Pilih **Parameters**.
3.  Ketik argumen Anda di kotak **Parameters to pass to your program** (misal: `Budi 123`).
4.  Klik **OK**, lalu jalankan program dengan **Compile & Run (F11)**.

##### **C) Melalui VS Code**
1.  Gunakan terminal terintegrasi ( `Ctrl + ~` ).
2.  Ikuti langkah yang sama dengan metode **Terminal** di atas.

#### **(6) Tabel Praktik Terbaik (Pointer Best Practices)**

| Praktik | Deskripsi | Mengapa? |
| :--- | :--- | :--- |
| **Inisialisasi nullptr** | `int* p = nullptr;` | Menghindari *Wild Pointer* yang menunjuk alamat acak. |
| **Cek NULL setelah Malloc** | `if (p == NULL) ...` | Menangani kegagalan memori jika RAM penuh. |
| **Hapus dan nullptr** | `delete p; p = nullptr;` | Menghindari *Dangling Pointer* (menggunakan pointer yang sudah dihapus). |
| **Satu Alokasi = Satu Dealokasi** | Setiap `new` harus ada `delete`. | Mencegah *Memory Leak* (kebocoran memori). |

---

### **VIII. TROUBLESHOOTING**
1.  **Segmentation Fault**: Mengakses pointer `NULL` atau di luar batas alokasi.
2.  **Memory Leak**: Lupa memanggil `free()` atau `delete[]`.
3.  **Dangling Pointer**: Menggunakan pointer setelah memori dibebaskan.
4.  **Alignment Mismatch**: Membaca data multi-byte pada alamat yang tidak terselaras.

---

### **IX. KESIMPULAN**
Manajemen memori manual memberikan kontrol penuh atas sumber daya sistem, namun menuntut ketelitian tinggi untuk menghindari kebocoran dan kerusakan memori. Pemahaman akan *pointer arithmetic* dan *heap layout* adalah kunci utama dalam pemrograman tingkat rendah.
