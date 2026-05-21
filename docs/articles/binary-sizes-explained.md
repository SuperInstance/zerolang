# Binary Sizes Explained: Why Are These Programs So Small?

The laman-rigidity proof compiles to 614 bytes. That's not a typo. Let's break down why.

## What's in a Binary?

An ELF executable on Linux has these components:

| Section | Purpose | Typical size |
|---|---|---|
| ELF header | File format metadata | 64 bytes |
| Program headers | Segment map for the loader | ~120 bytes |
| `.text` | Machine code | Varies |
| `.rodata` | String literals, constants | Varies |
| `.data` / `.bss` | Mutable globals | Varies |
| Section headers | Linker metadata (optional) | ~200 bytes |

A C "hello world" compiled with glibc is ~16 KB because it links against `libc.so`. A statically-linked C hello world is ~800 KB. A Rust hello world (static, release) is ~300 KB. A Go hello world is ~1.3 MB.

## Laman Rigidity: 614 Bytes

Let's look at the actual size breakdown from `zero size --json`:

```
Section breakdown:
  .text (code):      528 bytes
  .rodata (literals): 94 bytes
  stack (memory):     80 bytes
  debug metadata:      0 bytes

Function breakdown:
  edges_have:        104 bytes
  edges_needed:       92 bytes
  main:              236 bytes

Total:              614 bytes
```

**Why it's 614 bytes, not 16 KB:**

1. **No libc.** The binary doesn't link against glibc, musl, or any C library. The only OS interaction is a `write` syscall to fd 1 (stdout).

2. **No runtime.** No garbage collector, no thread pool, no exception unwinding tables, no type metadata at runtime.

3. **No heap.** All data lives on the 80-byte stack or in the 94 bytes of read-only string literals. The program never calls `malloc` or `mmap`.

4. **Aggressive dead code elimination.** The compiler only emits functions that are reachable from `main`. Unused standard library helpers are not included.

5. **Direct syscalls.** Instead of calling `printf("hello\n")` through libc's formatted I/O, zerolang writes the raw bytes directly via the `write(2)` syscall.

## All Package Sizes

| Package | Binary size | What it does |
|---|---|---|
| laman-rigidity | 614 bytes | Proves 12-neighbor fleets are always rigid |
| eisenstein-snap | 715 bytes | Verifies hexagonal lattice points |
| information-bound | 784 bytes | Computes log₂(48) ≈ 5.585 bits |
| constraint-solve | 989 bytes | Triangle inequality constraint check |
| pythagorean48 | 1,068 bytes | Counts 48 exact unit vectors |
| fleet-edges | 1,438 bytes | Edge counting for 5 fleet sizes |
| holonomy-consensus | 1,452 bytes | Ring topology consensus simulation |
| spline-interpolation | 1,470 bytes | Linear spline with 4 control points |
| triplet-loss | 2,265 bytes | Metric learning with Newton's sqrt |
| deadband-filter | 3,238 bytes | Noise filter over 10 signal values |

The smallest (laman-rigidity at 614 bytes) and largest (deadband-filter at 3,238 bytes) differ by 5×. The difference is purely code volume — deadband-filter processes 10 values with individual diff/comparison/print blocks.

## How Does This Compare?

| Language | Hello world (static) | Laman rigidity equivalent |
|---|---|---|
| zerolang | ~400 bytes | 614 bytes |
| C (static, musl) | ~16 KB | ~20 KB |
| Rust (static) | ~300 KB | ~350 KB |
| Go | ~1.3 MB | ~1.4 MB |
| Python | N/A (interpreter) | ~50 MB (env + deps) |

## What's the Catch?

Small binaries come with constraints:

- **No strings library.** You can write literal strings, but there's no `sprintf`, no `String.concat`, no regex. If you need formatted output, you construct it at compile time.

- **No heap allocation.** All data is fixed-size and stack-allocated. You can't grow a vector, allocate a linked list, or dynamically resize a buffer. (The stdlib has `std.mem.allocBytes` for programs that need it, but the proof packages don't.)

- **No floating point.** The proof packages use `i32` with ×1000 scaling. If you need `f64`, it's available as a type, but the math is straightforward — no SIMD, no vectorized operations.

- **Limited I/O.** Read from files, write to stdout/stderr, make HTTP requests. No databases, no GUIs, no network servers. The runtime is ~1 syscall away from bare metal.

- **Single-file programs.** Each package is one `main.0` file. There's no module system for splitting code across files (yet).

## Why Does This Work For Us?

Our mathematical proofs don't need any of those things. A Laman rigidity check is: loop from 3 to 100, compare `6n` vs `2n-2`, track a boolean. That's 236 bytes of machine code for the main function. The string literal "laman rigidity: all fleet sizes 3-100 are rigid with 12 neighbors\n" is 68 bytes. The ELF overhead is the rest.

The tradeoff is clear: you can't write a web server in zerolang (yet). But you can write a mathematical proof that compiles to less than 1 KB, and that proof is a static binary anyone can download and run in 0 seconds with 0 dependencies.
