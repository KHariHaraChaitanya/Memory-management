# Memory-management
Memory management comparison of Go, Rust, C++ 

# Stack vs Heap Memory

This repository demonstrates core memory‑management models across four languages (C, C++, Go, Rust) using a uniform “in‑memory cache” workflow. Each demo highlights how allocation and cleanup happen under the hood.

---

## Introduction

### Manual Memory Management in C/C++
In C and traditional C++, every heap allocation and deallocation is under the programmer’s direct control. You call `malloc`/`free` (or `new`/`delete`) to reserve and release memory, and the runtime will neither track nor garbage‑collect anything for you. This gives you maximum flexibility and minimal runtime overhead, but also puts the onus squarely on you to:

- **Match allocations and frees** exactly—miss one and you leak memory; double‑free and you invite crashes or security holes.  
- **Avoid dangling pointers**—using memory after it’s been freed leads to undefined behavior.  
- **Manage fragmentation**—small, scattered allocations can leave your heap full of unusable gaps over time.  

C++ smart pointers (e.g. `std::unique_ptr`, `std::shared_ptr`) build on these primitives with RAII (“Resource Acquisition Is Initialization”), automatically calling `delete` when an object goes out of scope. RAII dramatically reduces leaks and simplifies ownership, but it still relies on deterministic destructors rather than a background collector.

### Automatic Memory Management with Garbage Collection
Languages like Go, Java, and JavaScript shift the burden of reclaiming unused memory from the programmer to the runtime via a **garbage collector (GC)**. The core idea is:

1. **Allocation** on the heap is still explicit (`new`, `make`, etc.), but you never call `free`.  
2. **Roots** (global variables, stack variables, CPU registers) are scanned to find all live objects.  
3. **Mark phase**: reachable objects are “marked” as in‑use.  
4. **Sweep (or compact) phase**: unmarked objects are reclaimed or moved, freeing up memory automatically.  

This approach greatly simplifies application code—there’s no manual free, and many classes of bugs (dangling pointers, double‑free) vanish. However, it introduces:

- **Non‑deterministic latency**: GC pauses can interrupt your program at unpredictable times.  
- **Runtime overhead**: the collector needs CPU and memory to track object graphs and perform reclamation.  
- **Tuning complexity**: large heaps or long‑lived objects often require tuning generational strategies, pause‑time budgets, or concurrency settings.

---

