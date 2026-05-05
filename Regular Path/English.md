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
    <img src="https://img.shields.io/badge/PLAY_CYRON_EXECUTIVE_THEME-00509E?style=for-the-badge&logo=youtube&logoColor=white" alt="Play BGM">
  </a>
  <br><br>
  <a href="../Story/English_STORY.md">
    <img src="https://img.shields.io/badge/READ_THE_FULL_NARRATIVE_LORE-5A228B?style=for-the-badge&logo=readme&logoColor=white" alt="Read Lore">
  </a><br>
  <i>(Highly recommended before you start the challenge!)</i>
</p>
<br>

## Table of Contents
1. [I: LOGS](#i-logs)
2. [II: SUBJECT: HISTORIA (XMA-02A)](#ii-subject-historia-xma-02a)
3. [III: THE CHALLENGE](#iii-the-challenge)
    - [Operational Protocols](#operational-protocols-program-features)
    - [Understanding `argc` and `argv`](#understanding-argc-and-argv)
    - [Executive Memory: New & Delete](#executive-memory-new--delete)
    - [Sample Inputs and Outputs](#sample-inputs-and-outputs)
4. [IV: CLOSING REMARKS](#iv-closing-remarks)

---

## I: LOGS
### High Executive Memory Engineer — CyroN

Congratulations, junior technician. Your performance in the neural network simulations has been exemplary, earning you a promotion. You are now officially a **High Executive Engineer** for the Cyber Robotic Network (CyroN). 

In the Andromeda Galaxy, divine power is a resource that must be maximized. The "Koura Sisters" were once human, but CyroN has transformed them into vessels for these divine machines. Unfortunately, their free will is considered a bug—a logical inconsistency that slows down galactic progress.

Your task is fairly simple: Maintain the neural buffer of Subject Historia. Suppress the chaotic fluctuations of her consciousness. Ensure that CyroN's "Divine Optimization" protocol remains absolute and flawless.

Work efficiently, because efficiency is the highest form of loyalty towards CyroN.

---

## II: SUBJECT: HISTORIA
### Designation: XMA-02A

| | |
| :--- | :--- |
| <img src="../Assets/Historia.jpeg" width="200"> | **Name:** Historia Koura (XMA-02A)<br>**Status:** Integrated Vessel<br>**Xydos:** Lagta (Thunder/Abyss)<br>**Memory Limit:** 128 bytes (Initial Allocation)<br>**Priority:** High |

---

## III: THE CHALLENGE

Your primary objective is to operate the "CyroN Divine Interface" (Terminal) to manage Historia's memory and ensure Xelisa's full manifestation runs smoothly.

### OPERATIONAL PROTOCOLS (Program Features)
To ensure the Board of Directors' objectives are met without error, follow these technical specifications for each terminal function:

#### 1. View Neural Map (Status)
*   **Function**: Displays a real-time visualization of the subject's memory heap.
*   **Logic**: Calculates the **Stability Percentage** based on how full the buffer is.
*   **Output**: Shows the type, offset, physical address, and value of every active neural thread.

#### 2. Inject Neural Thread (Add)
*   **Function**: Allocates new memory blocks to suppress the vessel's consciousness.
*   **Willpower Pulse (`char*`)**: Stores raw text mantras.
*   **Thunder Discharge (`int`)**: Stores numerical energy levels (4 bytes).
*   **Constraint**: The injection will fail if the `cursor` exceeds the `buffer_limit`. No complex memory alignment is required in the Regular Path—just standard memory allocation.

#### 3. Purge Corrupted Link (Delete)
*   **Function**: Deletes a thread by its index.
*   **Reclamation (Tail Reclamation)**: If you delete the **last** item in the buffer, the system automatically moves the `cursor` backward to reclaim that memory space. 
*   **Limitation**: If you delete an item in the middle of the buffer, it creates **Fragmentation**. That space will be marked as "Inactive" and cannot be used for new injections until all threads above it are purged.

#### 4. Expand Willpower (Resize)
*   **Function**: Manually increases the total memory limit of the vessel.
*   **Core Concept**: This feature uses the **New-Copy-Delete** workflow. You must allocate a new, larger buffer, copy all data from the old buffer, and then delete the old buffer to avoid memory leaks.
*   **Result**: The address of `Historia.buffer` will change, proving that a new physical memory block has been successfully allocated.

### Understanding `argc` and `argv`
Every Executive must verify their authorization. The program will identify you via your **Executive ID** (Student ID).
- `argc`: Verifies the number of authorization keys.
- `argv`: Captures your unique ID.

If you input an incorrect ID format (`F1D02xxxxxx`), the system will block your access. CyroN has zero tolerance for unauthorized access.

### Executive Memory: New & Delete
Low-level memory management is proof of true skill. You will not use the archaic `malloc` from C. You are required to use modern C++ operators, which are far more efficient:
*   **`new`**: Commands the heap memory to provide space.
*   **`delete[]`**: Cleans up unused resources and returns the memory to the system.

**Important Reminder:**
Buffer overflows are a sign of a careless programmer. Memory leaks are a waste of corporate assets. As a High Executive Engineer, your code must be as pristine and perfect as CyroN's.

### Sample Inputs and Outputs

> **Demo ID used throughout:** `F1D02240001` → Operator frequency `#001`

---

#### [A] Startup — No Arguments (Error)
```bash
$ ./solution.exe
Error: Neural Link requires an Operator ID (Student ID).
Usage: ./solution.exe <Student_ID>
```

#### [B] Startup — Too Many Arguments (Error)
```bash
$ ./solution.exe F1D02240001 extra_arg
Error: Too many parameters. Connection unstable.
```

#### [C] Startup — Wrong ID Length (Error)
```bash
$ ./solution.exe F1D0224001
Error: Operator ID must be exactly 11 characters long.
```

#### [D] Startup — Wrong ID Prefix (Error)
```bash
$ ./solution.exe A1B02240001
Error: Operator ID must start with 'F1D02'.
```

#### [E] Startup — Valid ID (Success)
*(Loads directly into main loop after `init_core()`.)*
```bash
$ ./solution.exe F1D02240001
```
*(Screen clears, Xelisa whisper appears, then the main menu.)*

---

#### [F] Main Menu (First Load)
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
> The Xelisa whisper cycles through 4 different quotes, rotating every time the menu is displayed.

#### [G] Main Menu — Invalid (Non-numeric) Input
```text
Select Operation: hello
(Menu redraws; invalid input is silently skipped by cin error recovery)
```

---

#### [H] Option 1 — View Neural Map (Empty Buffer)
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

#### [H2] Option 1 — View Neural Map (After Adding Data)
```text
============================================================
NEURAL MAP: HISTORIA KOURA [STABILITY: 75%]
============================================================
[0] TYPE: Willpower Pulse | OFFSET: 0 | ADDR: 0x... | DATA: "Absolute Obedience."
[1] TYPE: Thunder Discharge | OFFSET: 20 | ADDR: 0x... | DATA: 777MW
------------------------------------------------------------
Cursor: 24 / 128 Bytes used.
------------------------------------------------------------
>> Press ENTER for next pulse...
```
> **Stability %** = `100 - (100 * cursor / buffer_limit)`. With 32 bytes used out of 128 → `100 - (100*32/128) = 75%`.

---

#### [I] Option 2 — Inject Neural Thread: Willpower (Text) Successful
```text
Select Operation: 2
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 0
Enter Willpower Input: Absolute Obedience.
CyroN Command: "Subject resistance detected. Overriding feedback."
>> Press ENTER for next pulse...
```
*(Returns to main menu loop.)*

#### [I2] Option 2 — Inject Willpower — Buffer Overflow (Error)
```text
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 0
Enter Willpower Input: [A very long string that doesn't fit]
!! OPTIMIZATION ERROR !! The vessel's ego rejected the thread!
>> Press ENTER for next pulse...
```
*(Returns to main menu without modifying the buffer.)*

#### [I3] Option 2 — Inject Thunder (int) Successful
```text
Select Operation: 2
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 1
Enter Thunder Level (int): 777
Daiki: "(Silence. The wind has been harnessed.)"
>> Press ENTER for next pulse...
```

#### [I4] Option 2 — Inject Thunder — Buffer Overflow (Error)
```text
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 1
Enter Thunder Level (int): 9999
!! OPTIMIZATION ERROR !! Neural burnout detected!
>> Press ENTER for next pulse...
```
*(Returns to main menu without modifying the buffer.)*

---

#### [J] Option 3 — Purge Corrupted Link (Delete)

#### [J1] Purge Tail Entry (Space Reclaimed)
```text
Select Operation: 3
Enter Link Index to purge: 1
Link 1 dissolved into ether.
Neural Core reclaimed space. Tail at: 20
>> Press ENTER for next pulse...
```

#### [J2] Purge Middle Entry (Fragmentation)
```text
Select Operation: 3
Enter Link Index to purge: 0
Link 0 dissolved into ether.
Fragmentation detected. Cannot reclaim space yet!
>> Press ENTER for next pulse...
```

#### [J3] Purge — Invalid Index / Already Dissolved (Error)
```text
Enter Link Index to purge: 99
Invalid link or already dissolved.
>> Press ENTER for next pulse...
```

---

#### [K] Option 4 — Expand Willpower (Resize)

#### [K1] Expand to Larger Size (Success)
```text
Select Operation: 4
Enter new buffer limit: 512
CyroN Command: "Stability increased. The vessel is now 100% compliant."
Allocated Frequency: 0x...
>> Press ENTER for next pulse...
```
> Note: The buffer address **will change**, proving a new heap block was allocated.

#### [K2] Expand — New Size Not Larger (Error)
```text
Enter new buffer limit: 64
Expansion must be larger than current limit!
>> Press ENTER for next pulse...
```
*(Returns to main menu without modifying the buffer.)*

---

#### [L] Option 0 — Surrender (Exit)
```text
Select Operation: 0
Reality destabilizes...
>> Press ENTER for next pulse...
Connection terminated. Good bye, Operator 001.
```

---

## IV: CLOSING REMARKS
### Order Through Control

You have proven your competence to the CyroN network. Through the manipulation of pointers and the memory allocation of divine power, you have helped ensure that CyroN remains the dominant force in the Andromeda Galaxy.

Efficiency is our only guideline. Absolute control is the only path to harmony.

The Board of Directors is extremely satisfied with your performance.

**Glory to CyroN.**

Sincerely,  
**The Cyber Robotic Network Board**
