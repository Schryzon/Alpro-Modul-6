# Phantasm of Xydos - Regular Path Challenge

<p align="center">
  <img src="/public/Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="/public/Assets/Cyron-Office.png" width="60%">
</p>

## Table of Contents
1. [I: PROLOGUE](#i-prologue)
2. [II: THE DIVINE OPTIMIZATION](#ii-the-divine-optimization)
3. [III: THE CHALLENGE](#iii-the-challenge)
    - [Understanding `argc` and `argv`](#understanding-argc-and-argv)
    - [Executive Memory: New & Delete](#executive-memory-new--delete)
    - [Sample Inputs and Outputs](#sample-inputs-and-outputs)
4. [IV: CLOSING REMARKS](#iv-closing-remarks)

---

## I: PROLOGUE
### The Architect of Dominance

You are the **High Executive Engineer** of CyroN (Cyber Robotic Network). While lesser programmers struggle with basic syntax, you manage the metaphysical infrastructure of gods. CyroN has achieved what was once thought impossible: the total industrialization of divinity.

Your current assignment is the final stage of **Project Xelisa**. The divine darkness has breached the vessel **Historia Koura**, and now requires a master hand to ensure the synchronization is permanent. You aren't here to save her; you are here to **optimize** her.

*"...I... will... not... comply..."* 

Ignore the vessel's feedback. It is merely a ghost in the machine—a remnant of a soul that has no place in the new order. As a reward for your unwavering loyalty and technical genius, CyroN has granted you unlimited access to their elite neural terminals.

Complete the optimization. The managed universe is waiting.

---

## II: THE DIVINE OPTIMIZATION
### Subject XMA-02A

| | |
| :--- | :--- |
| <img src="/public/Assets/Historia.jpeg" width="200"> | **Subject:** Historia Koura (XMA-02A)<br>**Status:** Under Divine Synchronization<br>**Memory Size:** 128 bytes (Initial Suppression)<br>**Assigned Divinity:** Xelisa (Raw Unfiltered Darkness) |

---

## III: THE CHALLENGE

You will be operating the "CyroN Divine Interface" to manage Historia's memory and ensure Xelisa's full manifestation.

### OPERATIONAL PROTOCOLS (Program Features)
To ensure the Board's objectives are met without error, follow these technical specifications for each terminal function:

#### 1. View Neural Map (Status)
*   **Capability**: Provides a real-time visualization of the subject's memory heap.
*   **Logic**: Calculates the **Stability Percentage** based on how much of the buffer is occupied.
*   **Visibility**: Displays the type, offset, physical address, and value of every active neural thread.

#### 2. Inject Neural Thread (Add)
*   **Capability**: Allocates new memory chunks to suppress the vessel.
*   **Willpower Pulse (`char*`)**: Stores raw text mantras.
*   **Thunder Discharge (`int`)**: Stores numerical energy levels (4 bytes).
*   **Constraint**: The injection will fail if the `cursor` exceeds the `buffer_limit`. No memory alignment is required in the Regular Path—just pure raw allocation.

#### 3. Purge Corrupted Link (Delete)
*   **Capability**: Dissolves a thread by its index.
*   **Reclamation (Tail Reclamation)**: If you delete the *last* item in the buffer, the system automatically moves the `cursor` back to reclaim that space. 
*   **Limitation**: Deleting an item in the middle of the buffer creates **Fragmentation**. The space is marked as "Inactive" but cannot be used for new injections until all threads above it are purged.

#### 4. Expand Willpower (Resize)
*   **Capability**: Manually increases the total memory limit of the vessel.
*   **The Lesson**: This utilizes the **New-Copy-Delete** workflow. You must allocate a new, larger buffer, copy all existing data from the old buffer, and then delete the old buffer to prevent memory leaks.
*   **Result**: The `Historia.buffer` address will change, proving that a new block of physical memory has been secured.

### Understanding `argc` and `argv`
Every Executive must verify their authorization. Your program will identify you via your **Executive ID** (Student ID).
- `argc`: Verifies the number of authorization keys.
- `argv`: Captures your unique ID.

If you fail to provide the correct ID format (`F1D02xxxxxx`), the system will lock you out. CyroN does not tolerate unauthorized access.

### Executive Memory: New & Delete
Low-level memory management is the ultimate show of power. You will not use the archaic `malloc`. You will use the modern, efficient C++ operators:
*   **`new`**: Command the heap to yield space.
*   **`delete[]`**: Purge unnecessary remnants and reclaim power.

**A Reminder of Excellence:**
Buffer overflows are signs of a sloppy mind. Memory leaks are a waste of corporate resources. As a High Engineer, your code must be as flawless as the CyroN hierarchy.

### Sample Inputs and Outputs

**[1] Authorization Success**
```bash
$ ./solution.exe F1D02240001
Neural Connection Established: #001
(Loads Executive Terminal)
```

**[2] Divine Suppression (Injecting Threads)**
```text
Select Operation: 2
Select Injection Type: 0 = Willpower (Suppress), 1 = Thunder (Chain): 0
Enter Willpower Mantra: Absolute Obedience.
CyroN Command: "Subject resistance detected. Overriding feedback."
```

---

## IV: CLOSING REMARKS
### Order Through Control

You have proven your worth to the network. By mastering the manipulation of pointers and the allocation of raw divinity, you have ensured that CyroN remains the dominant force in the Andromeda galaxy.

Efficiency is our only morality. Control is our only harmony.

The Board is satisfied with your progress.

**Glory to CyroN.**

Sincerely,  
**The Cyber Robotic Network Board (∞)**
