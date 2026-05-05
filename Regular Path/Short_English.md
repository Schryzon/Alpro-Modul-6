# Lab Report Module 6 — Regular Path
## CyroN Divine Interface: Neural Memory Management for Historia

### Program Description

This program is a text-based terminal (TUI) implementation called the **CyroN Divine Interface**, serving as a neural buffer management system for subject Historia Koura (XMA-02A). The program is launched with a single argument — a Student ID via `argv[1]` in the format `F1D02xxxxxx`. This ID is validated by the program and used to extract the operator frequency from its last three digits through pure arithmetic, without relying on any built-in conversion functions.

Memory management in this program uses modern C++ operators: `new` to allocate a dynamic buffer on the heap, and `delete[]` to release it when the program terminates. All data is maintained in a `Neural_Core` structure that holds the buffer pointer, its capacity limit (`buffer_limit`), the current write position (`cursor`), and an array of active entries (`Neural_Entry`).

---

### Argument Validation

Before the terminal is activated, the program validates its command-line arguments. If no Student ID is provided, if more than one argument is given, if the ID is not exactly 11 characters, or if it does not start with `F1D02`, the program denies access, prints an appropriate error message, and exits with `return 1`.

```text
$ ./solution.exe
Error: Neural Link requires an Operator ID (Student ID).
Usage: ./solution.exe <Student_ID>
```
*Figure 6.1 — Error handling when the program is run without a Student ID argument*

---

### Main Menu

Once validation passes and `init_core()` initializes the buffer with `new`, the program enters its main loop and presents the operation menu. A narrative quote from the character Xelisa is displayed above the menu, cycling through four different strings using a static counter variable.

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
*Figure 6.2 — CyroN Divine Interface main menu*

---

### Feature 2: Inject Neural Thread

The injection operation allows two types of data to be added to the buffer: a *Willpower Pulse* for text strings (`char*`) and a *Thunder Discharge* for integer values (`int`). Data is copied byte-by-byte into the heap buffer using pointer arithmetic, without using helper functions such as `memcpy` or `strcpy`.

Each successful injection records the data's starting offset (`start_pos`), allocated length (`len`), data type, and active status in the corresponding `Neural_Entry`, then advances the cursor by the size of the new data.

```text
Select Operation: 2
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 0
Enter Willpower Input: Absolute Obedience.
CyroN Command: "Subject resistance detected. Overriding feedback."
>> Press ENTER for next pulse...
```
*Figure 6.3 — Successful Willpower Pulse (text) injection into Historia's buffer*

If the data to be injected would exceed the remaining buffer capacity (`cursor + need > buffer_limit`), the operation is cancelled without modifying the buffer state, and an error message is displayed.

```text
Select Injection Type: 0 = Willpower (Text), 1 = Thunder (Level): 0
Enter Willpower Input: [input whose length exceeds remaining buffer capacity]
!! OPTIMIZATION ERROR !! The vessel's ego rejected the thread!
>> Press ENTER for next pulse...
```
*Figure 6.4 — Injection failure handling due to buffer capacity overflow*

---

### Feature 1: View Neural Map

After successful injections, the buffer contents can be inspected via the View Neural Map option. The program calculates the **Stability Percentage** using the formula `100 - (100 * cursor / buffer_limit)` and displays all active entries with their type, offset, physical heap address, and stored value.

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
*Figure 6.5 — Neural Map after two entries are injected, showing each entry's physical offset and address*

---

### Feature 3: Purge Corrupted Link

The purge operation deactivates an entry by index (`threads[idx].active = false`). If the purged entry is the last active entry in the buffer (the "tail"), the system automatically recalculates the cursor position back to the end of the highest remaining active entry — a process called **Tail Reclamation**, which makes that space immediately available for future injections.

```text
Select Operation: 3
Enter Link Index to purge: 1
Link 1 dissolved into ether.
Neural Core reclaimed space. Tail at: 20
>> Press ENTER for next pulse...
```
*Figure 6.6 — Tail Reclamation: cursor retreats after the last active entry is purged*

Conversely, if there are still active entries at higher indices, the reclaimed space becomes **fragmented** and cannot be recovered. This occurs when a non-tail entry is deleted while entries above it are still active.

```text
Enter Link Index to purge: 0
Link 0 dissolved into ether.
Fragmentation detected. Cannot reclaim space yet!
>> Press ENTER for next pulse...
```
*Figure 6.7 — Memory fragmentation caused by purging a non-tail entry while higher entries remain active*

---

### Feature 4: Expand Willpower

The Expand Willpower feature implements the core concept of this module: the **New-Copy-Delete** workflow. The program allocates a new, larger buffer with `new`, manually copies all old data into it byte-by-byte, then frees the old buffer with `delete[]`. The success of this process is proven by the changed physical address of the buffer displayed after the operation completes.

```text
Select Operation: 4
Enter new buffer limit: 512
CyroN Command: "Stability increased. The vessel is now 100% compliant."
Allocated Frequency: 0x7f9a3c000b20
>> Press ENTER for next pulse...
```
*Figure 6.8 — Successful buffer expansion; the changed address proves a new heap block was allocated*

---

### Program Termination

When the operator selects option 0, the program displays a closing narrative, waits for ENTER confirmation, then calls `destroy_core()` which releases all heap memory via `delete[]` and sets the pointer to `nullptr`. The program then prints a farewell message including the operator frequency extracted from the Student ID.

```text
Select Operation: 0
Reality destabilizes...
>> Press ENTER for next pulse...
Connection terminated. Good bye, Operator 001.
```
*Figure 6.9 — Program termination sequence and heap memory release*
