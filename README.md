# Alpro Module 6: Pointers and Memory Engineering
## Phantasm of Xydos: Andromeda I

<p align="center">
  <img src="Assets/Banner-Xydos.jpg" width="80%">
</p>

Welcome to the official repository for **Algorithms & Programming Module 6**. This module focuses on the most powerful (and dangerous) aspect of C++: **Pointers and Dynamic Memory Management**.

Structured as a choice between two polar opposite paths, this assignment challenges students to build a memory management interface while navigating the dystopian conflicts of the Andromeda Galaxy.

---

## 🌗 The Two Paths

Students must choose one architectural path based on their technical ambition and narrative alignment:

### 🏢 Regular Path: The CyroN Loyalist (Executive)
*   **Narrative**: Serve the Cyber Robotic Network as a High Executive Engineer. Efficiency and control are your only goals.
*   **Technical Focus**: 
    - Basic Pointer Arithmetic.
    - Modern C++ Memory Operators (`new`, `delete[]`).
    - Manual Buffer Resizing (The New-Copy-Delete workflow).
    - Simplified single-buffer management for Historia.
*   **Scenario**: [OPERATION: DIVINE OPTIMIZATION]

### ⚔️ Expert Path: The Schryza Resistance (Rebel)
*   **Narrative**: Join the Resistance on Planet Schryza. Risk everything to save the divine vessels from CyroN's chains.
*   **Technical Focus**:
    - Advanced Pointer Arithmetic.
    - Standard C Memory Management (`malloc`, `free`, `realloc`).
    - Multiple Heap Pool management.
    - **Memory Alignment (16/8/4-byte)** and ID-based Offset Calculations.
    - Tail Reclamation & Cluster Management.
*   **Scenario**: [RECOVERY PROTOCOL: PHOENIX]

---

## 📁 Repository Structure

```text
public/
├── Regular Path/          # Executive Path: Code & Detailed Docs
├── Expert Path/           # Resistance Path: Code & Detailed Docs
├── Assets/                # Visual dossiers and banners
└── FULL_LORE.md           # The comprehensive history of Xydos and CyroN
```

---

## 🛠️ Technical Requirements

*   **Compiler**: `g++` (MinGW-w64 recommended) or any C++11/14/17 compatible compiler.
*   **Standard Library**: Strictly limited use of standard libraries to encourage manual memory manipulation.
*   **Flags**: `-O3` recommended to observe performance during diagnostics.

---

## 📜 The Lore

The Andromeda galaxy is failing. As **CyroN** (Cyber Robotic Network) attempts to industrialize the gods, the "Historia Resistance" fights for a future based on free will. Whether you choose to optimize order or restore freedom, your code will decide the fate of the **Koura Sisters**.

For the full architectural history of the galaxy, read the [FULL_LORE.md](FULL_LORE.md).

---

> "In the world of raw memory, there are no safety nets. Only your pointers determine your reality."  
> — **Jay (∞)**
