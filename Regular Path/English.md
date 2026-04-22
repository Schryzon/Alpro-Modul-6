# Phantasm of Xydos - Regular Path Challenge

<p align="center">
  <img src="../Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="../Assets/Cyron-Office.png" width="60%">
</p>

## Table of Contents
1. [I: LOGS](#i-logs)
2. [II: SUBJECT: HISTORIA (XMA-02A)](#ii-subject-historia-xma-02a)
3. [III: THE ASSIGNMENT](#iii-the-assignment)
    - [Operational Protocols](#operational-protocols)
    - [Memory Engineering Workflow](#memory-engineering-workflow)
    - [Expected Output](#expected-output)
4. [IV: FINAL COMPLIANCE](#iv-final-compliance)

---

## I: LOGS
### High Executive Memory Engineer — CyroN

Congratulations, Citizen. Your performance in the neural drafting simulations has earned you a promotion. You are now a **High Executive Engineer** for the Cyber Robotic Network (CyroN). 

In the Andromeda Galaxy, divinity is a resource to be optimized. The "Koura Sisters" were once human, but CyroN has ascended them into divine machine vessels. However, their free will is a bug—a logical inconsistency that slows down galactic progress.

Your task is simple: Maintain the neural buffer of Subject Historia. Suppress the chaotic fluctuations of her consciousness. Ensure that CyroN's "Divine Optimization" protocol is absolute.

Work hard. Efficiency is the highest form of worship.

---

## II: SUBJECT: HISTORIA
### Designation: XMA-02A

| | |
| :--- | :--- |
| <img src="../Assets/Historia.jpeg" width="200"> | **Name:** Historia Koura (XMA-02A)<br>**Status:** Integrated Vessel<br>**Xydos:** Lagta (Thunder/Abyss)<br>**Memory Threshold:** 1024 bytes (Initial Allocation)<br>**Priority:** Alpha |

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
