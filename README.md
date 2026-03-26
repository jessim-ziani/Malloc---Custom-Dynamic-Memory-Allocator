# Malloc - Custom Dynamic Memory Allocator in C

> **Privacy Note:** The source code of this project is kept private to comply with EPITA's anti-plagiarism regulations. The code is structured, commented, and available upon request for recruiters during an interview. This repository serves as a technical showcase and provides a testable compiled version of the library.

## 📖 Project Context

The **Malloc** project is a fundamental pillar of the EPITA engineering curriculum. The objective is to fully recreate the standard dynamic memory management library (`libc`). This project requires a deep understanding of system-level memory management, data alignment, and resource optimization via low-level system calls.

**My Objectives:**
- Replace standard allocation functions with a high-performance custom implementation.
- Manage memory page allocation directly via the `mmap` system call.
- Guarantee strict data alignment to maximize CPU performance.

## ✨ Implemented Features

All of the following features were developed in C and are fully operational:

* **Standard Libc Functions:**
    * `malloc(size_t size)`: Allocation of aligned memory blocks.
    * `free(void *ptr)`: Memory deallocation and return to the system via `munmap` if the page is empty.
    * `calloc(size_t nmemb, size_t size)`: Zero-initialized allocation with overflow protection.
    * `realloc(void *ptr, size_t size)`: Dynamic resizing of blocks with existing data preservation.
* **Optimized Memory Management:**
    * **Block Splitting:** Carving out smaller chunks from large blocks to limit internal fragmentation.
    * **Block Fusion:** Immediate bidirectional fusion of adjacent free blocks to optimize reuse.
    * **Strict Alignment:** Systematic alignment (on `long double` size) to ensure hardware compatibility.
* **System Interactions:**
    * Use of `mmap` and `munmap` for memory acquisition and release from the kernel.
    * Dynamic detection of system page size via `sysconf`.

## 🏗️ Technical Architecture

The allocator is based on a doubly linked list structure managing precise metadata:

1.  **Metadata Management (`struct Space`):** Each block is preceded by a structure storing its state (free/occupied), size, and pointers to neighboring blocks for navigation and fusion.
2.  **Allocation Strategy (First-Fit):** `malloc` traverses the list of existing blocks to find the first sufficient free space. If no block fits, a new page is requested from the system via `mmap_init`.
3.  **Alignment Logic:** An `aligner` function calculates the necessary padding so that each returned pointer respects alignment constraints.

## 🧠 Technical Challenges Solved

* **Fragmentation Reduction:** Implementation of fusion algorithms (`prev` / `next`) in the `free` function to merge free spaces.
* **Pointer Arithmetic:** Rigorous navigation between control structures and the user memory area to avoid overlaps.
* **System Efficiency:** Optimization of `mmap` calls by allocating blocks aligned with the system page size.

## 🛠️ Technologies Used

* **Language:** C (Standard C99)
* **Build System:** Makefile
* **System APIs:** POSIX (`mmap`, `munmap`, `sysconf`)
* **Testing:** Automated Bash test suite (`testsuite.sh`) validating non-regression.

## 🎯 How to test the project?

A pre-compiled version of the shared library (`libmalloc.so`) is available. You can inject it into any Linux binary to replace the default `libc` allocator.

1.  Go to the **[Releases](../../releases)** section.
2.  Download the **`libmalloc.so`** file.
3.  Use `LD_PRELOAD` to test the library with system commands:

```bash
# Test with the 'ls' command
LD_PRELOAD=./libmalloc.so ls -la

# Test with 'factor' or 'cat'
LD_PRELOAD=./libmalloc.so factor 20 30 40
LD_PRELOAD=./libmalloc.so cat Makefile
```

## 👤 Developer

This project was entirely designed and developed solo as part of the EPITA engineering curriculum.

<div align="center">
  <table>
    <tr>
      <td align="center">
        <a href="https://github.com/jessim-ziani">
          <img src="https://github.com/jessim-ziani.png" width="60px" style="border-radius: 50%;" alt="Jessim Ziani"/><br />
          <sub><b>Jessim Ziani</b></sub>
        </a>
      </td>
    </tr>
  </table>
</div>

---
**🛠️ Technologies:** C (C99), Make, POSIX (mmap), Bash (Tests), Git.
