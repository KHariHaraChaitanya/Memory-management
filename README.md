# Memory Management Comparison

A concise demonstration of memory‑management models in Go, Rust, and C++ using a unified “in‑memory cache” example. Each implementation highlights allocation and reclamation strategies at work.

---

## Table of Contents

1. [Overview](#overview)  
2. [Stack vs. Heap](#stack-vs-heap)  
3. [C/C++: Manual Allocation & RAII](#cc-manual-allocation--raii)  
4. [Go: Garbage‑Collected Heap](#go-garbage-collected-heap)  


---

## Overview

This repository presents side‑by‑side demos of a simple in‑memory cache in C, C++, Go, and Rust. By running each version, you can observe:

- How memory is allocated (stack vs. heap)  
- When and how unused data is reclaimed  
- The trade‑offs between manual control, RAII, garbage collection, and ownership models  

---

## Stack vs. Heap

- **Stack**  
  - Stores local variables and function call frames  
  - Allocation and deallocation occur automatically on scope entry and exit  
  - Very low overhead, but limited in size and lifetime  

- **Heap**  
  - Used for dynamic allocations whose lifetime extends beyond a single function call  
  - Requires explicit management (manual free, RAII, or automatic GC)  
  - More flexible, but comes with overhead and potential fragmentation  

---

## C/C++: Manual Allocation & RAII

In C and classic C++, memory management is the programmer’s responsibility:

- **Manual** (`malloc`/`free` or `new`/`delete`)  
  - Offers maximal performance and control  
  - Risks memory leaks, double‑free errors, and dangling pointers  

- **RAII** (`std::unique_ptr`, `std::shared_ptr`)  
  - Encapsulates resource lifetime in object scope  
  - Ensures deterministic destruction, reducing leaks and mismatched frees  

---

## Go: Garbage‑Collected Heap

Go automates heap reclamation through a concurrent, generational garbage collector:

1. **Allocation** remains explicit (`new`, `make`) but without manual frees.  
2. **Root scanning** identifies live objects via global variables, stack frames, and registers.  
3. **Mark phase** flags all reachable objects.  
4. **Sweep/compact phase** reclaims unmarked memory or compacts live objects.  

**Advantages**  
- Eliminates most manual‑free errors and dangling pointers  
- Simplifies application code  

**Considerations**  
- GC introduces non‑deterministic pause times  
- Runtime and tuning overhead for large heaps  

---
