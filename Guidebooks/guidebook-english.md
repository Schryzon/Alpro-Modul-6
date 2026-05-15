# PROGRAMMING PRACTICUM GUIDE: MODULE 6 (POINTERS)
## ALGORITHMS AND PROGRAMMING

---

### **TABLE OF CONTENTS**
1. [PRACTICUM IDENTITY](#practicum-identity)
2. [MODULE 6: POINTERS](#module-6-pointers)
3. [I. PROGRAM LEARNING OUTCOMES](#i-program-learning-outcomes-module-6)
4. [II. COURSE LEARNING OUTCOMES](#ii-course-learning-outcomes-module-6)
5. [III. SUB-COURSE LEARNING OUTCOMES](#iii-sub-course-learning-outcomes-module-6)
6. [IV. OBJECTIVES](#iv-objectives-module-6)
7. [V. PREREQUISITES](#v-prerequisites-module-6)
8. [VI. PREPARATION BEFORE PRACTICUM](#vi-preparation-before-practicum-module-6)
9. [VII. PRACTICUM EXECUTION FLOW](#vii-practicum-execution-flow-module-6)
10. [VIII. TROUBLESHOOTING](#viii-troubleshooting)
11. [IX. CONCLUSION](#ix-conclusion)

---

### **PRACTICUM IDENTITY**

*   **COURSE**: ALGORITHMS AND PROGRAMMING
*   **MODULE/MEETING**: POINTERS
*   **TOPIC**: PROGRAMMING
*   **EXECUTION TIME**: (To be filled according to schedule)
*   **SOFTWARE USED**: Dev C++, VS Code
*   **NUMBER OF TEACHING ASSISTANTS**: 3

#### **PROGRAM LEARNING OUTCOMES (PLO)**
*   **PLO II**: Demonstrate a responsible and professional attitude towards work in their field of expertise independently and in teams, being responsible for personal/team performance, and recognizing the need for lifelong learning and applying it in professional life.
*   **PLO VI**: Able to apply logical, critical, systematic, and innovative thinking, and conduct research based on learned knowledge and technology.
*   **PLO VII**: Possess strong foundational knowledge (mathematics, computation, computer systems, and networks) to solve complex problems in the field of informatics engineering.

#### **COURSE LEARNING OUTCOMES (CLO)**
*   **CLO V**: Able to find and fix errors in programming related to structs and pointers.

---

### **MODULE 6: POINTERS**

#### **I. PROGRAM LEARNING OUTCOMES MODULE 6**
*   Responsibility and professionalism in performance.
*   Application of logical thinking in technological research.
*   Solving complex informatics engineering problems.

#### **II. COURSE LEARNING OUTCOMES MODULE 6**
*   Mastery of dynamic memory management and pointer-related bug fixing.

#### **III. SUB-COURSE LEARNING OUTCOMES MODULE 6**
*   Projecting the use of Pointers in algorithmic problem solving.

#### **IV. OBJECTIVES MODULE 6**
1.  Understand pointer definitions and mechanisms.
2.  Understand the relationship between `struct` and `pointer`.
3.  Understand `array-to-pointer decay` concepts.
4.  Able to perform dynamic memory allocation and manipulation.
5.  Perform efficient data transfer between functions.

#### **V. PREREQUISITES MODULE 6**
*   Understand basic programming concepts.
*   Master C++ syntax basics.
*   Installed Compiler/IDE.

#### **VI. PREPARATION BEFORE PRACTICUM MODULE 6**
*   Install supporting software (DevC++/VS Code).
*   Read brief theoretical material.
*   Complete the pre-test.

---

### **VII. PRACTICUM EXECUTION FLOW MODULE 6**

#### **(0) Topic Explanation & Challenge Expectations**
**i. About Memory Management**  
Memory management is the process of controlling and coordinating how a program accesses computer memory (RAM). In Module 6, we leave the safety zone of local variables (**Stack**) and enter the **Heap**. Here, you have complete freedom to determine how much memory is needed while the program is running (**Run-time**). However, this freedom comes with the responsibility of releasing it back to the system to prevent **Memory Leaks**.

**ii. What to Expect in the Challenge?**  

*   **Regular Path (C++ Style)**:  
    You will focus on the **New-Copy-Delete** pattern. The primary challenge is managing a single buffer that can grow in size (**resize**). You will learn how to migrate old data to a new address without corrupting its contents.
*   **Expert Path (C Style & Low-Level)**:  
    You will descend to the hardware level. The main challenges are **Memory Alignment** (aligning data to multiples of 4, 8, or 16 bytes) and **Special Gaps** (manual NIM-based offsets). You must ensure the CPU can read data with maximum efficiency.

#### **(1) Initial Setup**
1.  Open VS Code.
2.  Create a new file with the `.cpp` extension.

#### **(2) Core Experiment (Regular Path)**

##### **a) Usage of "new" and "delete"**
**i. Explanation**:
`new` allocates memory on the heap, while `delete` releases it. For arrays, use `new[]` and `delete[]`.

**ii. Syntax**:
```cpp
#include <iostream>
using namespace std;

int main() {
    // Single Allocation
    int* p = new int;
    *p = 100;
    cout << "Value: " << *p << endl;
    delete p;

    // Array Allocation
    int* arr = new int[3];
    arr[0] = 10; arr[1] = 20; arr[2] = 30;
    cout << "Index 1: " << arr[1] << endl;
    delete[] arr;
    return 0;
}
```
**iii. Sample Output**:
```text
Value: 100
Index 1: 20
```

##### **b) Tail Reclamation**
**i. Explanation**:
If the last element in the buffer is deleted, the `cursor` can move backward to reclaim that space.

**ii. Logic**:
```cpp
// Logic for Tail Reclamation
if (!any_active_above_current) {
    cursor = current_entry_start;
    cout << "Tail reclaimed. Cursor moved back to: " << cursor << endl;
}
```

##### **c) Expand Memory (New-Copy-Delete)**
**i. Explanation**:
Resizing a buffer by allocating a new block, copying old data, and deleting the old block.

**ii. Syntax**:
```cpp
// 1. Allocate a larger buffer
unsigned char* new_buf = new unsigned char[new_size];

// 2. Copy old data
for(size_t i = 0; i < cursor; i++) new_buf[i] = old_buffer[i];

// 3. Rebind pointers
delete[] old_buffer;
old_buffer = new_buf;
```

#### **(3) Core Experiment (Expert Path)**

##### **a) Understanding `void*` (Generic Pointer)**
**i. Explanation**:
A `void*` is a special pointer type that can hold the address of any data type (int, char, struct, etc.). However, it has specific rules:
*   **Cannot be dereferenced directly**: You cannot use `*p` if `p` is a `void*`.
*   **Must be cast**: Before use, it must be converted to a specific type (e.g., `(int*)p`).
*   **malloc()**: This function returns a `void*` because it only knows the number of bytes requested, not what will be stored there.

##### **b) Data Type Size Reference Table**
Essential for calculating *alignment* and *pointer arithmetic*:

| Data Type | Size (Typical 64-bit) | Description |
| :--- | :--- | :--- |
| `char` | 1 Byte | Single character / Raw byte |
| `int` | 4 Bytes | Integer |
| `double` | 8 Bytes | Double-precision decimal |
| `pointer` | 8 Bytes | Memory address (any type) |

##### **c) Memory Alignment**
**i. Explanation**:
Aligning addresses to specific byte multiples (e.g., 4, 8, 16) for CPU efficiency.

**ii. Visual Memory Map**:
Imagine storing a string "Hi" (3 bytes including `\0`) followed by an integer `789`:

**Without Alignment (Packed)**:
`[H][i][\0][7][8][9][...]` -> Integer starts at an odd address (inefficient).

**With 4-byte Alignment**:
`[H][i][\0][pad] [7][8][9][...]` -> Integer starts at a multiple of 4 (fast access).

**iii. Syntax**:
```cpp
size_t align_up(size_t off, size_t align) {
    if (align == 0) return off;
    size_t remainder = off % align;
    if (remainder == 0) return off;
    return off + (align - remainder);
}
```

##### **b) "Special Gap" Mechanism (Offset Formula)**
**i. Explanation**:
Additional manual displacement based on a unique algorithm (e.g., student ID parity).

**ii. Example Formulas**:
```cpp
// Based on last 3 digits of student ID (base_gap)
size_t gap_service_a = (base_gap % 2 != 0) ? 1 : 0;
size_t gap_service_b = (base_gap * 7 % 23) + 12;

size_t offset = align_up(cursor, 16) + gap_service_a;
```

##### **c) Dynamic Array of Structs**
**i. Explanation**:
Managing multiple pools using a heap-allocated array of structures.

**ii. Syntax**:
```cpp
struct MemoryPool {
    unsigned char* data;
    size_t capacity;
};

// Allocate array of 3 structs
MemoryPool* pools = (MemoryPool*) malloc(3 * sizeof(MemoryPool));

// Initialize internal pools
for(int i = 0; i < 3; i++) {
    pools[i].data = (unsigned char*) malloc(1024);
}
```

##### **d) Diagnostic Metrics**
**i. Formula**:
`Utilization % = (Bytes_Used / Pool_Capacity) * 100`

#### **(4) Experiment Code: Memory Management Sandbox**

##### **A) Regular Path (C++ Style)**
**i. Explanation**:
Focuses on using `new` and `delete[]` to manage simple dynamic buffers. Students learn how to sequentially shift a `cursor`.

**ii. Program Code**:
```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main() {
    size_t capacity = 100;
    unsigned char* buffer = new unsigned char[capacity];
    size_t cursor = 0;

    cout << "--- Regular Sandbox (C++) ---" << endl;

    // Store String
    const char* name = "Student";
    size_t len = strlen(name) + 1;
    for(size_t i = 0; i < len; i++) buffer[cursor + i] = name[i];
    cursor += len;

    // Store Integer (Manual casting)
    int value = 95;
    *(int*)(buffer + cursor) = value;
    cursor += sizeof(int);

    cout << "Data stored. Current cursor: " << cursor << endl;
    cout << "String content: " << (char*)buffer << endl;
    cout << "Integer content: " << *(int*)(buffer + len) << endl;

    delete[] buffer;
    return 0;
}
```

##### **B) Expert Path (C Style & Alignment)**
**i. Explanation**:
Using `malloc`, `free`, and *Memory Alignment* techniques. Students learn how to align memory addresses to specific byte boundaries (e.g., multiples of 8) for system efficiency.

**ii. Program Code**:
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

    cout << "--- Expert Sandbox (C & Alignment) ---" << endl;

    // Allocation 1: String with 8-byte Alignment
    size_t start1 = align_up(bump, 8);
    const char* msg = "Expert Data";
    memcpy(pool + start1, msg, strlen(msg) + 1);
    bump = start1 + strlen(msg) + 1;

    // Allocation 2: Integer with 16-byte Alignment + Manual Gap of 2
    size_t start2 = align_up(bump, 16) + 2;
    int secret = 54321;
    memcpy(pool + start2, &secret, sizeof(int));
    
    cout << "Offset 1 (Aligned 8): " << start1 << endl;
    cout << "Offset 2 (Aligned 16 + Gap 2): " << start2 << endl;
    cout << "Data: " << (char*)(pool + start1) << " | " << *(int*)(pool + start2) << endl;

    free(pool);
    return 0;
}
```

#### **(5) Hands-On Task**

##### **A) Regular Path Task**
**Objective**: Build a simple student grade tracking system.
1.  Use `argc` and `argv` to receive the **Student Name** when the program is run.
2.  Allocate an `int` array on the heap using `new[]` to store 3 initial grades.
3.  Implement a **Resize** feature: If the user wants to add a 4th grade, allocate a new buffer, copy the old data, and delete the old buffer.
4.  Ensure no *Memory Leaks* occur (use `delete[]`).

##### **B) Expert Path Task**
**Objective**: Build a low-level memory allocator.
1.  Use `argc` and `argv` to receive a **System ID** (numeric) from the terminal.
2.  Use `malloc` to create a memory pool of 256 bytes.
3.  Use an `align_up` function to store string data at 8-byte aligned addresses.
4.  Add a "Custom Gap" to the memory offset based on `(SystemID % 5)`.
5.  Display the memory addresses and offsets of every piece of data stored.

---

#### **(6) Tutorial: Running Programs with Arguments (argc/argv)**

For your program to receive `argv` inputs, you must run it with parameters:

##### **A) Using Terminal / Command Prompt**
1.  Open the terminal in your `.cpp` file's folder.
2.  Compile the program:
    ```bash
    g++ file_name.cpp -o program.exe
    ```
3.  Run with arguments:
    ```bash
    .\program.exe ArgumentOne ArgumentTwo
    ```

##### **B) Using Dev-C++**
1.  Click the **Execute** menu at the top.
2.  Select **Parameters**.
3.  Type your arguments in the **Parameters to pass to your program** box (e.g., `Alice 456`).
4.  Click **OK**, then run the program with **Compile & Run (F11)**.

##### **C) Using VS Code**
1.  Use the integrated terminal ( `Ctrl + ~` ).
2.  Follow the same steps as the **Terminal** method above.

#### **(6) Pointer Best Practices Table**

| Practice | Description | Why? |
| :--- | :--- | :--- |
| **Initialize to nullptr** | `int* p = nullptr;` | Prevents *Wild Pointers* pointing to random addresses. |
| **Check for NULL** | `if (p == NULL) ...` | Handles memory allocation failures if RAM is full. |
| **Delete then nullptr** | `delete p; p = nullptr;` | Prevents *Dangling Pointers* (accessing freed memory). |
| **One New = One Delete** | Every allocation must be released. | Prevents *Memory Leaks*. |

---

### **VIII. TROUBLESHOOTING**
1.  **Segmentation Fault**: Accessing `NULL` or invalid memory addresses.
2.  **Memory Leak**: Forgetting to call `free()` or `delete[]`.
3.  **Dangling Pointer**: Accessing memory after it has been freed.
4.  **Alignment Mismatch**: Improper multi-byte data reads on unaligned addresses.

---

### **IX. CONCLUSION**
Manual memory management provides absolute control over system resources but requires high precision to prevent leaks and crashes. Mastering *pointer arithmetic* and *heap layouts* is fundamental for low-level programming.
