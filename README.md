# Project Calliope: An Exploration of N-Bit Information Spaces in Rust

## Overview

Project Calliope is a research endeavor and software implementation aimed at exploring the fundamental nature of n-bit information spaces. It operates under the hypothesis that these finite, discrete spaces, when subjected to simple, deterministic transformation rules, reveal an inherent, emergent order and a complex, knowable architecture. This project seeks to map this architecture, understand its "physical laws" (transformation rules), and discover how symbolic meaning can be intrinsically represented and processed within this dynamic framework.

This Rust implementation focuses primarily on an **n=12 bit space** (4096 unique Information Units or IUs), which has served as the principal laboratory for initial investigations due to its balance between richness and analytical tractability. The core philosophy is "Meaning in Dynamics," where the significance of an IU or informational structure is profoundly defined by its dynamic behavior under transformation rules.

## Core Concepts of the ASC (Adaptive Symbolic Core) Model Implemented

This implementation embodies key components and principles of the Adaptive Symbolic Core (ASC) model:

*   **Information Unit (IU):** The fundamental quantum of information, an n-bit binary string (currently `u16` for n=12).
*   **Bitwise Operations:** Standard logic (AND, OR, XOR, NOT/Complement) and shifts (Circular, Arithmetic) form the basis of transformations.
*   **Transformation Rules:** Deterministic functions mapping IUs to new IUs.
    *   **Rule A (`T_A(X) = X XOR CLS(X, 1)`):** Extensively analyzed. For n=12, it deterministically leads all 4096 IUs into one of 24 distinct stable cycle basins. Key properties like `T_A(X) = T_A(~X)` and Invariant XOR Masks have been verified.
    *   **Rule B (`T_B(X) = X XOR (X >> 2)`):** Implemented for comparative dynamics, showing a vastly different state space organization (550 cycles for n=12).
*   **"Cell" Primitives:** Foundational computational units operating on IUs:
    *   Half-Adder, Full-Adder (bitwise parallel).
    *   N-bit Ripple Carry Adder.
    *   Bitwise Multiplexer (MUX) and Demultiplexer (DEMUX).
    *   Boolean Logic Cells (e.g., `F = (A & B) | (~A & C)`).
*   **Standard ASC Character Set (SACS-12):** A mapping from human-readable characters to 12-bit IUs. Currently implemented using direct ASCII/Unicode scalar values as IUs, with provisions for definitive user-defined mappings.
*   **Volume IU Digests (Enfolding):** A deterministic function (`rule_a` per char, then recursive `CLS(Acc,5) XOR tChar`) to derive a single n-bit IU from a sequence of character IUs, representing a compact "enfolded address" for text.
*   **Content Store (Unfolding):** Stores sequences of character IUs keyed by their Volume IU Digest, allowing for the "unfolding" of content from its enfolded address.
*   **Dynamic Relational Structures (DRSs):** Represented as `(Subject_IU, Relation_IU, Object_IU)` triplets.
    *   A `DrsStore` manages these.
    *   Supports basic pattern queries and **Basin-Level DRS Querying** (retrieving DRSs based on the Rule A attractor basin signature of their components).
*   **Simple ALU (Arithmetic Logic Unit):** A register-based processing unit (`SimpleAlu`) with:
    *   `NUM_REGISTERS` (currently 4) general-purpose IU registers.
    *   Opcodes for ADD, SUB, AND, OR, XOR, NOT, SHIFTs (Logical Left/Right, Arithmetic Right), ROTATE Left, LOAD Constant, MOVE Register.
    *   Support for both register-to-register and register-immediate operations.
    *   Produces Zero and Carry flags.
*   **Stateful Logic Simulation:** Demonstration of an N-bit SR Latch using bitwise primitives.
*   **ASC Scenario Runner (Rudimentary ISL):**
    *   `AscState` holding all major system components (ALU, Stores, Memory).
    *   `AscOp` enum defining a set of operations (ALU ops, memory ops, content/DRS ops, dynamic rule application, control flow).
    *   `run_scenario` function executes a sequence of `AscOp`s, simulating a simple Internal Symbolic Language program.
    *   Supports conditional jumps based on ALU flags and basic memory access (absolute and register-indirect).
    *   Includes a Canonical BigInt Index (CBI) tick counter.
*   **IU Discovery Experiments:** Demonstrations of how to search the n-12 space for IUs with specific dynamic properties, aiding in the selection of canonical SACS IUs or Relation IUs.

## Project Structure (Cargo Workspace)

The project is organized as a Cargo workspace with two main crates:

1.  **`calliope_core` (Library Crate):**
    *   Provides the absolute foundational elements: IU definition, bitwise utilities, transformation rules (`rule_a`, `rule_b`), dynamics analysis tools (trajectory tracing, landscape analysis, basin ID), and core arithmetic/logic "cell" primitives.
    *   Focuses on the n=12 space but designed with future genericity for `n` in mind.

2.  **`calliope_system` (Binary Crate):**
    *   Implements higher-level ASC components and services, utilizing `calliope_core`.
    *   Contains modules for: SACS, Volume IU Digest generation, Content Store, DRS Store, the Simple ALU, and the ASC Scenario Runner.
    *   Includes a `demos` module structure where various functionalities and experiments are showcased via `main.rs`.

## Current State & How to Run

*   The system compiles cleanly on Rust stable (ensure `edition = "2021"` in `Cargo.toml` files).
*   The `calliope_system` binary executes a series of demonstrations and experiments, printing detailed output to the console. This includes:
    *   Verification of Rule A properties.
    *   Demonstration of all implemented ALU primitives.
    *   Enfolding/Unfolding of text.
    *   DRS creation and querying (static and basin-level).
    *   SR Latch simulation.
    *   IU discovery experiments.
    *   Execution of a scripted scenario using the ASC Scenario Runner, showcasing ALU operations, memory access, and control flow.

**To Build and Run:**
1.  Navigate to the workspace root directory (`Calliope/`).
2.  Clean previous builds (optional but recommended): `cargo clean`
3.  Build in release mode: `cargo build --release`
4.  Run the main demonstration program: `cargo run -p calliope_system --release`

*(Unit tests for `calliope_core` and specific modules within `calliope_system` can be run with `cargo test --all` from the workspace root.)*

## Next Steps & Future Directions

*   **Definitive SACS-12 & Relation IU Integration:** Populate `sacs_data.rs` and `drs.rs` with canonical IU values based on Project Calliope research or further discovery experiments.
*   **Expand ISL/Scenario Runner:**
    *   Add more ALU opcodes (e.g., dedicated Compare operations).
    *   Enhance control flow (more jump conditions, loops, subroutines).
    *   Improve error handling.
*   **Persistent Storage:** Implement saving/loading for `ContentStore` and `DrsStore`.
*   **Wasm Integration:** Explore compiling core logic to WebAssembly for browser-based demonstrations or visualizations.
*   **Hierarchical Construction & Advanced Dynamics:** Further research into how lower-bit primitives influence higher-bit IU dynamics and how to construct symbols with desired dynamic properties.

Project Calliope aims to continue unveiling the dynamic architecture of N-bit information spaces, with this Rust implementation serving as a key tool for exploration, verification, and demonstration.
