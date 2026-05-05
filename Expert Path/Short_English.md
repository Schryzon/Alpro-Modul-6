# Lab Report Module 6 — Expert Path
## Schryza Recovery Terminal: Multi-Pool Memory Management for the Koura Sisters

### Program Description

This program is a text-based terminal implementation called the **Schryza Recovery Terminal** [RECOVERY PROTOCOL: PHOENIX], serving as a multi-pool memory management system for three characters: Historia, Mira, and Victoria Koura. Each character owns a separate memory pool managed using C standard library functions — `malloc()`, `realloc()`, and `free()` — from `<cstdlib>`.

The program accepts a single Student ID argument via `argv[1]` in the format `F1D02xxxxxx`. The ID is validated, then its last three digits are extracted mathematically to compute a unique **Special Gap** for each character. This gap directly influences memory layout and is represented through a `union Gap` that holds a different field type per character.

---

### Initialization and Seed Data

Once the Student ID is validated, the program initializes three memory pools with `malloc()` at fixed sizes: Historia (1024 bytes), Mira (2048 bytes), Victoria (4096 bytes). Each pool carries distinct alignment requirements and a gap derived from the Student ID. Immediately after initialization, five seed entries are pre-loaded across the three pools as a baseline for demonstration. The program then presents the main menu with eight operations.

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
*Figure 6.1 — Initialization banner and main menu of the Schryza Recovery Terminal*

---

### Features 1–3: Show Memories and Memory Alignment

The technical core of this program is **memory alignment**. Every allocation is placed at an offset aligned to the character's specific boundary using the `align_up()` function. For `char*` allocations specifically, an additional **Special Gap** is prepended before the data — this gap is computed from the Student ID.

With Student ID `F1D02410053` (last three digits: `053`):
- **Historia** (align 16, gap `1`): `char*` is placed at `align_up(bump, 16) + 1`
- **Mira** (align 8, gap `14`): `char*` is placed at `align_up(bump, 8) + 14`
- **Victoria** (align 4, gap `14`): `char*` is placed at `align_up(bump, 4) + 14`

The **Jump** column in the output shows the byte distance between the end of the previous entry and the start of the current one — this is the actual alignment padding visible in memory.

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
*Figure 6.2 — Historia's memory view revealing alignment offsets, Special Gap placement, and inter-entry Jump values*

---

### Feature 4: Add Memory

When adding a new entry, the program first computes the aligned offset and appends the gap if the type is `char*`. The data is then copied byte-by-byte into the pool. A character-specific narrative response is displayed after the allocation confirms the alignment contract.

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
*Figure 6.3 — Adding a char\* entry to Mira, illustrating the gap and null terminator in the narrative response*

If the pool does not have sufficient space to accommodate the new data after alignment and gap are applied, the operation is aborted and a failure message is displayed without modifying the pool state.

```text
Not enough space in Historia at aligned 1024 need 5
Add failed
[FAIL] Press ENTER to continue...
```
*Figure 6.4 — Memory allocation failure when the pool is exhausted*

---

### Feature 5: Delete Memory and Tail Reclamation

Memory deletion marks an entry as inactive (`used = false`) without physically erasing the data from the pool. If the deleted entry was the last active one (the "tail"), the `bump` pointer is reclaimed backward to the end position of the highest remaining active entry.

```text
Choose: 5
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter index to delete: 1
Cyria: "You remove a shard, but voids remain if higher shards still exist."
Deleted index 1 at offset 48
Tail reclaimed, new Bump: 48
[OK] Press ENTER to continue...
```
*Figure 6.5 — Successful Tail Reclamation in Historia: bump retreats after the tail entry is deleted*

If active entries with higher indices still exist, the freed space becomes **fragmented** and the `bump` cannot be reclaimed — the user must purge from the top down.

```text
Enter index to delete: 0
Cyria: "You remove a shard, but voids remain if higher shards still exist."
Deleted index 0 at offset 1
Fragmentation prevents reclaim. Delete higher indices first!
[OK] Press ENTER to continue...
```
*Figure 6.6 — Fragmentation in Historia's pool: deleting a non-tail entry leaves an orphaned gap the bump cannot cross*

---

### Feature 6: Pool Reallocation

A sister's pool can be resized at runtime using `realloc()`. If the pool is shrunk below the end offset of existing entries, the program emits a per-entry out-of-bounds warning to alert the operator of data that is now beyond the new pool boundary.

```text
Choose: 6
Choose sister: 0 = Historia, 1 = Mira, 2 = Victoria: 0
Enter new pool size (bytes): 30
Historia pool resized to 30 bytes
Warning: entry 0 now out of bounds!
Warning: entry 1 now out of bounds!
[OK] Press ENTER to continue...
```
*Figure 6.7 — Out-of-bounds warning when Historia's pool is shrunk below existing entry offsets*

---

### Feature 8: Pool Diagnostics

The Diagnostics menu prints a comprehensive utilization summary for a selected sister. It reports the number of active slots, the total bytes actually occupied by live data, and the pool utilization percentage. Notably, alignment padding and Special Gap bytes consume pool space but are excluded from the "Used Bytes" count — they represent overhead rather than stored data.

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
*Figure 6.8 — Pool diagnostics for Historia: utilization calculated from raw data bytes only, excluding padding overhead*

---

### Program Termination and Final Summary

When option 0 is selected, the program exits the main loop and prints a full diagnostic summary for all three sisters, each preceded by its narrative story banner. It then calls `free()` on each pool and prints a closing message.

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
*Figure 6.9 — Final summary on exit: Historia's diagnostics and the god-blessing message before all pools are freed*
