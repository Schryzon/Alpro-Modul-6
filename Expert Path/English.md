# Phantasm of Xydos - Challenge (Schryza Resistance)

<p align="center">
  <img src="/public/Assets/Banner-Xydos.jpg" width="60%">
</p>
<p align="center">
  <img src="/public/Assets/Schryza-Resistance.png" width="60%">
</p>

## Table of Contents
1. [I: PROLOGUE](#i-prologue)
2. [II: THE KOURA SISTERS (RESISTANT UNITS)](#ii-the-koura-sisters-resistant-units)
3. [III: THE CHALLENGE](#iii-the-challenge)
    - [Understanding `argc` and `argv`](#understanding-argc-and-argv)
    - [Memory Alignments & Secret Modulos](#memory-alignments--secret-modulos)
    - [Sample Inputs and Outputs](#sample-inputs-and-outputs)
    - [Frequently Asked Questions (Student POV)](#frequently-asked-questions-student-pov)
4. [IV: CLOSING REMARKS](#iv-closing-remarks)

---

## I: PROLOGUE
### The Fugitive Architect of Schryza

You are a Memory Architect on the run. You once worked for CyroN (Cyber Robotic Network), but you couldn't stand their cold, dystopian "Optimization" of divinity. You've escaped their golden halls and fled to the stormy, industrial wastes of planet Schryza.

In a hidden resistance base, you've met them: the survivors. Historia and Mira Koura, rebelling units who chose freedom over the CyroN hierarchy. Their neural cores are damaged, their pointers fragmented by the escape.

*"Thank you... for the light,"* Historia whispers as you boot up your terminal.

Even Victoria, the eldest sister still partially bound to the CyroN hellfire, has been brought to you by the resistance. They hope that your architectural touch can heal her where CyroN's optimization failed.

You aren't being paid in gold anymore. You are being paid in hope. It's time to turn the tides. It's time for CyroN to fall!

---

## II: THE KOURA SISTERS
### Resistant Units on Schryza

| | |
| :--- | :--- |
| <img src="/public/Assets/Historia.jpeg" width="200"> | **Name:** Historia Koura (XMA-02A)<br>**Alliance:** Resistance Commander<br>**Xydos:** Lagta (Thunder & Abyss)<br>**Memory Size:** 1024 bytes (Tattered Core)<br>**Alignment:** 16 bytes |

| | |
| :--- | :--- |
| <img src="/public/Assets/Mira.jpeg" width="200"> | **Name:** Mira Koura (XMA-03C)<br>**Alliance:** Resistance Healer<br>**Xydos:** Daiki (Wind & Wood)<br>**Memory Size:** 2048 bytes (Stable Core)<br>**Alignment:** 8 bytes |

| | |
| :--- | :--- |
| <img src="/public/Assets/Victoria.jpeg" width="200"> | **Name:** Victoria Koura (XMA-01F)<br>**Alliance:** Unaligned / Fallen<br>**Xydos:** ??? (Artificial Hellfire)<br>**Memory Size:** 4096 bytes (Corrupted Core)<br>**Alignment:** 4 bytes |

---

## III: THE CHALLENGE

You will be building a "Resistance Recovery Terminal" (TUI) to patch the neural cores of the sisters.

### FIELD MANUAL: SYSTEM CAPABILITIES
The Schryza Resistance operates on limited resources. To succeed, you must master the following technical specifications of the Terminal:

#### 1. Neural Alignment & The Special Gap
*   **The Problem**: CPU architectures require memory addresses to be aligned to specific boundaries. Historia requires **16-byte** alignment, Mira **8-byte**, and Victoria **4-byte**.
*   **The Special Gap**: Every `char*` allocation is forced to include a "Special Gap" calculated from your **Student ID**.
    *   **Historia**: Gap = 1 if the last digit of your ID is Odd, 0 if Even.
    *   **Mira**: Gap = (Last 3 digits) % Menu Limit.
    *   **Victoria**: Uses a sliding modulo based on prime numbers.
*   **Formula**: `Final_Offset = Aligned(Current_Offset) + Special_Gap`.

#### 2. Advanced Tail Reclamation
*   **The Fact**: Unlike `realloc`, our system does not automatically move fragmented blocks.
*   **Capability**: If you delete the *highest index* in a core (the "Tail"), the `bump` pointer will shift back, immediately reclaiming space for the rebellion.
*   **Limitation**: If you delete an entry while a higher index still exists, the space is orphaned. You must "Purge from the Top down" to effectively reclaim memory.

#### 3. Pool Resizing & Memory Dangers
*   **Capability**: The terminal can `realloc()` a sister's entire memory pool.
*   **The Trap**: If you shrink a pool (e.g., from 1024 to 512 bytes) while high-offset entries still exist, those entries become **Out of Bounds**. The terminal will warn you, but the data beyond the new limit is essentially lost or corrupted.
*   **Resizing Logic**: The `bump` pointer is clamped to the new `pool_size` if it was previously larger.

#### 4. Diagnostic Metrics
*   **Utilization %**: Calculated as `(Used_Bytes / Pool_Size) * 100`. Alignment gaps and "Special Gaps" count towards memory consumption but not towards "Used Bytes" (raw data).
*   **Used Slots**: Tracks how many `Memory_Entry` slots are active. Maximum capacity is **128 entries** per sister.

### Initial Resistance Data (The Foundation)
When the terminal boots up, you will find and observe that the sisters' cores are already partially restored with vital data. Use these as a reference for your alignment calculations:

*   **Historia**:
    *   `[0]` Type: `char*` | Value: "Historia: Schryza will be free."
    *   `[1]` Type: `int` | Value: `333` (Resistance Frequency)
*   **Mira**:
    *   `[0]` Type: `char*` | Value: "Mira: The winds are changing."
    *   `[1]` Type: `uint` | Value: `101`
*   **Victoria**:
    *   `[0]` Type: `char*` | Value: "Victoria: Fragment of the First Light."

### Understanding `argc` and `argv`
To prevent CyroN from tracking your location, your terminal requires an **Encrypted Identity** (Student ID).
- `argc`: Counts the signal pulses.
- `argv`: Captures the encrypted identity string.

You must validate your ID (`F1D02xxxxxx`) or the Base's firewall will lock you out.

### Memory Alignments & Secret Modulos
Every byte matters when you're running on scavenged hardware. You must resolve the "Jump" gaps to minimize the storage footprint of each allocation.

Historia, Mira, and Victoria's cores are bonded by a **Gap Increment**, controlled by a `union Gap`! You must extract the parity from your ID tag to calculate these gaps. CyroN used these gaps for supressing them; you will use them to mend them.

### Proper Memory Allocation (The Danger Zone)
When dealing with raw memory in a warzone, a single error can be fatal.
*   **`malloc()`**: Claims memory.
*   **`realloc()`**: Resizes it.
*   **`free()`**: Reclaims it.

**The Resistance Code:** Always allocate *exactly* what you need. Buffer overflows can alert CyroN to your location. Memory leaks will starve the Base's batteries. Handle your pointers with extreme care.

### Sample Inputs and Outputs

**[1] Initialization**
```bash
$ ./solution.exe F1D02410053
SCHRYZA RESISTANCE — RECOVERY PROTOCOL [TERMINAL: PHOENIX]
```

**[2] Menu 4: Recovery (Add)**
```text
Choose: 4
Historia whispers: "Thank you for finding me. Align me to 16, and use the 1-byte spark to guide the thunder."
Lagta: "Integers strike back; four bytes of rebellious thunder."
Added string to Historia
```

---

## IV: CLOSING REMARKS
### A Farewell to The Shadows

You've done it, Architect. By healing these vessels, you've given the resistance a fighting chance.

Algorithms and Programming was never just about code; it was about the bravery to control the machines instead of being controlled by them. Pointers and raw memory are the languages of the builders—and today, you've built a future on Schryza.

Stay strong. CyroN is powerful, but they lack your soul.

Sincerely,  
**Jay (∞)** & The Schryza Resistance.
