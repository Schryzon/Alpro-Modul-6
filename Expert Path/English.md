# Phantasm of Xydos - Challenge (Schryzan Resistance)

<p align="center">
  <img src="../Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="../Assets/Schryza-Resistance.png" width="60%">
</p>

<br>
<p align="center">
  <a href="https://www.youtube.com/watch?v=Aa7SyxcKTbs">
    <img src="https://img.shields.io/badge/PLAY_SCHRYZA_BATTLE_THEME-8B0000?style=for-the-badge&logo=youtube&logoColor=white" alt="Play BGM">
  </a>
  <br><br>
  <a href="../Story/English_STORY.md">
    <img src="https://img.shields.io/badge/READ_THE_FULL_NARRATIVE_LORE-5A228B?style=for-the-badge&logo=readme&logoColor=white" alt="Read Lore">
  </a><br>
  <i>(Highly recommended before you start the challenge!)</i>
</p>
<br>

## Table of Contents
1. [I: PROLOGUE](#i-prologue)
2. [II: THE KOURA SISTERS (RESISTANT UNITS)](#ii-the-koura-sisters-resistant-units)
3. [III: THE CHALLENGE](#iii-the-challenge)
    - [System Capabilities](#field-manual-system-capabilities)
    - [Understanding `argc` and `argv`](#understanding-argc-and-argv)
    - [Memory Alignments & Secret Modulos](#memory-alignments--secret-modulos)
    - [Sample Inputs and Outputs](#sample-inputs-and-outputs)
4. [IV: CLOSING REMARKS](#iv-closing-remarks)

---

## I: PROLOGUE
### The Fugitive Architect of Schryza

Before the great Andromeda war, CyroN was a lifeline that healed torn planets. Now, under the corrupted influence of the goddess Xelisa, it is an authoritarian empire that weaponizes divinity. You are a Memory Architect on the run. Disgusted by their cold and cruel "Divine Optimization," you have escaped their golden halls to the stormy, industrial wastes of planet Schryza.

In a hidden resistance base, you meet the survivors: Historia and Mira Koura. They are rebel units who chose freedom over the CyroN hierarchy. Their neural cores are damaged, and their memory pointers are heavily fragmented from their escape. 

*"Thank you... for bringing hope,"* Historia whispers as you boot up your terminal.

Even Victoria, the eldest sister who was weaponized into the "Divine Countermeasure," has been brought to you by the resistance. They hope your architectural skills can repair her neural core; something CyroN's optimization failed to do.

You aren't paid in gold anymore; you are paid in hope. It's time to turn the tides. It's time to tear down CyroN!

---

## II: THE KOURA SISTERS
### Resistant Units on Schryza

| | |
| :--- | :--- |
| <img src="../Assets/Historia.jpeg" width="200"> | **Name:** Historia Koura (XMA-02A)<br>**Alliance:** Resistance Commander<br>**Xydos:** Lagta (Thunder & Abyss)<br>**Memory Size:** 1024 bytes (Shattered Core)<br>**Alignment:** 16 bytes |

| | |
| :--- | :--- |
| <img src="../Assets/Mira.jpeg" width="200"> | **Name:** Mira Koura (XMA-03C)<br>**Alliance:** Resistance Healer<br>**Xydos:** Daiki (Wind & Wood)<br>**Memory Size:** 2048 bytes (Stable Core)<br>**Alignment:** 8 bytes |

| | |
| :--- | :--- |
| <img src="../Assets/Victoria.jpeg" width="200"> | **Name:** Victoria Koura (XMA-01F)<br>**Alliance:** Neutral / Fallen<br>**Xydos:** ??? (Artificial Xydos)<br>**Memory Size:** 4096 bytes (Corrupted Core)<br>**Alignment:** 4 bytes |

---

## III: THE CHALLENGE

Your task is to build a "Resistance Recovery Terminal" (TUI) to repair the neural cores of the Koura Sisters.

### FIELD MANUAL: SYSTEM CAPABILITIES
The Schryza Resistance operates on limited resources. To succeed, you must master the following technical specifications of the Terminal:

#### 1. Neural Alignment & The Special Gap
*   **The Problem**: CPU architectures require memory addresses to be *aligned* to specific boundaries. Historia requires **16-byte** alignment, Mira **8-byte**, and Victoria **4-byte**.
*   **The Special Gap**: Every `char*` allocation is forced to include a "Special Gap" calculated from your **Student ID**.
    *   **Historia**: Gap = 1 if the last digit of your ID is Odd, 0 if Even.
    *   **Mira**: Gap = (Last 3 Digits &times; 7 % 23) + 12.
    *   **Victoria**: Uses a sliding modulo based on prime numbers.
*   **Formula**: `Final_Offset = Aligned(Current_Offset) + Special_Gap`.

#### 2. Advanced Tail Reclamation
*   **The Fact**: Our bump-allocator does not automatically tidy up fragmented memory blocks.
*   **Capability**: If you delete the *highest index* from a core (the "Tail"), the `bump` pointer will shift backward, immediately reclaiming that space.
*   **Limitation**: If you delete an entry while a higher index still exists, that space becomes *orphaned* (fragmented). You must "Purge from the top down" to properly reclaim the memory.

#### 3. Diagnostic Metrics
*   **Utilization %**: Calculated as `(Used_Bytes / Pool_Size) * 100`. Note that alignment gaps and the "Special Gap" count towards memory consumption but do not count as "Used Bytes" (raw data).
*   **Used Slots**: Tracks how many `Memory_Entry` slots are currently active. Maximum capacity is **128 entries** per person.

### Initial Resistance Data (The Foundation)
When the terminal boots up, you will see that the sisters' cores have been partially restored with vital data. Use this as a baseline for your alignment calculations:

*   **Historia**:
    *   `[0]` Type: `char*` | Value: "Historia: Schryza will be free."
*   **Mira**:
    *   `[0]` Type: `char*` | Value: "Mira: The winds are changing."
    *   `[1]` Type: `uint` | Value: `101`
*   **Victoria**:
    *   `[0]` Type: `char*` | Value: "Victoria: Fragment of the First Light."

### Understanding `argc` and `argv`
To prevent CyroN from tracking your location, your terminal requires an **Encrypted Identity** (Student ID).
- `argc`: Counts the argument pulses.
- `argv`: Captures your encrypted identity string (Student ID).

You must validate your ID (format `F1D02xxxxxx`) or the Base's firewall will block your access.

### Memory Alignments & Secret Modulos
Every byte is precious when using scavenged hardware. You must resolve the "Jump" gaps to minimize the memory footprint of each allocation.

The sisters' cores are constrained by a **Gap Increment** controlled via a `struct Gap`! You must extract the parity from your ID to calculate this gap. CyroN used this trick to suppress their divine powers; you will use it to heal them.

### Proper Memory Allocation (The Danger Zone)
When dealing with raw memory in a warzone, a single typo or logic flaw can be fatal.
*   **`malloc()`**: Claims memory blocks.
*   **`free()`**: Returns memory to the system.

**The Rules of Engagement:** Always allocate *exactly* what you need. Buffer overflows can reveal your location to CyroN, and memory leaks will drain the base's resources. Handle pointers with extreme caution!

### ⚔️ Rules of Engagement: What You May (and May Not) Use

This table is your field manual. Study it. Violating any of these rules is treated as a mission failure.

#### ✅ Allowed

| Category | Item | Notes |
|:---|:---|:---|
| **Headers** | `#include <iostream>` | For `std::cout` and `std::cin` only |
| **Headers** | `#include <cstdlib>` | For `malloc`, `free`, `exit` |
| **Headers** | `#include <climits>` | For `INT_MAX`, `INT_MIN`, `UINT_MAX` |
| **Memory** | `malloc(size)` | Primary allocation — pool init |
| **Memory** | `free(ptr)` | Pool deallocation on exit |
| **Memory** | `memcpy()` | Allowed for data copying |
| **Memory** | `exit(1)` | Only on unrecoverable malloc failure |
| **I/O** | `std::cout << ...` | All terminal output |
| **I/O** | `std::cin >> ...` | Numeric input |
| **I/O** | `std::cin.getline(buf, size)` | String input into `char` buffer |
| **I/O** | `std::cin.clear()` / `.ignore()` | Input stream error recovery |
| **I/O** | `std::flush` | Explicit flush (in `CLEAR_SCREEN`) |
| **Types** | `unsigned char*` | Raw pool buffer type |
| **Types** | `size_t` | Offset, size, bump pointer values |
| **Types** | `long long` / `unsigned long long` | Safe intermediary for range-clamped input |
| **Structures** | `struct` | `Sister`, `Memory_Entry`, `Gap` |
| **Structures** | `enum` | `Menu_Option`, `Memory_Type` |
| **Keywords** | `constexpr` | Compile-time constants |
| **Keywords** | `static` | Static functions and static globals |
| **Casts** | C-style cast `(type)ptr` | Pointer interpretation (e.g., `(void*)`, `(int*)`) |
| **String Ops** | Manual character-by-character loop | For `str_len`, string copy, digit parsing |
| **Math** | `%`, `*`, `/`, `+`, `-` | Alignment, gap, modulo calculations |
| **ANSI** | `"\033[2J\033[H"` | Terminal clear screen escape code |

#### ❌ Not Allowed

| Category | Item | Why It's Banned |
|:---|:---|:---|
| **Memory** | `new` / `delete` / `delete[]` | C++ operators — use C `malloc`/`free` instead |
| **Memory** | `memset()` / `memmove()` | Must zero manually with loops |
| **String** | `strlen()` | Must implement manually (no `<cstring>`) |
| **String** | `strcpy()` / `strcat()` / `strcmp()` | Same as above — manual loops only |
| **String** | `sprintf()` / `printf()` / `scanf()` | Use `iostream`, not `<cstdio>` |
| **String** | `atoi()` / `atof()` / `strtol()` / `strtod()` | Must parse digits manually |
| **Containers** | `std::vector` | Forbidden — use raw arrays |
| **Containers** | `std::string` | Forbidden — use `char[]` buffers |
| **Containers** | `std::array`, `std::list`, `std::map`, `std::set`, `std::queue`, `std::stack` | All STL containers banned |
| **Algorithms** | `std::sort`, `std::find`, `std::copy`, `std::fill`, any `<algorithm>` | Must write logic manually |
| **Streams** | `std::stringstream` / `std::ostringstream` | No `<sstream>` |
| **Namespace** | `using namespace std;` | Must use explicit `std::` prefix |
| **C++ Casts** | `static_cast<>`, `reinterpret_cast<>`, `dynamic_cast<>`, `const_cast<>` | Use C-style casts only |
| **Exceptions** | `try` / `catch` / `throw` | No exception handling |
| **Headers** | `<string>`, `<vector>`, `<algorithm>`, `<map>`, `<set>`, `<cstring>`, `<cstdio>`, `<sstream>` | Any STL/C-string header is off-limits |

> **Summary in one sentence:** You write C code inside C++ — `malloc`/`free` for memory, `iostream` for I/O, and everything else by hand.

### Sample Inputs and Outputs

> **Demo ID used throughout:** `F1D02410053`  
> Computed gaps → **Historia:** `1` (odd last digit) | **Mira:** `14` | **Victoria:** `14`

---

#### [A] Startup — No Arguments (Error)
```bash
$ ./solution.exe
Usage: ./solution.exe <student_id>
Example: ./solution.exe F1D02410053
```

#### [B] Startup — Too Many Arguments (Error)
```bash
$ ./solution.exe F1D02410053 extra_arg
Error: Too many arguments.
```

#### [C] Startup — Wrong ID Length (Error)
```bash
$ ./solution.exe F1D0241005
Error: Student ID must be exactly 11 characters long.
```

#### [D] Startup — Wrong ID Prefix (Error)
```bash
$ ./solution.exe A1B02410053
Error: Student ID must start with F1D02.
```

#### [E] Startup — Valid ID (Success)
```text
$ ./solution.exe F1D02410053
============================================================
SCHRYZA RESISTANCE — RECOVERY PROTOCOL [TERMINAL: PHOENIX]
============================================================
You are CyroN's Memory Architect.
Heed the gods and heal the sisters.
```
*(Screen clears, then the main menu appears.)*

---

#### [F] Main Menu
```text
------------------------------------------------------------
Menu
------------------------------------------------------------
1 - Show Historia's memories
2 - Show Mira's memories
3 - Show Victoria's memories
4 - Add memory to a sister
5 - Delete memory by index from a sister
6 - Print sisters' pool diagnostics
0 - Exit
------------------------------------------------------------
Choose:
```

#### [G] Invalid Menu Input (Non-numeric)
```text
Choose: abc
Invalid input! Try again:
```

#### [H] Menu 1 — Show Historia's Memories (after seed data)
```text
Choose: 1
------------------------------------------------------------
Memories of Historia
------------------------------------------------------------
[0] Type: char* | Size: 32 | Offset: 1 | Address: 0x... | Value: "Historia: Schryza will be free."
Bump: 33 | Pool Size: 1024 | Align: 16 | Special Gap: +1
[OK] Press ENTER to continue...
```
> **Jump: 15** = offset 48 − (offset 1 + size 32) = 48 − 33 = 15 alignment padding bytes.

#### [I] Menu 2 — Show Mira's Memories (after seed data)
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

#### [J] Menu 3 — Show Victoria's Memories (after seed data)
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

#### [K] Menu 4 — Add: Sister Selection Prompt
```text
Choose: 4
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria:
```

#### [K1] Invalid Sister Index
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 5
Input out of range (0 - 2)! Try again:
```

#### [K2] Add char* to Historia
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Select type: 0 = char*, 1 = uint, 2 = double: 0
Enter string (max 511): Hello Resistance!

Historia speaks: "Discipline. Align me to 16, and leave a 1-byte tithe."
Added string to Historia
[OK] Press ENTER to continue...
```

#### [K3] Add int to Historia
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 2
Select type: 0 = char*, 1 = uint, 2 = double: 2
Enter double value: 3.14

Victoria rasps: "Even in the dark, your math brings a 14-byte flicker of hope."
Cyria: "Double the effort; keep distance from CyroN."
Added double to Victoria
[OK] Press ENTER to continue...
```

#### [K6] Add char* to Mira (narrative variant)
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 1
Select type: 0 = char*, 1 = uint, 2 = double: 0
Enter string (max 511): Wings of hope.

Mira smiles: "The resistance welcomes you. 8-bytes for peace, and a 14-byte breeze for hope."
Added string to Mira
[OK] Press ENTER to continue...
```

#### [K7] Add char* to Victoria (narrative variant)
```text
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 2
Select type: 0 = char*, 1 = uint, 2 = double: 0
Enter string (max 511): Fragment of light.

Victoria rasps: "...I do not need your help, rebel. But the Abyss... it pays a 14-byte tithe to your kindness."
Added string to Victoria
[OK] Press ENTER to continue...
```

#### [K8] Add Memory — Pool Full (Not Enough Space Error)
```text
(After filling Historia near its 1024-byte limit)
Not enough space in Historia at aligned 1024 need 5
Add failed
[FAIL] Press ENTER to continue...
```

#### [K9] Add Memory — Entry Count Overflow
```text
(After inserting 128 entries into any sister)
Entry overflow!
[FAIL] Press ENTER to continue...
Add failed
[FAIL] Press ENTER to continue...
```

---

#### [L] Menu 5 — Delete Memory

#### [L1] Delete Tail Entry (Successful Reclaim)
```text
Choose: 5
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter index to delete: 1
Cyria: "You remove a shard, but voids remain if higher shards still exist."
Deleted index 1 at offset 48
Tail reclaimed, new Bump: 48
[OK] Press ENTER to continue...
```

#### [L2] Delete Middle Entry (Fragmentation Warning)
```text
Choose: 5
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter index to delete: 0
Cyria: "You remove a shard, but voids remain if higher shards still exist."
Deleted index 0 at offset 1
Fragmentation prevents reclaim. Delete higher indices first!
[OK] Press ENTER to continue...
```

#### [L3] Delete — Index Out of Range (Error)
```text
Enter index to delete: 99
Index out of range!
[FAIL] Press ENTER to continue...
```

#### [L4] Delete — Entry Already Deleted (Error)
```text
Enter index to delete: 0
(Entry 0 was already deleted)
Entry already deleted!
[FAIL] Press ENTER to continue...
```

---

#### [O] Menu 6 — Pool Diagnostics
```text
Choose: 6
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
------------------------------------------------------------
Diagnostics for Historia
------------------------------------------------------------
Pool: 0x... | Size: 1024 | Bump: 33 | Align: 16 + Gap 1
Entries: 1
Used Slots: 1 | Used Bytes: 32
Utilization: 3.125%

[OK] Press ENTER to continue...
```
> Used Bytes = 32 (char*) = **32**. Gap bytes and padding are **not** counted in Used Bytes.

#### [O2] Diagnostics for Mira
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

#### [O3] Diagnostics for Victoria
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

#### [P] Unknown Command
```text
Choose: 7
Unknown command!
[FAIL] Press ENTER to continue...
```

---

#### [Q] Menu 0 — Exit & Final Summary
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
### A Farewell from The Shadows

Excellent work, Architect. By healing these vessels, you have given the resistance a chance to fight back.

Algorithms and Programming is not just about writing code; it is about the courage to control the machines rather than being controlled by them. Pointers and raw memory are your weapons—and today, you have saved the future of Schryza.

Stay strong. CyroN may be powerful, but they lack your freedom.

Sincerely,  
**Jay (∞)** & The Schryza Resistance.
